---
title: 'PDC de paiement partagé : invite de l’IA de l’orchestrateur App Builder'
description: Découvrez comment utiliser cette invite pour créer l’application split-pay-orchestrator. Événements I/O, orchestrateur de paiement, actions web, tableau de bord de démonstration et déploiement d’applications en mode automatique.
feature: App Builder, Configuration, Eventing, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 421
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 8dfbf2694378aae76c91afa11bfee7d93077d8ba
workflow-type: tm+mt
source-wordcount: '927'
ht-degree: 0%

---

# PDC de paiement partagé : invite de l’IA de l’orchestrateur App Builder

Utilisez cette page pour copier l’invite complète qui génère le projet **split-pay-orchestrator** : le **pay-orchestrator** consommateur d’événement d’E/S, **pay-accept** et **pay-deny** actions web, le **demo-dashboard** et le client REST Commerce.

## Utiliser cette invite

Copiez tout du **DÉBUT INVITE** au **Fin de l&#39;invite** dans Cursor (avec Claude) ou directement dans Claude. Exécutez l’invite à partir du répertoire `split-payment-orchestrator/` (racine du projet App Builder).

## Avant d’exécuter

* Terminer [preuve de concept de paiement partagé : conditions préalables et configuration de l’environnement](./prerequisites-and-setup.md).
* Préparez votre [preuve de concept de paiement partagé : référence des variables d’environnement](./env-reference.md) et `.env` fichier dans le projet.


## L’invite

**DÉBUT DE L’INVITE**


Vous générez une application Adobe App Builder complète pour orchestrer les paiements partagés dans Adobe Commerce. Cette application reçoit les événements d’E/S de Commerce, traite les décisions de paiement fractionné et appelle de nouveau Commerce via REST.

**Projet:** `split-payment-orchestrator`
**Runtime:** Node.js 18
**Dépendances clés :** `@adobe/aio-sdk ^6.0.0`, `got ^11.8.6`, `oauth-1.0a ^2.2.6`

Générez tous les fichiers répertoriés ci-dessous. L’application doit fonctionner avec Adobe I/O Runtime (`aio app deploy`).


### Structure de fichier à générer

```
split-payment-orchestrator/
├── package.json
├── app.config.yaml
├── .env.example
└── actions/
    ├── payment-orchestrator/
    │   ├── index.js         ← I/O Event entry point
    │   ├── commerce-client.js
    │   ├── threshold.js
    │   ├── cash-payment.js
    │   ├── order-update.js
    │   └── store-credit.js  ← Deprecated stub; kept for reference
    ├── payment-accept/
    │   └── index.js
    ├── payment-decline/
    │   └── index.js
    └── demo-dashboard/
        └── index.js
```


### `package.json`

```json
{
  "name": "split-payment-orchestrator",
  "version": "1.0.0",
  "private": true,
  "description": "Adobe App Builder action — split payment PoC orchestrator",
  "engines": { "node": ">=18" },
  "scripts": {
    "deploy": "NODE_OPTIONS=--disable-warning=DEP0040 aio app deploy",
    "aio": "NODE_OPTIONS=--disable-warning=DEP0040 aio"
  },
  "dependencies": {
    "@adobe/aio-sdk": "^6.0.0",
    "@adobe/exc-app": "^1.5.9",
    "got": "^11.8.6",
    "oauth-1.0a": "^2.2.6"
  }
}
```


### `app.config.yaml`

Définissez quatre actions dans le package `split_payment_orchestrator` :

**`payment-orchestrator`**
* `web: "no"` (déclencheur d’événement d’E/S uniquement - non directement appelable via HTTP)
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* Entrées de env : `LOG_LEVEL`, `COMMERCE_BASE_URL`, `COMMERCE_CONSUMER_KEY`, `COMMERCE_CONSUMER_SECRET`, `COMMERCE_ACCESS_TOKEN`, `COMMERCE_ACCESS_TOKEN_SECRET`, `PAYMENT_THRESHOLD`

**`payment-accept`**
* `web: "yes"` (action web HTTP - appelable par un tableau de bord ou un ERP)
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* Mêmes entrées d’informations d’identification Commerce (aucune `PAYMENT_THRESHOLD`)

