---
title: Extensibilité hors processus pour Adobe Commerce
description: Découvrez Adobe App Builder et pourquoi il constitue un aspect important de l’extensibilité hors processus.
landing-page-description: Découvrez App Builder et comment il peut vous aider avec les stratégies de développement Adobe Commerce.
short-description: Découvrez App Builder et comment il peut vous aider avec les stratégies de développement Adobe Commerce.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-16
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 94f8d82a-4a95-46ea-8eed-edf9bed5760c
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 6%

---

# Présentation d’App Builder

Historiquement, le développement Adobe Commerce a utilisé l’extensibilité en cours de processus. Le modèle en cours de traitement nécessite que tout nouveau code soit compatible avec les mises à niveau, la version PHP du serveur et de nombreuses autres applications et services de serveur essentiels utilisés par Commerce. Adobe Developer App Builder utilise l’extensibilité hors processus pour éviter ces problèmes de compatibilité.

## App Builder pour Adobe Commerce {#app-builder}

>[!VIDEO](https://video.tv.adobe.com/v/3412839?quality=12&learn=on)

Adobe Developer App Builder est une plateforme d’extensibilité sans serveur permettant d’intégrer et de créer des expériences personnalisées pour étendre les solutions Adobe. Elle est désormais disponible pour Adobe Commerce. Avec App Builder, vous pouvez créer des applications sécurisées et évolutives qui étendent les fonctionnalités natives de Commerce et s’intègrent à des solutions tierces. En tant que développeur, vous pouvez désormais tirer parti de l’extensibilité hors processus avec Adobe Commerce, ce qui offre des avantages immédiats et à long terme.

App Builder fournit un framework tiers unifié permettant d’intégrer et de créer des applications personnalisées pour étendre les [!DNL Adobe Commerce]. Comme ce framework d’extensibilité repose sur l’infrastructure d’Adobe, les développeurs peuvent créer des microservices personnalisés, et étendre et intégrer des [!DNL Adobe Commerce] sur d’autres solutions Adobe et intégrations tierces.

App Builder permet aux clients d’étendre les [!DNL Adobe Commerce] dans divers cas d’utilisation :

* extensibilité des middleware : connectez des systèmes externes à des applications Adobe en créant des connecteurs personnalisés ou tirez parti d’une suite d’intégrations préconfigurées.
* extensibilité des services principaux : étendez les fonctionnalités de l’application principale en étendant le comportement par défaut avec des fonctionnalités personnalisées et une logique métier.
* extensibilité de l’expérience utilisateur : étendez l’expérience principale pour prendre en charge les besoins de l’entreprise ou créer des propriétés numériques, des storefronts et des applications back-office spécifiques aux clients.

Adobe Developer App Builder est une solution cloud, ce qui signifie qu’elle se met automatiquement à l’échelle. Ce service est également distribué à l’échelle mondiale pour offrir les meilleures performances, quelle que soit votre situation géographique.

## Pourquoi en savoir plus sur App Builder ?

Comme Adobe Commerce n’est pas un produit entièrement SAAS, le code que vous développez peut ajouter de la complexité et des problèmes de mise à niveau. En utilisant l’extensibilité hors processus, telle qu’App Builder, vous pouvez fournir des fonctionnalités personnalisées et uniques à votre boutique Adobe Commerce sans nécessiter de méthodes en processus.

Autres avantages :

* Les fonctionnalités découplées permettent un lancement plus rapide.
* Les mises à niveau sont désormais plus faciles. Les fonctionnalités personnalisées se trouvent en dehors de la base de code Commerce, ce qui évite les problèmes de compatibilité lors de la mise à niveau.
* Le déplacement des fonctionnalités et de la logique en dehors de Commerce libère des ressources qui sont normalement utilisées par les méthodes de développement en cours de processus.

## Architecture {#architecture}

Au lieu d’une solution prête à l’emploi, Adobe Developer App Builder fournit une plateforme de développement commune, cohérente et normalisée permettant d’étendre les solutions cloud Adobe telles qu’Adobe Commerce, parmi les suivantes :

* Adobe Developer Console utilisé pour le développement de microservices et d’extensions personnalisés. Créez et gérez des projets tout en accédant à tous les outils et API nécessaires à la création de modules externes et d’intégrations.
* Outils open source, SDK et bibliothèques pour créer des extensions et des intégrations personnalisées. Utilisez React Spectrum (boîte à outils de l’interface utilisateur d’Adobe) pour avoir une interface utilisateur commune à toutes les applications Adobe.
* des services tels que I/O Runtime pour l’hébergement de l’infrastructure sur la plateforme sans serveur d’Adobe et des événements d’E/S pour les intégrations basées sur des événements. Adobe fournit également une prise en charge prête à l’emploi pour le stockage des données et des fichiers.
* Adobe Experience Cloud dans lequel vous envoyez des extensions et des intégrations à publier dans votre organisation Experience Cloud. Les administrateurs système peuvent examiner, gérer et approuver ces extensions. Une fois la publication effectuée, vos extensions et outils App Builder personnalisés sont disponibles avec d’autres applications Adobe Experience Cloud.

Le diagramme suivant illustre la manière dont une application standard créée sur App Builder utilise ces fonctionnalités :

![&#x200B; Architecture &#x200B;](/help/assets/app-builder/app-builder-architecture.jpeg)

Pour plus d’informations sur l’architecture d’App Builder, consultez la [Présentation de l’architecture](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## Prise en main d’App Builder {#additional-resources}

Vous trouverez un aperçu de la stratégie de commerce composable, qui comprend la configuration initiale, en lisant l’article de blog suivant :

[Comment App Builder contribue à l’agilité commerciale de votre plateforme commerciale](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

Pour vous aider à prendre en main App Builder, Adobe a créé la documentation suivante :

* [Prise en main d’App Builder](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## Poursuivre l’apprentissage avec la documentation {#appbuilder-documentation}

App Builder fournit des vidéos et de la documentation à l’intention des développeurs, y compris des guides et une documentation de référence pour vous aider à développer vos propres applications personnalisées :

* [Documentation App Builder](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [Vidéos App Builder](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## Essayez l’un des exemples d’applications {#appbuilder-codesamples}

Prêt à développer ? Le lien suivant contient des exemples d’applications pour vous aider à commencer :

* [App Builder Code Labs sur le site web d’Adobe Developer](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## Support technique {#support}

Pour les demandes d’assistance aux développeurs, utilisez le [forum Experience League](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly?profile.language=fr){target="_blank"} pour obtenir de l’aide.

{{$include /help/_includes/app-builder-related-links.md}}
