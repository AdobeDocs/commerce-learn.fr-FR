---
title: 'PDC de paiement partagé : invite d’IA de l’extension de l’interface utilisateur d’Experience Cloud'
description: 'Découvrez comment utiliser cette invite facultative pour intégrer le paiement fractionné dans Commerce Admin : SDK de l’interface utilisateur d’administration, IMS, OAuth, accepter et refuser, ainsi que le script de simulation.'
feature: App Builder, Admin Workspace, Extensibility, Paas, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 192
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 7ea8492b082fb3f6e9ed7794526b0f83cb0481b3
workflow-type: tm+mt
source-wordcount: '496'
ht-degree: 0%

---

# PDC de paiement partagé : invite d’IA de l’extension de l’interface utilisateur d’Experience Cloud

Il s’agit de l’étape facultative qui incorpore un panneau d’ordres de paiement fractionnés dans le shell d’administration **[!UICONTROL Adobe Commerce]** (Experience Cloud) à l’aide de la `commerce-checkout-starter-kit` et du modèle de `commerce-backend-ui-1`. Le [tableau de bord de démonstration](split-payment-poc-app-builder-orchestrator-prompt.md) autonome de l’orchestrateur App Builder couvre le même flux d’acceptation et de refus sans intégration Admin Shell.

## Utiliser cette invite

Copiez tout du **INVITE DÉBUT** au **Fin de l&#39;invite** dans Curseur ou Claude. Exécutez l’invite à partir du répertoire `commerce-checkout-starter-kit/`.

## Avant d’exécuter

* Ce chemin d’accès nécessite des informations d’identification **IMS** en plus des valeurs OAuth (voir [Référence des variables d’environnement pour le paiement partagé : référence des variables d’environnement](split-payment-poc-env-reference.md) pour les variables de `commerce-checkout-starter-kit`).
* Terminez [Split Payment POC : invite de l’IA de l’orchestrateur d’App Builder](split-payment-poc-app-builder-orchestrator-prompt.md) tout d’abord si vous souhaitez comparer le même `payment-accept` et `payment-decline` comportement ; l’extension d’interface utilisateur réutilise cette logique avec les noms d’environnement de `COMMERCE_INTEGRATION_*`.


## L’invite

**DÉBUT DE L’INVITE**


Vous générez l’extension SDK de l’interface utilisateur d’administration `commerce-backend-ui-1` pour la preuve de concept de paiement partagé. Cette extension incorpore un tableau de bord des ordres de paiement fractionnés dans Adobe Commerce Admin Shell à l’aide des modèles `@adobe/aio-app-dev-toolkit` et `@adobe/commerce-backend-ui-1` du kit de démarrage de passage en caisse Commerce.

**Projet de base :** `commerce-checkout-starter-kit/`
**Répertoire d’extension :** `commerce-checkout-starter-kit/commerce-backend-ui-1/`


### Ce Que Fournit Cette Extension

1. **Panneau Grille des commandes d’administration** — un élément de menu personnalisé dans Commerce Admin Shell répertoriant les ordres de paiement fractionnés avec `split_cash_status = 'pending'`
2. **Vue détaillée de la commande** — affiche la répartition des paiements fractionnés (montant en espèces, montant du crédit de la boutique, statut) avec la commande
3. **Accepter/Refuser les actions** : boutons qui appellent les actions `payment-accept` et `payment-decline` App Builder via OAuth 1.0a.


### Structure de fichier à générer

