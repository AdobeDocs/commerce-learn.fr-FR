---
title: Guide de mise en œuvre étape par étape de la PDC de paiement fractionné
description: Découvrez comment mettre en œuvre la PDC de paiement partagé une fois la configuration terminée, en couvrant le module, les environnements, l’orchestrateur, la vérification et l’interface utilisateur facultative.
feature: App Builder, Eventing, Extensibility
topic: Commerce, Architecture, App Builder, Development
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 480
jira: KT-20902
last-substantial-update: 2026-04-30T00:00:00Z
source-git-commit: 27811c05f066c95a60ac309472d6488295fb2c96
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 0%

---

# Guide de mise en œuvre étape par étape de la PDC de paiement fractionné

Cette page présente la liste de contrôle de l’implémentation. Cela suppose que vous avez déjà visionné la [présentation](./overview.md) et la [démonstration complète](./full-demo.md) et que votre site Commerce et votre projet App Builder sont tous deux prêts. Les pages de tutoriel individuelles contiennent des détails complets sur chaque sujet ci-dessous : utilisez cette page pour savoir ce que vous devez faire et dans quel ordre.

## Avant de commencer

Confirmez tous les éléments suivants avant d’exécuter une invite ou une commande. La page [ Conditions préalables et configuration ](./prerequisites-and-setup.md) couvre chaque élément en détail.

**côté**

* [ ] Adobe Commerce 2.4.5 ou une version ultérieure
* [ ] les événements d’E/S activés et déployés (Commerce Cloud uniquement — nécessite des `ENABLE_EVENTING: true` dans `.magento.env.yaml`)
* [ ] **Contre remboursement** activé dans Admin avec son titre défini exactement sur `Cash`
* [ Crédit de la boutique ] activé dans l’administrateur
* [ ] client Test a un crédit de magasin d’au moins 50 $
* [ ] intégration Commerce a été créée, activée et les quatre valeurs OAuth ont été enregistrées en toute sécurité

**côté**

* [ ] projet App Builder existe dans Adobe Developer Console
* [ ] service Adobe I/O Events ajouté à l’espace de travail
* [ ] Commerce connecté en tant que fournisseur d’événements
* [ ] `aio login` terminé ; corriger l’espace de travail sélectionné avec `aio app use`
* [ ] Node.js 18 ou version ultérieure installé ; `aio` l’interface en ligne de commande installée (`npm install -g @adobe/aio-cli`)

S&#39;il manque quelque chose au-dessus, arrêtez-vous ici et terminez-le d&#39;abord.

## Phase 1 : création du module Commerce

**Fonction :** génère le module PHP `Client_SplitPayment` qui gère l’interface utilisateur de passage en caisse, l’application de crédit de magasin, les points d’entrée REST et l’abonnement à l’événement I/O. Il s’agit de l’adaptateur fin in-Commerce : tous les workflows de l’opérateur restent dans App Builder.

### Étape 1.1 — Personnaliser l&#39;invite pour votre projet

Ouvrez la page d’invite d’IA du module Commerce [](./commerce-module-prompt.md) et notez les points suivants avant de copier l’invite :

* Remplacez `Client` par le nom réel du fournisseur à l’invite
* Remplacez `SplitPayment` si vous souhaitez un autre nom de module
* Si votre thème n’est pas Luma, il se peut que le chemin d’injection `LayoutProcessorPlugin` et les mappages RequireJS doivent être ajustés
* Si votre méthode d&#39;encaissement à la livraison utilise un code autre que `cashondelivery`, mettez à jour `payment-method-helper.js`

### Étape 1.2 — Exécuter l&#39;invite

Copiez l&#39;invite complète de **INVITE DÉBUT** à **Fin de l&#39;invite** et collez-la dans Cursor (avec Claude) ou directement dans Claude. Exécutez-le à partir de la racine de votre projet Commerce afin que l’IA puisse créer des fichiers sous `app/code/`.

### Étape 1.3 — Activer et installer le module

Exécutez les commandes suivantes à partir de la racine de votre projet Commerce :

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### Étape 1.4 — Vérifier que le module est correctement installé

