---
title: Commerce Composable Avec Adobe
description: Découvrez le commerce composable avec les options de la plateforme Adobe, les storefronts Edge Delivery Services, le maillage API, App Builder et les outils de MCP de développement Commerce.
feature: API Mesh, App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Developer, User
level: Intermediate
doc-type: Tutorial
duration: 307
last-substantial-update: 2026-11-13
jira: KT-15730
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
TQID: 'https://experienceleague.adobe.com/NG-US7zLBgzV425mheo3oQ9Z6gnzAr6aRCLNjHVU0aM'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: c32adafa-ed01-4b31-997e-2413013911b0
    internal-label: Integrations
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
    internal-label: Privacy
autotag-review: '2026-11-13T20:04:10.194Z'
source-git-commit: 3a52f4de8c63f08418f4fa44833a7bf7eb90175b
workflow-type: tm+mt
source-wordcount: '1311'
ht-degree: 0%
---
# Commerce composable

Le commerce composable est une approche architecturale qui combine des fonctionnalités commerciales par le biais d’API. Vous pouvez sélectionner et intégrer des composants pour les expériences storefront, la gestion des catalogues, la découverte de produits, le passage en caisse et d’autres besoins de l’entreprise au lieu d’exiger que chaque fonctionnalité vienne d’une application étroitement couplée.

Le commerce découplé sépare le calque de présentation du storefront du serveur principal de commerce. Le commerce composable s’appuie sur cette séparation en permettant aux équipes de choisir les fonctionnalités à conserver ensemble et celles à intégrer indépendamment. Un storefront découplé ne rend pas intrinsèquement chaque composant principal remplaçable indépendamment.

Adobe prend en charge cette approche par le biais des options de la plateforme commerciale, de la technologie storefront et des outils d’intégration et de développement. Les MCP de développement Adobe Commerce as a Cloud Service (ACCS), Adobe Commerce Optimizer (ACO), Edge Delivery Services, API Mesh, App Builder et Commerce ont des objectifs différents. Ce ne sont pas des produits interchangeables ou une pile combinée requise.

## Avantages et compromis

Une approche composable peut vous aider à :

* **Adaptez l&#39;expérience client :** choisissez des composants storefront et intégrez des services qui répondent aux besoins de votre entreprise.
* **Moderniser de manière incrémentielle :** Remplacer les fonctionnalités sélectionnées tout en conservant les autres systèmes et leurs données.
* **Mise à l’échelle et évolution indépendante :** conception d’intégrations et d’extensions afin que leurs modifications ne nécessitent pas la publication de l’ensemble de l’application commerciale.
* **Réutiliser les fonctionnalités commerciales :** utilisez les fonctionnalités de plateforme et les API prises en charge plutôt que de recréer toutes les fonctions commerciales.

Ces avantages dépendent de la mise en œuvre. Évaluez la maturité technique, les coûts de diffusion, les licences, la sécurité et la confidentialité des données. Définissez la propriété des contrats d’API, de la synchronisation des données, de l’authentification, des performances et des échecs entre les services. Une architecture plus modulaire peut également accroître l’intégration et la complexité opérationnelle.

## Options d’architecture d’Adobe Commerce

Commencez par décider si vous avez besoin d’une plateforme de commerce gérée ou si vous souhaitez moderniser les expériences client autour d’un serveur principal transactionnel existant.

Pour une comparaison de vidéos, consultez la section [Adobe - Options commerciales disponibles](./adobes-available-commerce-options.md).

### Adobe Commerce as a Cloud Service

Adobe Commerce as a Cloud Service est une plateforme commerciale gérée en SaaS. Adobe gère l’application principale, l’infrastructure et les mises à jour. Son architecture comprend Commerce Foundation, des fonctionnalités de storefront découplé, des services de marchandisage et des outils de développement.

Utilisez cette option lors de l’évaluation d’une base de commerce gérée avec des points d’extension pris en charge. Pour la logique commerciale personnalisée, prévoyez l’extensibilité hors processus prise en charge plutôt que de supposer que les extensions PHP existantes peuvent être déployées inchangées. ACCS ne prend pas en charge les storefronts de Luma. Incluez la migration des storefronts dans votre plan d’implémentation, le cas échéant.

