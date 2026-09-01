---
title: Exécution d’essai de l’agent de développement Adobe Commerce App Builder
description: Découvrez comment créer, déployer et tester trois cas d’utilisation d’extensibilité de Commerce avec l’agent de développement Adobe Commerce dans cette exécution à sec pratique d’App Builder.
feature: Extensibility, App Builder, Eventing, Configuration
topic: App Builder, Development, Integrations
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 438
last-substantial-update: 2026-08-28T00:00:00Z
source-git-commit: 92af5355fa31c1ce9e627679b0a1bb92cce0e1d8
workflow-type: tm+mt
source-wordcount: '1700'
ht-degree: 0%

---

# Exécution d’essai de l’agent de développement Adobe Commerce App Builder

Une présentation pratique pour créer, déployer et tester des cas d’utilisation d’extensibilité de Commerce avec l’agent de développement Adobe Commerce (CDA). Cette simulation couvre trois cas d’utilisation : un webhook de limite de quantité du panier, un blocage de commande de grande valeur et un archivage piloté par les événements pour les commandes bloquées, du plan directeur aux tests fonctionnels.

## Prise en main

### Comment signaler des problèmes et des commentaires

Tout au long de l&#39;essai, vous rencontrez des bords rugueux, ce qui est normal lorsque vous travaillez avec une nouvelle fonctionnalité. Saisissez et partagez tout problème avec votre contact de programme Adobe à l’aide du modèle de retour d’informations fourni lors de l’intégration.

>[!TIP]
>
> Lors du signalement d’un problème :
>
> * Incluez la `projectId` (visible dans l’URL de votre navigateur).
> * Insérez des captures d’écran chaque fois que cela est pertinent.

### Conditions préalables

**Comptes et accès**

* Au moins le rôle **Développeur** dans votre organisation IMS en accès anticipé.
* Un accès administrateur à une instance Adobe Commerce as a Cloud Service (ACCS) au sein de cette organisation, disponible à l’adresse experience.adobe.com sous **Instances Cloud Service**.
* Un compte GitHub.

**Outils**

Un storefront Edge Delivery Services (EDS) est requis pour la validation fonctionnelle. Vous aurez besoin de :

* Node.js 22+
* Interface de ligne de commande Adobe I/O : `npm install -g @adobe/aio-cli`
* Plug-in Commerce de l’interface de ligne de commande d’AIO : `aio plugins:install https://github.com/adobe-commerce/aio-cli-plugin-commerce`

Installez le standard storefront dans un dossier vide, en sélectionnant votre instance ACCS lorsque vous y êtes invité :

```bash
aio commerce extensibility app-setup -s aem-boilerplate-commerce -n storefront
```

Démarrez le storefront :

```bash
cd storefront
npm run start
```

## Ouverture de l’agent de développement Commerce

1. Accédez à l’agent de développement Commerce à l’adresse experience.adobe.com, sous **Agent de développement**.
1. Connectez-vous à l’aide de vos identifiants d’organisation IMS en accès anticipé.

## Cas d’utilisation 1 : webhook du nombre maximal d’unités du panier

Ce cas pratique valide les limites de quantité du panier avant l’ajout d’un produit, à l’aide d’un webhook Commerce synchrone.

### Étape du plan directeur

Saisissez l’invite suivante et cliquez sur **Générer le plan directeur** :

```text
Add a validation webhook that runs before a product is added to the cart.

Use the Commerce webhook method observer.sales_quote_item_save_before (type before) — do not use
observer.checkout_cart_product_add_before, observer.sales_quote_add_item, or any other event.

Calculate the total by summing all quote line quantities and the quantity of the current item.
If the same SKU already exists in the quote, exclude its existing quantity to avoid double-counting.

If the total is greater than the maximum allowed, block the add and show:
"You have reached the maximum amount of items."

The maximum allowed must be configurable in Commerce Admin as max_cart_units, with default 10.

Map payload fields using name and source properties:
- name: item.qty, source: data.item.qty
- name: item.sku, source: data.item.sku
- name: quote, source: context_checkout_session.get_quote[items.qty,items.sku]

Set required: true and fallback_error_message: "You have reached the maximum amount of items."
on the webhook config.

When blocking the add, do not use exceptionOperation, because it serializes exceptionClass as class.
Instead, manually return an exception operation response whose body includes type:
{
  "op": "exception",
  "message": "You have reached the maximum amount of items.",
  "type": "\\Magento\\Framework\\GraphQl\\Exception\\GraphQlInputException"
}
```

