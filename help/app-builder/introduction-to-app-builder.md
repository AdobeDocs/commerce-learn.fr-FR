---
title: Extensibilité hors processus pour Adobe Commerce
description: Découvrez Adobe App Builder et pourquoi il s’agit d’un aspect important de l’extensibilité hors processus.
landing-page-description: Découvrez ce qu’est App Builder et comment il peut vous aider avec les stratégies de développement Adobe Commerce.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-16T00:00:00Z
source-git-commit: 82ccecf2789e1eedf447af2705a3840d0302fdba
workflow-type: tm+mt
source-wordcount: '819'
ht-degree: 0%

---


# Présentation d’App Builder

Historiquement, le développement d’Adobe Commerce utilisait l’extensibilité du processus. Le modèle en cours de traitement nécessite que tout nouveau code soit compatible avec les mises à niveau, la version PHP du serveur et de nombreux autres services et applications de serveur essentiels utilisés par Commerce. Adobe Developer App Builder utilise l’extensibilité hors processus pour éviter ces problèmes de compatibilité.

## App Builder pour Adobe Commerce {#project-firefly}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder est une plateforme d’extensibilité sans serveur permettant d’intégrer et de créer des expériences personnalisées afin d’étendre les solutions d’Adobe. Elle est désormais disponible pour Adobe Commerce. Avec App Builder, vous pouvez créer des applications sécurisées et évolutives qui étendent les fonctionnalités natives au commerce et s’intègrent à des solutions tierces. En tant que développeur, vous pouvez désormais tirer parti de l’extensibilité hors processus avec Adobe Commerce, ce qui à son tour apporte des avantages immédiats et à long terme.

App Builder fournit une structure d’extensibilité tierce unifiée pour l’intégration et la création d’applications personnalisées qui étendent [!DNL Adobe Commerce]. Ce framework d’extensibilité reposant sur l’infrastructure de l’Adobe, les développeurs peuvent créer des microservices personnalisés, puis étendre et intégrer [!DNL Adobe Commerce] sur d’autres solutions Adobe et intégrations tierces.

App Builder permet aux clients d’étendre [!DNL Adobe Commerce] dans divers cas d’utilisation :

* Extensibilité du middleware : connectez des systèmes externes à des applications Adobe en créant des connecteurs personnalisés ou en profitant d’une suite d’intégrations préconfigurées.
* extensibilité des services principaux - Étendez les fonctionnalités de l’application principale en étendant le comportement par défaut avec des fonctionnalités personnalisées et une logique métier.
* extensibilité de l’expérience utilisateur : étendez l’expérience principale pour répondre aux besoins de l’entreprise ou créez des propriétés numériques, des vitrines et des applications de back-office spécifiques aux clients.

App Builder (précédemment connu sous le nom de Project Firefly) est une solution cloud, ce qui signifie qu’elle est automatiquement mise à l’échelle. Ce service est également distribué à l’échelle mondiale afin d’offrir des performances optimales, quel que soit votre emplacement géographique.

## Pourquoi en savoir plus sur App Builder

Adobe Commerce n’étant pas un produit SAAS complet, le code que vous développez peut ajouter de la complexité et des problèmes de mise à niveau. En utilisant l’extensibilité hors processus, telle qu’App Builder, vous pouvez fournir des fonctionnalités personnalisées et uniques à votre boutique Adobe Commerce sans nécessiter de méthodes de processus.

Autres avantages :

* Les fonctionnalités découplées permettent un lancement plus rapide.
* Les mises à niveau sont désormais plus simples. Les fonctionnalités personnalisées se trouvent en dehors du code base de Commerce, ce qui empêche les problèmes de compatibilité lors de la mise à niveau.
* Le déplacement des fonctionnalités et de la logique en dehors de Commerce libère les ressources qui sont normalement utilisées par les méthodes de développement en processus.

## Architecture {#architecture}