**`payment-decline`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: true`, `final: true`
* Mêmes entrées d’informations d’identification Commerce

**`demo-dashboard`**
* `web: "yes"`
* `runtime: nodejs:18`
* `require-adobe-auth: false` tableau de bord ← est accessible au public (protégé uniquement par des `DEMO_UI_SECRET` s’il est défini)
* Entrées : toutes les informations d’identification Commerce + `DEMO_UI_SECRET`, `DEMO_UI_BASE_URL`

**Enregistrement des événements** (sous `events.registrations`) :

```yaml
Split payment — sales order place before:
  description: Invokes payment-orchestrator when an order is about to be placed
  events_of_interest:
    - provider_metadata: dx_commerce_events
      event_codes:
        - com.adobe.commerce.observer.sales_order_place_before
  runtime_action: split_payment_orchestrator/payment-orchestrator
```


### `actions/payment-orchestrator/commerce-client.js`

Client REST OAuth 1.0a partagé pour Adobe Commerce. Implémente :

**`createCommerceClient(params, logger)`** — renvoie `{ request, baseUrl }`

* Lit `COMMERCE_BASE_URL`, `COMMERCE_CONSUMER_KEY`, `COMMERCE_CONSUMER_SECRET`, `COMMERCE_ACCESS_TOKEN`, `COMMERCE_ACCESS_TOKEN_SECRET` à partir de `params`
* Renvoie les informations d’identification manquantes
* Utilise `oauth-1.0a` avec `HMAC-SHA256` (`crypto.createHmac` Node.js)
* Utilise `got@11` (non `got@12+` — le projet utilise CJS) avec `prefixUrl = ${baseUrl}/rest/V1/`
* Ajoute `Authorization` en-tête via `beforeRequest` hook
* `request(method, path, options)` : normalise le tracé (délimite les `/`), renvoie `{ statusCode, body }`
* `throwHttpErrors: false` — ne lance jamais sur 4xx/5xx ; renvoie toujours le code d&#39;état


### `actions/payment-orchestrator/threshold.js`

**`evaluateThreshold({ orderTotal, storeCreditAmount, cashAmount, params, logger })`** — renvoie `{ pass: boolean, reason: string }`

Logique :
1. Lecture `params.PAYMENT_THRESHOLD` ; analyse en tant que flottant ; valeur par défaut `100` en cas d’absence, NaN ou ≤ 0
2. Si `orderTotal > threshold` : renvoi `{ pass: false, reason: 'PAYMENT_THRESHOLD_EXCEEDED' }`
3. Si `Math.abs((storeCreditAmount + cashAmount) - orderTotal) > 0.02` : renvoi `{ pass: false, reason: 'SPLIT_AMOUNT_MISMATCH' }`
4. Sinon : `{ pass: true, reason: '' }` retour


### `actions/payment-orchestrator/cash-payment.js`

**`recordCashPending({ commerce, orderId, cashAmount, logger })`** — renvoie `{ ok: boolean, error? }`

* Appels `POST orders/${orderId}/comments` avec :

  ```json
  {
    "statusHistory": {
      "comment": "Cash payment of $X.XX pending. Awaiting admin confirmation.",
      "entity_name": "order",
      "parent_id": "<orderId>",
      "is_visible_on_front": true,
      "is_customer_notified": false,
      "status": "pending_payment"
    }
  }
  ```

* Renvoie `{ ok: true }` sur 2xx ; renvoie `{ ok: false, error: { code, message } }` dans le cas contraire
* Enveloppe dans try/catch ; renvoie l&#39;objet error, ne lance jamais


### `actions/payment-orchestrator/order-update.js`

**`updateOrderAfterOrchestration({ commerce, orderId, success, detail, logger })`** — renvoie `{ ok: boolean, error? }`

* Si `success` : publie un commentaire d&#39;historique `"Split payment orchestration completed. Order awaiting cash confirmation."`
* Si `!success` : publie des `"Payment could not be processed. Please try again or contact support."` — n’incluez jamais de `detail` dans le commentaire visible par le client ; `detail` uniquement en interne
* Renvoie `{ ok: boolean, error? }` — ne renvoie jamais


### `actions/payment-orchestrator/store-credit.js`

**`applyStoreCredit({ commerce, cartId, amount, logger })`** : mise en œuvre obsolète sans opération

Incluez ce fichier avec un avis de `@deprecated` JSDoc expliquant :
> L’orchestrateur n’applique plus de crédit de magasin via REST. Le crédit de magasin est appliqué lors du passage en caisse dans le module Commerce PHP (`PlaceOrderPlugin`) à l’aide de `BalanceManagementInterface::apply()`, qui nécessite un panier actif. Lorsque App Builder reçoit l’événement d’E/S, le panier est inactif. Ce fichier est conservé à titre de référence ou pour les flux personnalisés où le crédit de magasin est appliqué après la commande.

Conservez une implémentation fonctionnelle (la même forme que les autres modules) afin que les développeurs puissent étudier le modèle, mais marquez-le clairement comme n’étant pas utilisé dans le flux actuel.


### `actions/payment-orchestrator/index.js`

Point d’entrée des événements I/O. Implémente `async function main(params)`.

**Extraction de la payload :**

Les événements d’Adobe Commerce I/O peuvent diffuser la payload sous plusieurs formes. Extrayez l’objet de valeur d’ordre en vérifiant ces chemins dans l’ordre :
1. `params.__ow_body` (analyse de la chaîne JSON si nécessaire) → `.event.data.value`
2. `params.data.value`
3. `params.value`
4. `params.body.event.data.value`
5. Retour à la `params` elle-même

**Extraction du champ à partir de la valeur :**
* `orderId = value.entity_id || value.id`
* `orderTotal = parseFloat(value.grand_total ?? value.base_grand_total ?? value.subtotal ?? '0')`
* Fractionner les montants de `value.extension_attributes` (cochez à la fois `extension_attributes` et `extensionAttributes`) :
   * `storeCredit = parseFloat(ext.split_store_credit_amount ?? value.split_store_credit_amount ?? '0')`
   * `cash = parseFloat(ext.split_cash_amount ?? value.split_cash_amount ?? '0')`

**Flux:**
1. Extraire la valeur ; si la `orderId` est manquante, consigner l’erreur et renvoyer la `{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`
2. Appeler `evaluateThreshold(...)` — si échoue, consigner et renvoyer la même réponse 200
3. Appel `createCommerceClient(params, logger)` — si échoue, retourne une erreur 200
4. Si `storeCredit > 0`, consignez que le crédit de magasin a été appliqué lors du passage en caisse (aucun appel REST nécessaire).
5. Appeler `recordCashPending(...)` : en cas d’échec, appeler `updateOrderAfterOrchestration(..., success: false)` et renvoyer l’erreur 200
6. `updateOrderAfterOrchestration(..., success: true)` d’appel
7. `{ statusCode: 200, body: { ok: true, message: 'processed' } }` de retour

**Important :** toujours renvoyer `statusCode: 200` — I/O Runtime réessaiera les réponses non-200, ce qui entraînerait un traitement des commandes en double. Des erreurs sont signalées dans le corps.

**`PUBLIC_ERROR`constante :** `"Payment could not be processed. Please try again or contact support."` — utilisée pour tous les messages d&#39;erreur orientés vers l&#39;extérieur.


### `actions/payment-accept/index.js`

Action web HTTP. Appels `POST /V1/split-payment/orders/:orderId/cash-received`.

**Résolution de l’ID de commande :** cochez `params.orderId`, puis `params.payload.orderId` et `params.__ow_body` (JSON analysé). Renvoie 400 en cas d’absence.

**Flux:**
1. Résoudre les `orderId` ; renvoyer la valeur 400 si elle est manquante
2. Client Commerce init ; renvoie 500 si échoue
3. Appeler `POST split-payment/orders/${orderId}/cash-received` avec un corps JSON vide
4. Si 2xx : `{ statusCode: 200, body: { ok: true, orderId, message: 'accepted' } }` retour
5. En cas d’erreur : consigner et renvoyer les `{ statusCode: 200, body: { ok: false, message: PUBLIC_ERROR } }`


### `actions/payment-decline/index.js`

Action web HTTP. Même modèle que `payment-accept`, mais appelle `POST /V1/split-payment/orders/:orderId/cash-decline`.

Renvoyer la `{ ok: true, orderId, message: 'declined' }` en cas de succès.


### `actions/demo-dashboard/index.js`

Tableau de bord autonome de l’opérateur de démonstration. Fournit un tableau de bord HTML pour répertorier les commandes en espèces en attente et déclencher des actions d’acceptation/refus. Il s’agit d’une action web unique qui sert à la fois l’interface utilisateur d’HTML et une API JSON.

**Sécurité:**
* Vérification `DEMO_UI_SECRET` facultative : si elle est définie, exigez `?secret=<value>` paramètre de requête ou un en-tête de `x-demo-secret` pour toutes les requêtes. Renvoie 401 si manquant/erroné.
* Enregistrer un avertissement si `DEMO_UI_SECRET` n’est pas défini (tableau de bord non protégé)

**Routage (basé sur la méthode HTTP + chemin/corps) :**

L’action reçoit toutes les demandes. Déterminez l’intention à partir de :
* `params.__ow_method` (GET/POST) et `params.__ow_path` ou paramètres d’action
* `GET` sans action → le tableau de bord HTML
* `GET` avec `action=list` paramètre → renvoyer la liste JSON des commandes en attente
* `POST` avec `action=accept` et `orderId` → appeler `payment-accept` logique (ou incorporer l’appel REST Commerce)
* `POST` avec `action=decline` et `orderId` → appel `payment-decline` logique

**Récupération des commandes en attente :**
* `GET orders?searchCriteria[pageSize]=50` d’appel à partir du REST Commerce
* Filtrez les commandes pour lesquelles `extension_attributes.split_cash_status === 'pending'` commande ET n’est pas dans un état terminal (`complete`, `closed`, `canceled`, `cancelled`).
* Trier le plus récent en premier en mémoire
* Renvoie jusqu’à 20 (configurable via le paramètre `limit`)

**Tableau de bord HTML :**
Le tableau de bord est une chaîne HTML renvoyée avec `Content-Type: text/html`. Il devrait :
* Répertorier les commandes en espèces en attente dans une table : n° de commande (increment_id), entity_id, nom du client, montant en espèces, montant du crédit de magasin, date
* Fournissez des boutons **Accepter** et **Refuser** pour chaque commande qui appelle le point d’entrée de l’API de l’action
* Afficher les réponses de succès/erreur intégrées
* Inclure un bouton d’actualisation
* être suffisamment stylisés pour être utilisables (un CSS minimal est correct ; aucune dépendance de réseau CDN externe pour éviter des problèmes CORS au moment de l’exécution).
* Afficher une bannière d’avertissement si `DEMO_UI_SECRET` n’est pas défini

**Erreur lors de l’affichage dans l’interface utilisateur :**
Lorsqu’un appel REST Commerce échoue, incluez le statut HTTP et une brève description du corps de l’erreur Commerce (sanitize — supprimer les balises HTML, tronquer à 500 caractères). N’exposez pas les informations d’identification.

**Assistants de réponse :**

```javascript
function jsonResponse(statusCode, obj, extraHeaders = {}) { ... }
function htmlResponse(html) { ... }
```


### `.env.example`

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=

# OAuth 1.0a integration credentials (Admin → System → Integrations)
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# PoC threshold — must match split_payment/general/threshold in Commerce (default: 100)
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard optional shared secret
DEMO_UI_SECRET=
DEMO_UI_BASE_URL=
```


### Déployer la commande

Après avoir généré tous les fichiers, à partir du répertoire `split-payment-orchestrator/` :

```bash
npm install
cp .env.example .env
# Edit .env with your credentials
aio app deploy
```

Après le déploiement, notez les URL d’action imprimées par `aio app deploy`. L’URL `demo-dashboard` vous permet d’accéder au tableau de bord de l’opérateur.


### Fin de l&#39;invite


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