>[!NOTE]
>
> Recherchez les éléments suivants :
>
> * Un plan directeur (v1) capturant les exigences est créé.
> * Des tâches pour guider l’implémentation sont créées.

Affinez le plan directeur en saisissant les détails dans la boîte de dialogue ou en cliquant sur l’une des pilules au-dessus de la boîte de dialogue (*Remettre en question les hypothèses*, *Trouver les lacunes de conception*, etc.). Une fois que vous êtes satisfait, cliquez sur **Approuver le plan** pour continuer.

### Étape de développement

L’agent passe à l’étape de développement et commence à configurer l’espace de travail.

>[!NOTE]
>
> Recherchez ces fichiers dans le panneau Explorateur :
>
> * `app.commerce.config.ts`
> * `app.config.yaml`
> * `install.yaml`
> * `package-lock.json`
> * `package.json`

Une fois configuré, l’agent affiche une liste des tâches d’implémentation et commence à créer.

>[!NOTE]
>
> Recherchez les éléments suivants :
>
> * Le code généré correspond aux exigences.
> * L’écran `Validate` la diffusion en continu affiche la progression de la validation de l’espace de travail (`aio app build`).
> * L’agent corrige automatiquement le code généré si la validation échoue.

Une fois satisfait du code, cliquez sur l’onglet **Intégrations** pour continuer.

### Configuration des intégrations

**Connexion ou création d’un espace de travail App Builder**

Pour créer ou connecter un projet App Builder, suivez les instructions à l’écran.

Si vous vous connectez à un espace de travail existant, assurez-vous qu’il dispose des éléments suivants :

* Ajout du service `Runtime`.
* Les API suivantes ont été ajoutées : Adobe Commerce as a Cloud Service, I/O Management API, App Builder Data Services, I/O Events, Adobe I/O Events pour Adobe Commerce.

Si vous créez un espace de travail, ajoutez manuellement l’API **Adobe Commerce as a Cloud Service**.

>[!IMPORTANT]
>
> Une fois la connexion à un projet App Builder existant établie, développez **Configuration avancée** et collez le fichier JSON de l’espace de travail, puis cliquez sur **Vérifier le statut** pour confirmer que toutes les API requises sont installées.

Cliquez sur **Suivant** pour continuer.

**Connexion à Commerce**

Sélectionnez votre instance ACCS dans la liste ou saisissez l’URL dans le champ **URL de base REST** puis cliquez sur **Connecter l’instance Commerce**. Cliquez sur **Suivant** pour continuer.

**Connexion à GitHub**

Connectez l’espace de travail à un référentiel GitHub en saisissant l’URL du référentiel et en utilisant l’application GitHub ou un jeton d’accès personnel. Cliquez sur **Suivant** pour continuer.

**Configurer les variables d’environnement**

Renseignez toutes les variables d’environnement requises par le projet.

### Déployer

Cliquez sur **Développer** pour revenir à l’étape de développement, puis demandez à l’agent de se déployer dans le champ d’invite.

>[!NOTE]
>
> Recherchez un message « Confirmer le déploiement » affichant l’espace de noms d’organisation, de projet, de Workspace et d’exécution.

Confirmez le déploiement.

>[!NOTE]
>
> Recherchez :
>
> * Écran de diffusion en continu `Validate` affichant la progression de la validation avant déploiement.
> * L’agent corrige automatiquement le code si la validation échoue.
> * Écran de streaming `Deploy` affichant la progression du déploiement (`aio app deploy`).
> * L’agent corrige automatiquement le code en cas d’échec du déploiement.

### Associer l’application dans la gestion des applications

