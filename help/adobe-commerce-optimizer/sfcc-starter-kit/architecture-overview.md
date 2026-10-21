---
title: Architecture du connecteur Commerce Cloud de Salesforce
description: Découvrez comment le kit de démarrage du connecteur Commerce Cloud Salesforce utilise les actions d’exécution App Builder et les exportations delta pour synchroniser les catalogues avec Adobe Commerce Optimizer.
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 288
last-substantial-update: 2025-10-20T00:00:00.000Z
jira: KT-19014
exl-id: 1e0edcbb-5619-45c2-b06d-9133f23a634f
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: d3b92bef-63fa-5031-a925-d04d9362d616
    internal-label: Saas
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
subfeature_v2:
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%
---
# Architecture du kit de démarrage Salesforce Commerce Cloud

Découvrez l’architecture et les fonctionnalités du kit de démarrage du connecteur Commerce Optimizer, qui intègre Salesforce Commerce Cloud (SFCC) et Adobe App Builder. Le kit de démarrage est utilisé par Adobe Commerce Optimizer pour rationaliser la synchronisation des catalogues pour les storefronts Edge Delivery. Il explique comment une cartouche personnalisée dans SFCC détecte les modifications de catalogue par le biais de fichiers d’exportation delta et les expose par le biais d’API personnalisées. Ces modifications sont utilisées par les actions d’exécution d’App Builder (synchrones et asynchrones) pour effectuer des synchronisations complètes et delta, des mises à jour de métadonnées et des synchronisations spécifiques au produit. Le système comprend également des outils de validation pour assurer la précision du storefront et utilise la gestion de l’état d’App Builder pour suivre l’état de synchronisation et prévenir les conflits.

## À qui s&#39;adresse cette vidéo ?

* Architecte de solution Commerce
* Ingénieurs et ingénieures marketing techniques
* Administrateurs de la plateforme eCommerce

## Contenu vidéo

* Les cartouches et API SFCC personnalisées détectent les modifications de catalogue par le biais d’exportations delta, ce qui permet une synchronisation efficace des données avec Adobe App Builder.
* Les actions d’exécution d’App Builder gèrent les synchronisations complètes et par différence, la validation et le suivi d’état afin d’assurer des mises à jour précises et sans conflit de Commerce Optimizer.

>[!VIDEO](https://video.tv.adobe.com/v/3476046?learn=on)

