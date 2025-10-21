---
title: Présentation de l’architecture de l’application Salesforce Commerce Cloud Connector
description: Découvrez l’architecture du Commerce Cloud Salesforce avec Adobe Commerce Optimizer.
feature: App Builder,Saas
topic: Administration,Commerce,Integrations
role: Architect, Developer
level: Beginner
doc-type: Technical Video
duration: 0
last-substantial-update: 2025-10-20T00:00:00Z
jira: KT-19014
source-git-commit: 54a1a8e62e86f8ae3456bb41a1b0450134f26b71
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 0%

---


# En savoir plus sur l’architecture du kit de démarrage cloud Salesforce Commerce

Découvrez l’architecture et les fonctionnalités du kit de démarrage du connecteur Commerce Optimizer, qui intègre Salesforce Commerce Cloud (SFCC) et Adobe App Builder. Le kit de démarrage est utilisé par Adobe Commerce Optimizer pour rationaliser la synchronisation des catalogues pour les storefronts Edge Delivery. Il explique comment une cartouche personnalisée dans SFCC détecte les modifications de catalogue par le biais de fichiers d’exportation delta et les expose par le biais d’API personnalisées. Ces modifications sont utilisées par les actions d’exécution d’App Builder (synchrones et asynchrones) pour effectuer des synchronisations complètes et delta, des mises à jour de métadonnées et des synchronisations spécifiques au produit. Le système comprend également des outils de validation pour assurer la précision du storefront et utilise la gestion des états d’App Builder pour suivre l’état de synchronisation et prévenir les conflits.

## À qui s&#39;adresse cette vidéo ?

* Architecte de solution Commerce
* Ingénieurs et ingénieures marketing techniques
* Administrateurs de la plateforme eCommerce

## Contenu vidéo

* Les cartouches et API SFCC personnalisées détectent les modifications de catalogue par le biais d’exportations delta, ce qui permet une synchronisation efficace des données avec Adobe App Builder.
* Les actions d’exécution d’App Builder gèrent les synchronisations complètes et par différence, la validation et le suivi d’état afin d’assurer des mises à jour précises et sans conflit de Commerce Optimizer.

>[!VIDEO](https://video.tv.adobe.com/v/3476053?captions=fre_fr&learn=on)