1. Accédez à l’URL d’administration de votre instance ACCS et connectez-vous.
1. Sélectionnez **Applications** dans le menu de gauche, puis **Gestion des applications**.
1. Cliquez sur **+ Associer l’application** (en haut à droite).
1. Sélectionnez le projet et le Workspace sur lesquels les analyses entre appareils ont été déployées, puis cliquez sur **Associer**.

>[!NOTE]
>
> Recherchez une carte présentant le nom et la version de l’application, ainsi que les fonctionnalités mises en œuvre (configuration métier, Webhooks, événements, etc.).

### Installation et configuration dans App Management

1. Sur la ligne correspondant à votre application, cliquez sur **Installer**, puis **Fermer**.
1. Sur la même ligne, cliquez sur **Configurer** pour renseigner les valeurs de configuration métier, puis sur **Fermer**.

>[!NOTE]
>
> Recherchez un formulaire affichant chaque champ de configuration spécifié par le plan directeur, prérempli avec les valeurs par défaut que vous avez spécifiées.

### Tests fonctionnels

1. Dans la configuration de l’application App Management, définissez **Nombre maximal d’unités de panier** sur 3 (valeur faible pour un test rapide).
1. Sur le storefront, commencez avec un panier vide.
1. Ajoutez des produits à partir de la page Détails du produit (PDP) jusqu’à ce que la quantité totale dépasse 3 ; le dernier ajout échoue.
1. Sur PDP, vous voyez : *« Vous avez atteint la quantité maximale d’éléments »*.
1. En dessous de la limite, les ajouts réussissent toujours.

>[!NOTE]
>
> Dans la page de liste de produits (PLP), un ajout bloqué échoue silencieusement sans message : il s’agit du comportement de storefront, et non d’un échec de webhook. Préférez le PDP pour la vérification.

## Cas d’utilisation 2 : blocage de commande de grande valeur et code de vérification

Revenez à l’étape **Plan directeur** pour commencer ce cas d’utilisation.

### Étape du plan directeur

Saisissez l’invite suivante et cliquez sur **Générer le plan directeur** :

```text
Add a Commerce event priority subscription to `plugin.sales.api.order_management.place`.

Extract `entity_id` and `grand_total` from the Commerce event payload using event `fields` in `app.commerce.config.ts`.

Important: the runtime action receives a CloudEvents-shaped payload. For Commerce eventing extracted fields,
parse them from `params.data.value`, not directly from `params.data`. The handler must use:
- `params.data.value.entity_id`
- `params.data.value.grand_total`

When `grand_total` is greater than `order_hold_threshold`:
1. Generate a verification code locally.
2. Put the order on hold with state and status `holded`.
When putting the order on hold, save the verification code using `custom_attributes`, not `extension_attributes`.
The Commerce `POST V1/orders` payload should include:
{
  "entity": {
    "entity_id": <entity_id>,
    "state": "holded",
    "status": "holded",
    "custom_attributes": [
      {
        "attribute_code": "<hold_verification_attribute>",
        "value": "<verification_code>"
      }
    ]
  }
}
3. Save the verification code via a `POST V1/orders` Commerce REST API call.

Make these configurable in Commerce Admin:
- `order_hold_threshold`, default `500`
- `hold_verification_attribute`, default `lab_verification_code`

Validate inputs before use:
- `entity_id` must be a positive integer.
- `grand_total` must be a non-negative number.
```

>[!NOTE]
>
> Recherchez les éléments suivants :
>
> * Un plan directeur (v2) capturant les exigences est créé.
> * Les tâches du plan d&#39;origine sont conservées.
> * De nouvelles tâches correspondant aux nouvelles exigences ont été ajoutées.

Affinez le plan directeur si nécessaire, puis cliquez sur **Approuver le plan** pour continuer.

### Développement, déploiement, association et installation

Suivez le même processus que celui utilisé dans le cas d’utilisation 1 pour passer des exigences à une application installée : il n’est pas nécessaire de reconfigurer les intégrations.

