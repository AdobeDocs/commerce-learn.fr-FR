---
title: 'PdC de paiement fractionné : prérequis et configuration de l''environnement'
description: Découvrez comment configurer Commerce, Admin pour le COD et le crédit de magasin, l’intégration OAuth, les événements I/O, App Builder et l’interface de ligne de commande aio avant les invites de création de paiement partagé.
feature: App Builder, Configuration, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 262
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: d5f1e76c3a5127698f2933810fca218b79082571
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 1%

---

# PdC de paiement fractionné : prérequis et configuration de l&#39;environnement

Suivez toutes les étapes de ce tutoriel avant d’exécuter l’une des invites de build. La raison la plus courante de l’interruption du flux en milieu de tutoriel est qu’une seule étape n’est pas renseignée.

## &#x200B;1. Configuration requise pour Adobe Commerce

* Adobe Commerce **2.4.5 ou version ultérieure** (sur site ou Commerce Cloud)
* Accès Git au projet Commerce (vous ajoutez un module sous `app/code/`)
* Accès à **[!UICONTROL Commerce Admin]**

### Commerce Cloud uniquement : activer les événements I/O

Ajoutez les éléments suivants à `.magento.env.yaml` et déployez-les avant d’ajouter le module :

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

> **Avertissement :** ce déploiement doit se terminer correctement avant que la dépendance du module Événement I/O puisse être résolue.


## &#x200B;2. Configuration d’administration de Commerce

Effectuez ces étapes avant toute autre chose. Le JavaScript d’extraction dépend des correspondances de chaîne exactes.

### 2 bis. Activer l&#39;option Contre remboursement avec le titre exact

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** > **[!UICONTROL Cash On Delivery Payment]**

* **[!UICONTROL Enabled]** : **Oui**
* **[!UICONTROL Title]** : **`Cash`** (cette chaîne exacte est celle à laquelle correspond le JavaScript de passage en caisse)

> **Remarque :** si votre boutique utilise une implémentation ou un titre de paiement à la livraison (COD) différent, ajustez les `payment-method-helper.js` dans le module Commerce.

### 2 ter. Activer le crédit de magasin

**[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]**

* **[!UICONTROL Enable Store Credit Functionality]** : **Oui**

### 2 quater. Ajouter du crédit de boutique à votre client de test