```
commerce-checkout-starter-kit/commerce-backend-ui-1/
├── ext.config.yaml
├── README.md
├── .env.simulation.example
├── actions/
│   ├── utils.js
│   ├── commerce/
│   │   └── index.js           ← IMS-based Commerce REST (order listing)
│   ├── payment-accept/
│   │   ├── commerce-client.js ← OAuth 1.0a (accept/decline)
│   │   └── index.js
│   ├── payment-decline/
│   │   └── index.js
│   └── registration/
│       └── index.js
├── scripts/
│   ├── README.md
│   └── simulate-split-payment.mjs
└── web-src/
    ├── index.html
    ├── package.json
    ├── .parcelrc
    └── src/
        ├── index.jsx
        ├── index.css
        ├── utils.js
        ├── exc-runtime.js
        ├── config.json
        ├── constants/
        │   └── extension.js
        ├── components/
        │   ├── App.jsx
        │   ├── ExtensionRegistration.jsx
        │   ├── MainPage.jsx
        │   ├── SplitPaymentDashboard.jsx
        │   └── SplitPaymentOrderDetail.jsx
        └── hooks/
            └── useSplitPaymentOrders.js
```


### Actions du serveur principal

**`actions/commerce/index.js`** — REST Commerce authentifié IMS
* Utilise le jeton IMS fourni par le contexte SDK de l’interface utilisateur d’administration pour appeler le REST Commerce.
* Récupère la liste des commandes avec `split_cash_status` filtre
* Renvoie la liste de commandes au format JSON

**`actions/payment-accept/commerce-client.js`** — Client OAuth 1.0a
* Même implémentation que `split-payment-orchestrator/actions/payment-orchestrator/commerce-client.js`
* Utilise `COMMERCE_INTEGRATION_*` variables d’environnement préfixées (pour se distinguer des informations d’identification IMS)

**`actions/payment-accept/index.js`** — Accepter l&#39;action
* Même logique que `split-payment-orchestrator/actions/payment-accept/index.js`
* Appels `POST /V1/split-payment/orders/:orderId/cash-received` via OAuth 1.0a

**`actions/payment-decline/index.js`** — Refuser l&#39;action
* Appels `POST /V1/split-payment/orders/:orderId/cash-decline`

**`actions/registration/index.js`** — Enregistrement SDK de l’interface utilisateur d’administration
* Enregistre l’extension avec Commerce Admin Shell
* Ajoute un élément de menu sous Commandes pour le tableau de bord de paiement fractionné


### Composants front-end React

**`SplitPaymentDashboard.jsx`**
* Répertorie les ordres de paiement fractionnés en attente dans une table de style Spectrum
* Colonnes : N° de commande (increment_id), Date, Client, Argent dû, Crédit de la boutique, Statut
* Boutons Accepter et Refuser par ligne
* Appelle les actions du serveur principal via les assistants de récupération `web-src/src/utils.js`
* Affiche les états de chargement/erreur ; s’actualise à la fin de l’action

**`SplitPaymentOrderDetail.jsx`**
* Affiche les détails de paiement fractionné pour une seule commande
* Affichages : montant de trésorerie, montant du crédit de magasin, statut_split_cash_actuel

**`useSplitPaymentOrders.js`** : hook React
* Récupère les ordres de paiement fractionnés à partir de `actions/commerce/index.js`
* Renvoie `{ orders, loading, error, refresh }`


### Script de simulation

**`scripts/simulate-split-payment.mjs`**

A Node.js ESM script for testing Commerce REST calls directly (without going through App Builder). Uses the same OAuth 1.0a signing as the App Builder actions.

Commands:
* `node simulate-split-payment.mjs help` — show usage
* `node simulate-split-payment.mjs list` — list recent orders with split payment data
* `node simulate-split-payment.mjs show <orderId>` — show split payment fields for a specific order (entity_id)
* `node simulate-split-payment.mjs accept <orderId>` — call `cash-received` endpoint
* `node simulate-split-payment.mjs decline <orderId>` — call `cash-decline` endpoint

Reads credentials from `commerce-backend-ui-1/.env.simulation` (copy from `.env.simulation.example`).

**`.env.simulation.example`:**

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


### `ext.config.yaml`

Configure the extension with:
* The `commerce-backend-ui-1` extension type
* The four backend actions (`commerce`, `payment-accept`, `payment-decline`, `registration`)
* `require-adobe-auth: true` for all actions except `registration`
* Input bindings from env for `COMMERCE_INTEGRATION_*` credentials


### Après la génération des fichiers

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

### Fin de l&#39;invite


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