>[!IMPORTANT]
>
> Pour apporter des modifications à une application déjà associée, vous devez la **Dissocier** et **Associer** à nouveau dans la gestion des applications.

### Tests fonctionnels

1. Dans la configuration de l’application App Management, définissez **seuil de blocage des commandes (USD)** sur 50 (facile à dépasser dans un panier de test).
1. Vérifiez que l’attribut personnalisé de commande existe (`lab_verification_code` par défaut).
1. Passer une commande dont le total est supérieur à 50 $.
1. Patientez environ 30 secondes (les événements sont asynchrones ; la diffusion non prioritaire peut prendre jusqu’à environ 59 secondes).
1. Dans Commerce Admin → Sales → Orders, ouvrez la commande. Le statut est **En attente** (`holded`) ; les attributs personnalisés incluent des `lab_verification_code` avec une valeur aléatoire.
1. Facultatif : passez d&#39;abord une commande inférieure à 50 $ — ce gestionnaire ne la met pas en attente.

## Cas d’utilisation 3 : archivage piloté par les événements pour les commandes retenues

Revenez à l’étape **Plan directeur** pour commencer ce cas d’utilisation.

### Étape du plan directeur

Saisissez l’invite suivante et cliquez sur **Générer le plan directeur** :

```text
When an order is saved with state holded, archive it to external storage and
record a reference that can be looked up later by order ID.

Add an event priority subscription on observer.sales_order_save_after, filtered to fire only when
state equals holded. From the event payload, extract:
- `entity_id`
- `payment.amount_ordered`
- `custom_attributes` (to read the `lab_verification_code` attribute set in Step 3)

The event handler must:
1. Persist the order details to the `held_orders` App Builder DB collection:
{
  "order_id": <entity_id>,
  "grand_total": <payment.amount_ordered>,
  "verification_code": <lab_verification_code>,
  "archived_at": <ISO timestamp>
}
2. Ensure the record can be looked up later by order ID.

The `held_orders` collection must exist before the handler runs:
- Provision persistent App Builder Database Storage in region `amer`.
- Create the collection during app installation.
- Create a unique index on `order_id` during installation.
- Drop the whole `held_orders` collection when the app is uninstalled.

Register the event handler separately from the existing cart validation webhook and high-value order hold action:
- runtime action: `order-archive/archive-held-order`
- non-web action
- `include-ims-credentials: true` on the archive action and the installation action

Follow the `commerce-app-storage` skill for DB auth, installation steps, and ext.config wiring.
Do not use custom IMS credential normalization or `Core.AuthClient.generateAccessToken`.
```

>[!NOTE]
>
> Recherchez les éléments suivants :
>
> * Un plan directeur (v3) capturant les exigences est créé.
> * Les tâches du plan d&#39;origine sont conservées.
> * De nouvelles tâches correspondant aux nouvelles exigences ont été ajoutées.

Affinez le plan directeur si nécessaire, puis cliquez sur **Approuver le plan** pour continuer.

### Développement, déploiement, association et installation

Suivez le même processus que celui utilisé dans les cas d’utilisation précédents pour passer des exigences à une application installée. Il n’est pas nécessaire de reconfigurer les intégrations.

>[!IMPORTANT]
>
> Pour apporter des modifications à une application déjà associée, vous devez la **Dissocier** et **Associer** à nouveau dans la gestion des applications.

### Tests fonctionnels

1. Assurez-vous que le seuil du cas d’utilisation 2 est suffisamment bas pour le test (par exemple, 50 $ dans la configuration de l’application).
1. Placez une commande au-dessus de ce seuil afin que le cas d’utilisation 2 la mette en attente (~30 secondes).
1. Dans Adobe Developer Console → votre projet → les événements → d’étape, ouvrez l’enregistrement de l’événement d’archivage des commandes conservées (ajouté ou mis à jour lors de l’installation).
1. Confirmez qu’un événement a été remis à cet enregistrement après le déplacement de la commande vers la suspension. Utilisez la trace ou la surveillance des événements pour l&#39;événement Commerce lié à `order-archive/archive-held-order`.