```bash
# Confirm the module is active
bin/magento module:status Client_SplitPayment

# Confirm the split_ columns were added to sales_order
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

Vous devriez voir cinq colonnes : `split_store_credit_amount`, `split_cash_amount`, `split_cash_status`, `split_sc_invoice_id` et `split_cash_invoice_id`.

Vérifiez ensuite dans un navigateur que les champs de paiement fractionné s&#39;affichent lors du passage en caisse lorsqu&#39;un client connecté avec un crédit de magasin sélectionne **Trésorerie** comme mode de paiement.

## Phase 2 - Configurer les variables d’environnement

**Fonction :** prépare les fichiers d’informations d’identification utilisés par l’orchestrateur App Builder, le script de simulation et (éventuellement) l’extension de l’interface utilisateur Experience Cloud. Les quatre mêmes valeurs OAuth de votre intégration Commerce sont utilisées dans chaque fichier `.env`.

Reportez-vous à la [référence des variables d’environnement](./env-reference.md) pour la liste complète des variables par composant.

### Étape 2.1 — Créer le `.env` de l&#39;orchestrateur

Dans votre répertoire `split-payment-orchestrator/` :

```bash
cp .env.example .env
```

Renseignez chaque valeur :

| Variable | Où l&#39;obtenir |
|---|---|
| `COMMERCE_BASE_URL` | URL de votre boutique — pas de barre oblique |
| `COMMERCE_CONSUMER_KEY` | Commerce Admin > Système > Intégrations |
| `COMMERCE_CONSUMER_SECRET` | Identique : affiché uniquement lors de l’activation |
| `COMMERCE_ACCESS_TOKEN` | Identique |
| `COMMERCE_ACCESS_TOKEN_SECRET` | Identique |
| `PAYMENT_THRESHOLD` | Doit correspondre à `split_payment/general/threshold` dans Commerce (valeur par défaut : `100`). |
| `DEMO_UI_SECRET` | Facultatif — si défini, obligatoire comme `?secret=` dans l&#39;URL du tableau de bord |

### Etape 2.2 — Créer le script de simulation `.env`

```bash
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
```

Renseignez `COMMERCE_BASE_URL` et les quatre valeurs OAuth (mêmes informations d’identification que ci-dessus).

## Phase 3 : création de l’orchestrateur App Builder

**Fonction :** génère le projet App Builder `split-payment-orchestrator` : le consommateur d’événement d’E/S `payment-orchestrator`, les actions web `payment-accept` et `payment-decline`, et l’interface utilisateur de l’opérateur `demo-dashboard`.

### Étape 3.1 — Exécuter l&#39;invite de l&#39;orchestrateur

Ouvrez la page [Invite de l’IA de l’orchestrateur d’](./orchestrator-prompt.md). Copiez l’invite complète et exécutez-la à partir du répertoire `split-payment-orchestrator/` .

### Étape 3.2 — Installation des dépendances et déploiement

```bash
cd split-payment-orchestrator
npm install
# Confirm .env is filled in (from Phase 2, Step 2.1)
aio app deploy
```

Après le déploiement, `aio app deploy` imprime les URL d’action. Notez l’URL `demo-dashboard` ; vous en avez besoin à la phase 4.

### Étape 3.3 — Confirmer l&#39;inscription à l&#39;événement

Dans Adobe Developer Console, ouvrez l’espace de travail de votre projet App Builder. Sous **Événements**, vérifiez que l’enregistrement `Split payment — sales order place before` existe et est lié à l’action `payment-orchestrator`.

## Phase 4 — Tester le flux complet

Suivez les étapes de vérification dans l’ordre. Le [guide de test et de vérification](./testing-and-verification.md) contient les commandes de curl complètes, l’utilisation du script de simulation et une référence de dépannage pour chaque étape ci-dessous.

### Étape 4.1 — Vérifier que les points d’entrée REST sont routables

```bash
# Expect 401, not 404 — the endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received
```

### Étape 4.2 - Tester l’interface utilisateur de passage en caisse

1. Connectez-vous au storefront en tant que client de test (celui avec le crédit du magasin).
2. Ajouter un produit au panier avec un total inférieur à 100 $ après expédition et taxe
3. À l’étape de paiement, sélectionnez **Trésorerie**
4. Confirmez que les champs de paiement fractionné s&#39;affichent : solde affiché, espèces pré-remplies, crédit en magasin à 0 $
5. Réduisez le montant en espèces et vérifiez que la partie crédit de magasin est correctement mise à jour

### Étape 4.3 — Tester la protection du seuil

Ajoutez des produits totalisant plus de 100 $, passez en caisse, sélectionnez Argent et tentez de passer la commande. Le `"Payment could not be processed. Please try again or contact support."` d’erreur doit apparaître.

### Étape 4.4 — Émettre un ordre de paiement fractionné de test

Créez un panier de moins de 100 $, saisissez un fractionnement de crédit au comptant/magasin et passez la commande. Dans Commerce Admin, confirmez :

* Le statut de la commande est `pending_payment`
* Deux commentaires d’App Builder sont visibles sur la commande :
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."`
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."`