Voir la [présentation d’Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/overview).

### Adobe Commerce Optimizer

Adobe Commerce Optimizer prend en charge la gestion de catalogues, la découverte de produits, la recherche, les recommandations et les expériences de storefront tout en vous permettant de conserver un serveur principal de commerce existant. Les données de catalogue peuvent provenir de sources externes et un système tiers peut rester responsable du cycle de vie du panier et du passage en caisse.

Utilisez cette option lorsque la modernisation face au client est l’objectif et que le remplacement du serveur principal transactionnel n’est pas nécessaire. Planifiez explicitement l’ingestion de catalogue et les intégrations de panier et de passage en caisse. ACCS et ACO sont des options d’architecture distinctes, et non une pile combinée requise. Vérifiez la prise en charge actuelle de l’intégration avant de sélectionner les services pour votre implémentation.

Voir la [présentation de Adobe Commerce Optimizer](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview).

### Déploiements Adobe Commerce existants

Adobe Commerce sur les déploiements d’infrastructure cloud et sur site peut également adopter des storefronts découplés et des intégrations basées sur les API prises en charge de manière incrémentielle. Selon le déploiement, vous pouvez conserver un storefront existant ou adopter un storefront découplé pris en charge. Le fait de conserver les fonctionnalités commerciales existantes ensemble peut réduire le travail d’intégration tout en permettant à certains services d’évoluer séparément.

La disponibilité des fonctionnalités, la compatibilité des extensions, les licences et les exigences de configuration varient selon l’offre et la version. Comparez les [plateformes et services Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce/information-center/explore-platforms-and-services/platform-and-services) avant de choisir une architecture.

## Commerce Storefront optimisé par Edge Delivery Services

Le storefront Commerce optimisé par Edge Delivery Services fournit l’expérience storefront et la couche de diffusion de contenu. Son architecture découplée permet aux équipes de développer le front-end indépendamment de la base de commerce. La création basée sur les documents et la modification visuelle prennent en charge les mises à jour de contenu, tandis que les composants de storefront utilisent les API de commerce pour les fonctionnalités d’achat.

Distinguer l’interface du storefront des systèmes responsables des données du catalogue, des prix, des opérations du panier, du passage en caisse et des commandes. Edge Delivery Services ne remplace pas le serveur principal de commerce transactionnel. Identifiez les services pris en charge et le chemin d’intégration pour le serveur principal de votre choix avant de mettre en œuvre le storefront.