>[!NOTE]
>
> Les événements sont asynchrones : jusqu’à 30 à 59 secondes après la mise en attente de la commande.

## Dépannage

Si l’application générée par les analyses entre appareils ne se comporte pas comme prévu ou génère des erreurs, demandez à l’agent de procéder à la résolution des problèmes à partir de l’étape de développement.

>[!NOTE]
>
> Analytics sur l’ensemble des appareils n’a aucune visibilité sur les étapes qui se produisent en dehors. Les tests fonctionnels, d’association, d’installation et de configuration s’exécutent tous dans Commerce Admin, App Management ou le storefront, mais pas dans Analytics sur l’ensemble des appareils. Si un problème se présente dans l&#39;une de ces zones, l&#39;agent ne peut pas le voir se produire, alors donnez-lui ce qui manque :
>
> * Ce que vous avez fait et où (par exemple, « a cliqué sur Installer dans App Management »).
> * Ce que vous attendiez.
> * Ce qui s&#39;est passé.
> * Texte ou message d’erreur exact affiché à l’écran.
> * Toute erreur pertinente provenant de la console du navigateur ou des journaux App Builder Adobe Developer Console et des traces de débogage de l’enregistrement des événements.

Plus le rapport est concret, plus l&#39;agent peut diagnostiquer le problème.

## Étapes facultatives

**Télécharger le code**

Pour continuer à affiner ou à modifier votre IDE préféré, téléchargez le code généré par les analyses entre appareils en cliquant sur l’icône de téléchargement dans la barre d’outils de l’explorateur d’étape de développement. Sélectionnez un dossier de destination et cliquez sur **Enregistrer**, puis décompressez le package de l’espace de travail.

>[!NOTE]
>
> Recherchez :
>
> * Tous les fichiers affichés dans l’explorateur d’étape de développement sont présents dans le dossier décompressé.
> * Aucune erreur de « compilation » lors de la création du projet avec `aio app build`.

Pour utiliser les mêmes compétences d’agent que celles utilisées par les analyses entre appareils, installez-les dans le dossier du projet :

```bash
npx skills add adobe/aio-commerce-sdk --skill commerce-app-init -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-eventing -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-webhooks -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-business-config -y && \
npx skills add adobe/aio-commerce-sdk --skill commerce-app-storage -y && \
npx skills add adobe/skills --skill appbuilder-project-init -y
```

Démarrez ensuite l’IDE ou l’interface de ligne de commande et commencez à demander.

**Joindre le contexte via un fichier ou un lien**

Au lieu de demander directement dans les étapes de plan directeur ou de développement, vous pouvez joindre un contexte à l’aide d’un fichier texte ou d’un lien :

1. Cliquez sur l’icône de pièce jointe dans la zone de conversation.
1. Cliquez sur **Ajouter un fichier** pour charger un fichier texte local, ou saisissez une URL et cliquez sur **Ajouter un lien** pour ajouter du contexte via un fichier distant.
1. Cliquez sur **Terminé** et saisissez une invite pour pousser l’agent.

>[!NOTE]
>
> Recherchez l’agent incorporant le contexte de vos pièces jointes dans son prochain tour.

## Problèmes connus et solutions

**L’étape de plan directeur ne génère pas de tâches**

Pour être débloqué et continuer, déplacez l’agent pour générer des tâches.

**Les boutons à activer et à extraire de GitHub ne sont pas fonctionnels**

Téléchargez plutôt le fichier ZIP du projet à partir de l’étape Développer .

{{$include /help/_includes/commerce-developer-agent-related-links.md}}

## Ressources supplémentaires

* [Présentation de l’agent de développement Commerce](https://developer.adobe.com/commerce/extensibility/developer-agent/)
* [Prise en main de l’agent de développement Commerce](https://developer.adobe.com/commerce/extensibility/developer-agent/getting-started)
* [Conseils sur l’invite de l’agent de développement Commerce](https://developer.adobe.com/commerce/extensibility/developer-agent/prompting)
* [Prise en charge et commentaires de l’agent de développement Commerce](https://developer.adobe.com/commerce/extensibility/developer-agent/support)