Si aucun commentaire App Builder n’apparaît, consultez les journaux avec `aio app logs` avant de continuer.

### Étape 4.5 — Acceptation du test via le script de simulation

```bash
# Find your order's entity_id
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Accept the payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept <entity_id>
```

Dans Commerce Admin, confirmez :

* Statut de la commande modifié en `processing`
* Commentaire sur l&#39;historique : `"Cash payment of $X.XX received."`
* Facture de caisse visible dans l&#39;onglet Factures
* Expédition visible dans l&#39;onglet Expéditions (le cas échéant)

### Étape 4.6 — Essai de déclin par script de simulation

Placez un autre ordre de test, puis :

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <entity_id>
```

Confirmez que le statut de la commande est `canceled` et que `split_cash_status` est `declined`.

### Étape 4.7 — Tester le tableau de bord de démonstration

Ouvrez l’URL du tableau de bord à partir de l’étape 3.2. Avec une commande en attente dans le système :

1. Confirmer que l’ordre apparaît dans la liste en attente
2. Cliquez sur **Accepter** et confirmez que l’ordre passe à `processing` dans Commerce
3. Passez une nouvelle commande, revenez au tableau de bord, puis cliquez sur **Refuser** — confirmer l&#39;annulation

## Phase 5 (facultatif) : ajoutez l’extension de l’interface utilisateur d’Experience Cloud

Cette phase incorpore le tableau de bord de l’opérateur dans le shell d’administration Commerce au lieu d’utiliser le `demo-dashboard` autonome. Passez cette phase si le tableau de bord autonome répond à vos besoins.

### Étape 5.1 — Revoir les prérequis

L’extension d’interface utilisateur Experience Cloud requiert des informations d’identification IMS en plus des valeurs d’intégration OAuth. Lisez la page [Invite de l’IA de l’extension de l’interface utilisateur d’](./experience-cloud-ui-prompt.md) avant d’exécuter l’invite, en prêtant attention à la `OAUTH_CLIENT_ID` et aux variables IMS associées.

### Étape 5.2 — Exécuter l’invite d’extension de l’interface utilisateur

Copiez l’invite complète depuis **INVITE DE DÉBUT** vers **Fin de l’invite** et exécutez-la à partir du répertoire `commerce-checkout-starter-kit/`.

### Étape 5.3 — Déploiement

```bash
cd commerce-checkout-starter-kit
npm install
cp .env.dist .env
# Fill in IMS credentials and COMMERCE_INTEGRATION_* credentials
aio app deploy
```

## Référence de dépannage rapide

| Symptôme | Première chose à vérifier |
|---|---|
| Les champs de paiement fractionné n&#39;apparaissent pas lors du passage en caisse | Erreurs RequireJS dans la console du navigateur ; s’exécute également `bin/magento cache:flush` |
| `"Payment could not be processed"` à la commande | `PlaceOrderPlugin` ou `BalanceManagementInterface` — ajout d&#39;une journalisation de débogage temporaire dans `PlaceOrderPlugin` |
| Aucun commentaire App Builder sur la commande | Exécutez `aio app logs` pour voir si l’événement s’est déclenché et ce que l’action a renvoyé |
| `403` à partir de Commerce REST dans App Builder | Fastly IP - placée sur la liste autorisée en route : testez d’abord le script de simulation ; si cela fonctionne, le problème est le blocage des adresses IP en sortie |
| `"The signature is invalid"` depuis Commerce | Confirmez `COMMERCE_BASE_URL` n’a pas de barre oblique ; confirmez que l’intégration est activée |
| Les commandes n’apparaissent pas dans le tableau de bord de démonstration | Vérifiez que `extension_attributes.split_cash_status` apparaît dans une réponse `GET /rest/V1/orders` directe |

Pour obtenir des informations complètes sur le dépannage, consultez le guide [test et vérification](./testing-and-verification.md#common-issues-and-fixes).

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