Consultez la [documentation du storefront Commerce](https://experienceleague.adobe.com/en/tools/commerce-storefront/).

## Intégration et extensibilité

Le maillage API et App Builder permettent de connecter et d’étendre les fonctionnalités commerciales sans qu’il soit nécessaire que chaque personnalisation soit incorporée dans l’application commerciale principale.

### Maillage API

Le maillage API pour Adobe Developer App Builder combine plusieurs sources d’API Adobe, privées et tierces en un point d’entrée GraphQL interrogeable. Utilisez-les pour composer des données pour les expériences front-end et coordonner l’accès aux services existants.

La composition de l’API ne supprime pas la nécessité de définir des contrats d’API, une authentification, une gestion des erreurs ou la propriété de la logique commerciale transactionnelle. Veillez à ce que ces responsabilités soient explicites dans la conception de l’intégration.

Consultez la [présentation du maillage API](https://developer.adobe.com/graphql-mesh-gateway/).

### Adobe App Builder

Adobe App Builder fournit une plateforme pour les applications qui étendent les solutions Adobe et s’exécutent sur l’infrastructure Adobe, y compris Adobe I/O Runtime. Vous pouvez l’utiliser pour les personnalisations et les intégrations hors processus avec des systèmes tels que les CRM, les ERP et les PIM.

Choisissez les points d’extension, événements et webhooks pris en charge pour l’offre Commerce que vous utilisez. Concevez et testez l’application séparément du cycle de vie de la version commerciale principale.

Voir la [présentation d’App Builder](https://developer.adobe.com/app-builder/docs/intro_and_overview/).

## Compétences et MCP de développement Commerce

Les MCP et les compétences de développement Commerce prennent en charge le développement assisté par l’IA des applications App Builder et des personnalisations de storefront. Ils peuvent également vous aider à migrer les extensions PHP Adobe Commerce existantes vers les applications App Builder lors du déplacement vers ACCS.

Les serveurs MCP (Model Context Protocol) connectent les clients de codage d’IA compatibles aux outils de développement. Les outils d’Adobe comprennent le serveur Commerce Developer MCP, le serveur App Builder MCP et les outils d’IA Dropins, ainsi que des compétences spécifiques à Commerce.

Les MCP de développement Commerce sont des outils de développement et non des services de diffusion, de panier, de passage en caisse ou de traitement des commandes en storefront. Ils ne rendent pas automatiquement une application compatible avec chaque offre Commerce. Examinez les modifications générées et testez-les par rapport à l’environnement cible.

Voir [MCP et compétences de développement Commerce](https://developer.adobe.com/commerce/extensibility/developer-agent/coding-tools).

## Planifier une implémentation composable

1. Définissez les objectifs commerciaux, les expériences client, les exigences en matière de sécurité et les compétences disponibles pour votre équipe.
2. Choisissez la plateforme commerciale et identifiez les systèmes existants à conserver ou à remplacer.
3. Sélectionnez un storefront pris en charge et définissez les limites entre la présentation, le marchandisage et les services transactionnels.
4. Établissez la propriété des données et les contrats d’API pour les informations de catalogue, les prix, l’inventaire, les clients, les paniers, le passage en caisse et les commandes.
5. Planifiez l’authentification, la synchronisation des données, la mise en cache, la surveillance et la gestion des échecs entre les intégrations.
6. Implémentez de manière incrémentielle et validez les fonctionnalités, les performances et la sécurité avant chaque version.

Une implémentation composable réussit lorsque ses composants répondent aux besoins de l’entreprise et fonctionnent ensemble de manière fiable. Sélectionnez les fonctionnalités dont vous avez besoin plutôt que d’ajouter des services uniquement pour rendre l’architecture plus modulaire.

## Ressources connexes

Consultez ces tutoriels, vidéos, guides d’implémentation et liste de lecture pour planifier votre architecture et créer des intégrations commerciales.

### Plateformes et storefronts Commerce

* [Architecture d’Adobe Commerce as a Cloud Service](../commerce-as-a-cloud-service/architecture.md)
* [Vitrines composables et architecture modulaire](../commerce-as-a-cloud-service/product-overview/composable-storefront-and-modular-architecture.md)
* [Présentation de Adobe Commerce Optimizer](../adobe-commerce-optimizer/overview.md)
* [Explication du modèle de données de catalogue composable](../adobe-commerce-optimizer/composable-catalog-data-model/why-ccdm-exists.md)
* [Expériences client avec Edge Delivery Services](../edge-delivery-services/shopper-experience.md)

### Intégration et développement

* [Extensibilité hors processus avec App Builder](../app-builder/foundations/introduction-to-app-builder.md)
* [Prise en main du maillage API](../api-mesh/getting-started-api-mesh.md)
* [Création d’un maillage API avec plusieurs sources](../api-mesh/graphql-multiple-source.md)
* [Créer des tarifs d’expédition personnalisés avec les outils d’IA et de MCP](../app-builder/custom-shipping-rates-ai-tools/ai-tools-starter-kit.md)

### Documentation sur l’architecture et le développement

* [Migration vers Adobe Commerce as a Cloud Service](https://experienceleague.adobe.com/en/docs/commerce/cloud-service/migration/overview)
* [Intégrations Adobe Commerce Optimizer](https://experienceleague.adobe.com/en/docs/commerce/optimizer/integrations-overview)
* [Prise en main du développement du storefront de Commerce](https://experienceleague.adobe.com/en/tools/commerce-storefront/get-started/)
* [Bonnes pratiques pour les MCP et les compétences de développement Commerce](https://developer.adobe.com/commerce/extensibility/developer-agent/best-practices)

### Playlist

* [Prise en main du maillage API avec Adobe App Builder et Adobe Commerce](https://experienceleague.adobe.com/en/playlists/commerce-get-started-app-builder-and-api-mesh)