Au lieu d’une solution prête à l’emploi, Adobe Developer App Builder fournit une plateforme de développement commune, cohérente et normalisée pour étendre les solutions Adobe Cloud telles qu’Adobe Commerce, notamment :

* Console Adobe Developer utilisée pour le développement de microservices et d’extensions personnalisés. Créez et gérez des projets tout en accédant à tous les outils et API nécessaires pour créer des modules externes et des intégrations.
* Outils Open Source, SDK et bibliothèques pour créer des extensions et des intégrations personnalisées. Utilisez React Spectrum (boîte à outils de l’interface utilisateur de l’Adobe) pour disposer d’une interface utilisateur commune pour toutes les applications d’Adobe.
* des services tels que I/O Runtime pour l’hébergement de l’infrastructure sur la plateforme sans serveur d’Adobe et des événements d’E/S pour les intégrations basées sur des événements. Adobe fournit également une prise en charge prête à l’emploi pour le stockage des données et des fichiers.
* Adobe Experience Cloud dans lequel vous envoyez des extensions et des intégrations à publier dans votre organisation Experience Cloud. Les administrateurs système peuvent examiner, gérer et approuver ces extensions. Une fois la publication effectuée, vos extensions et outils App Builder personnalisés sont disponibles avec d’autres applications Adobe Experience Cloud.

Le diagramme suivant illustre la manière dont une application standard créée sur App Builder utilise ces fonctionnalités :

![Architecture](/help/assets/app-builder/firefly-architecture.jpeg)

Pour plus d’informations sur l’architecture du créateur d’applications, voir [Présentation de l’architecture](https://developer.adobe.com/app-builder/docs/guides/){target="_blank"}.

## Extension Amazon Sales Channel {#amazon-sales-channel-extension}

>[!IMPORTANT]
>
>L’extension Amazon Sales Channel est toujours en cours de développement et n’a pas été officiellement publiée.  Ces vidéos et tutoriels sont destinés à vous montrer comment utiliser Adobe Developer App Builder dans un cas pratique.

Les tutoriels suivants montrent comment connecter Adobe Commerce à Amazon Sales Channel à l’aide d’une extension App Builder.

* [présentation technique d’App Builder](../app-builder/app-builder-technical-overview.md)
* [framework d’extensibilité](../app-builder/extensibility-framework-commerce-eventing.md)
* [générateur d’applications de démonstration fonctionnel](../app-builder/app-builder-functional-demonstration.md)

## Prise en main d’App Builder {#additional-resources}

Vous trouverez un aperçu de la stratégie de commerce composable, qui inclut la configuration initiale en lisant le billet de blog suivant :

[Comment App Builder contribue à accroître l’agilité de votre entreprise pour votre plateforme commerciale](https://business.adobe.com/blog/how-to/how-app-builder-helps-you-implement-a-composable-commerce-strategy){target="_blank"}

Pour vous aider à prendre en main App Builder, Adobe a créé la documentation suivante :

* [Prise en main d’App Builder](https://developer.adobe.com/app-builder/docs/getting_started/){target="_blank"}

## Poursuivre l’apprentissage avec la documentation {#appbuilder-documentation}

App Builder fournit des vidéos et de la documentation pour les développeurs, notamment des guides et une documentation de référence pour vous aider à développer vos propres applications personnalisées :

* [Documentation du générateur d’applications](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [Vidéos sur App Builder](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o){target="_blank"}

## Essai d’une des applications d’exemple {#appbuilder-codesamples}

Prêts à se développer ? Le lien suivant contient des exemples d’applications pour vous aider à démarrer :

* [Labs de code du créateur d’applications sur le site web d’Adobe Developer](https://developer.adobe.com/app-builder/docs/resources/){target="_blank"}

## Assistance {#support}

Pour les demandes d’assistance aux développeurs, utilisez le [Forum des Experience League](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly){target="_blank"} pour obtenir de l’aide.

{{$include /help/_includes/app-builder-related-links.md}}