**[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[votre client de test]* > **[!UICONTROL Store Credit]** > **[!UICONTROL Update Balance]**

Ajoutez au moins **50** de dollars de crédit en magasin. Vous effectuerez un test avec une commande inférieure à 100 $ au total.

### 2j. Création de l’intégration Commerce

**[!UICONTROL System]** > **[!UICONTROL Integrations]** > **[!UICONTROL Add New Integration]**

* **[!UICONTROL Name]** : `Split Payment App Builder` (ou tout nom que vous préférez)
* Dans l’onglet **[!UICONTROL API]** , accordez au minimum :
   * `Magento_Sales::actions` (obligatoire pour le point d’entrée `cash-received`)
   * `Magento_Sales::cancel` (obligatoire pour le point d’entrée `cash-decline`)
   * Gestion des devis ou des paniers (complet ou un sous-ensemble approprié)
   * **[!UICONTROL Customer balance]** (complet ou un sous-ensemble approprié)

Sélectionnez **[!UICONTROL Save]**, puis **[!UICONTROL Activate]**.

**Copiez les quatre valeurs ; elles ne s’affichent qu’une seule fois :**

| Valeur | Variable d’environnement |
| --- | --- |
| Clé du client | `COMMERCE_CONSUMER_KEY` |
| Secret du client | `COMMERCE_CONSUMER_SECRET` |
| Jeton d’accès | `COMMERCE_ACCESS_TOKEN` |
| Jeton d’accès secret (Access Token Secret) | `COMMERCE_ACCESS_TOKEN_SECRET` |

Stockez ces valeurs en toute sécurité. Vous en avez besoin dans chaque fichier `.env` d’App Builder.


## &#x200B;3. Adobe Developer Console et App Builder

* Accès à une organisation Adobe Developer Console
* Un **projet** (nouveau ou réutilisé)
* Un espace de travail configuré ; les invites supposent **[!UICONTROL Stage]**
* **[!UICONTROL Adobe I/O Events]** ajouté en tant que service à l’espace de travail
* **&#x200B;**&#x200B;connecté en tant que fournisseur d&#39;événements pour l&#39;espace de travail

Le code d’événement utilisé dans la preuve de concept est le suivant : `com.adobe.commerce.observer.sales_order_place_before`

Si vous n’avez pas connecté Commerce en tant que fournisseur d’événements, consultez [Configuration de Commerce pour émettre des événements vers Adobe I/O](https://developer.adobe.com/commerce/extensibility/events/configure-commerce/){target="_blank"} dans le guide Extensibilité de Commerce.


## &#x200B;4. Environnement de développement local

```bash
# Required versions
node --version    # 18.x or later
npm --version     # any recent version

# Adobe I/O CLI
npm install -g @adobe/aio-cli

# Authenticate
aio login

# Select your project and workspace
aio app use
# Confirm the org, project, and workspace shown are correct
```


## &#x200B;5. Deux répertoires de projet à connaître

Ce tutoriel utilise deux racines de répertoire distinctes. Gardez-les séparées.

**racine du projet** (votre référentiel git Magento) :

```text
<commerce-root>/
└── app/code/Client/SplitPayment/   ← Module goes here
```

**Racine du projet** (le dossier `split-payment-orchestrator` du package source ou un nouveau projet que vous créez) :

```text
split-payment-orchestrator/
├── app.config.yaml
├── package.json
├── .env                            ← Copy from .env.example, then fill in
└── actions/
```


## &#x200B;6. entity_id comparé à increment_id

> **Utilisez toujours `entity_id` (l’identifiant numérique de base de données), et non `increment_id` (par exemple `000000042`), dans les appels REST.**

Recherchez des `entity_id` à partir de l’URL de commande **[!UICONTROL Commerce Admin]** :

```text
/admin/sales/order/view/order_id/42/   →   entity_id = 42
```

Ou à partir du script de simulation :

```bash
node scripts/simulate-split-payment.mjs show 42
```


## &#x200B;7. Le seuil de 100 $

L’interface utilisateur de paiement partagé et les commandes cibles de garde de seuil **100,00 $ ou moins**. Le flux se comporte comme suit :

* **À 100 $ ou moins :** l’interface utilisateur de paiement partagé s’affiche ; le client peut définir un fractionnement de l’encaisse et du crédit en magasin
* **Au-dessus de $100:** `CheckoutPlugin` renvoie une erreur à l’étape de paiement

Pour effectuer un test, créez un panier dont le sous-total, les frais d’expédition et les taxes sont **inférieurs ou égaux à 100 $** (par exemple, un produit de moins de 90 $, de sorte que les frais d’expédition et de taxe soient toujours inclus dans la limite).

Le seuil est stocké dans :

* Configuration de Commerce : `split_payment/general/threshold` (`100` par défaut en `etc/config.xml`)
* Environnement App Builder : `PAYMENT_THRESHOLD=100` (doit correspondre à Commerce).


## &#x200B;8. Fastly (Commerce Cloud uniquement)

Les actions App Builder appellent Commerce via REST (`/rest/V1/split-payment/orders/...`). Si votre projet Commerce Cloud utilise Fastly avec une liste autorisée IP, les adresses IP de sortie du runtime App Builder doivent être placées sur la liste autorisée.

**Comment vérifier :** Exécutez d’abord le script de simulation (curl direct avec signature OAuth). Si cela fonctionne mais que l’action App Builder renvoie `403`, Fastly bloque probablement la requête.

Utilisez la documentation actuelle d’Adobe sur les plages d’adresses IP de sortie App Builder et ajoutez-les à votre configuration Fastly si nécessaire.


## Liste de contrôle de vérification

Avant de démarrer les invites de build, vérifiez les points suivants :

* [ ] version de Commerce est la version 2.4.5 ou ultérieure
* [ Les événements d’E/S ] sont activés (Commerce Cloud) et déployés.
* [ ] l&#39;option Contre remboursement est activée avec le titre défini exactement sur `Cash`
* [ Le crédit de la boutique ] est activé.
* [ ] Le client du test a un crédit de magasin d’au moins 50 $
* [ ] L’intégration Commerce est créée et activée, et les quatre valeurs OAuth sont enregistrées
* [ ] Le service Événements I/O et le fournisseur d’événements Commerce sont configurés pour le projet App Builder
* [ ] `aio login` est terminé et l’espace de travail approprié est sélectionné avec `aio app use`
* [ ] Node.js version 18 ou ultérieure est installé et l’interface de ligne de commande `aio` est installée
* [ ] fichiers `.env` sont préparés conformément à [Répartition des POC de paiement : référence des variables d’environnement](./env-reference.md) (et votre package source, le cas échéant)


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
