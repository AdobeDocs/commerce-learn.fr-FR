---
title: Extensibilité hors processus pour Adobe Commerce
description: Découvrez Adobe App Builder et pourquoi il s’agit d’un aspect important de l’extensibilité hors processus.
landing-page-description: Découvrez ce qu’est le générateur d’applications et comment il peut vous aider avec les stratégies de développement Adobe Commerce.
kt: 11433
doc-type: tutorial
audience: all
last-substantial-update: 2023-01-11T00:00:00Z
source-git-commit: ef0fa95e776b97ddbaf30e0acd1340e30f12738f
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 0%

---


# Extensibilité hors processus

Le développement d’Adobe Commerce a toujours été effectué à l’aide du même référentiel que l’application principale.  On l’appelle en cours de traitement.  Cette technique est très efficace et offre au développeur un mécanisme attendu pour étendre l’application.  Cependant, cela a un prix.  Chaque fois que vous ajoutez du nouveau code à la base de code, il doit être compatible avec toutes les mises à niveau.  Vous devez également être compatible avec la version PHP des serveurs, ainsi qu’avec de nombreux autres services et applications serveur que commerce utilisera.  Adobe Developer App Builder requiert la même extension que celle de la fonctionnalité, mais la déplace hors du site.  Le code et la logique sont entièrement externes et cette méthode est appelée &quot;hors processus&quot;.

## App Builder pour Adobe Commerce {#project-firefly}

>[!VIDEO](https://video.tv.adobe.com/v/3412839)

Adobe Developer App Builder fournit une structure d’extensibilité permettant aux développeurs d’étendre [!DNL Adobe Commerce] pour fournir une extensibilité hors processus.

App Builder fournit une structure d’extensibilité tierce unifiée pour l’intégration et la création d’applications personnalisées qui étendent [!DNL Adobe Commerce]. Ce framework d’extensibilité reposant sur l’infrastructure de l’Adobe, les développeurs peuvent créer des microservices personnalisés, mais aussi étendre et intégrer [!DNL Adobe Commerce] dans les solutions Adobe et d’autres intégrations tierces.

App Builder permet aux clients d’étendre [!DNL Adobe Commerce] dans divers cas d’utilisation :

* Extensibilité du middleware : connectez des systèmes externes à des applications Adobe en créant des connecteurs personnalisés ou en profitant d’une suite d’intégrations préconfigurées.
* extensibilité des services principaux - Étendez les fonctionnalités de l’application principale en étendant le comportement par défaut avec des fonctionnalités personnalisées et une logique métier.
* extensibilité de l’expérience utilisateur : étendez l’expérience principale pour répondre aux besoins de l’entreprise ou créez des propriétés numériques, des vitrines et des applications de back-office spécifiques aux clients.

App Builder (précédemment connu sous le nom de Project Firefly) est une solution cloud, ce qui signifie qu’elle est automatiquement mise à l’échelle. Ce service est également distribué à l’échelle mondiale afin d’offrir des performances optimales, quel que soit votre emplacement géographique.

## Pourquoi en savoir plus sur App Builder

Comme Adobe Commerce n’est pas entièrement SAAS, le code que vous développez ou installez peut ajouter de la complexité et des problèmes de mise à niveau. En utilisant l’extensibilité hors processus, telle qu’App Builder, vous pouvez fournir des fonctionnalités personnalisées et uniques à votre boutique Adobe Commerce sans avoir à recourir à des méthodes de processus.

Autres avantages :

* Les fonctionnalités découplées permettent un lancement plus rapide.
* Les mises à niveau sont désormais plus simples. Les fonctionnalités personnalisées se trouvent en dehors du code base de commerce, ce qui empêche les problèmes de compatibilité lors de la mise à niveau.
* Le déplacement de fonctionnalités et de logiques en dehors du commerce libère des ressources qui sont normalement utilisées par des méthodes de développement en cours de processus.

## Architecture {#architecture}

Au lieu d’une solution prête à l’emploi, Adobe Developer App Builder fournit une plateforme de développement commune, cohérente et normalisée pour étendre les solutions Adobe Cloud telles qu’Adobe Commerce, notamment :

* Console Adobe Developer utilisée pour le développement de microservices et d’extensions personnalisés. Créez et gérez des projets tout en accédant à tous les outils et API nécessaires pour créer des modules externes et des intégrations.
* Outils Open Source, SDK et bibliothèques pour créer des extensions et des intégrations personnalisées. Utilisez React Spectrum (boîte à outils de l’interface utilisateur de l’Adobe) pour disposer d’une interface utilisateur commune pour toutes les applications d’Adobe.
* des services tels que I/O Runtime pour l’hébergement de l’infrastructure sur la plateforme sans serveur d’Adobe et des événements d’E/S pour les intégrations basées sur des événements. Adobe fournit également une prise en charge prête à l’emploi pour le stockage des données et des fichiers.
* Adobe Experience Cloud dans lequel vous envoyez des extensions et des intégrations à publier dans votre organisation Experience Cloud. Les administrateurs système peuvent examiner, gérer et approuver ces extensions. Une fois la publication effectuée, vos extensions et outils App Builder personnalisés sont disponibles avec d’autres applications Adobe Experience Cloud.

Le diagramme suivant illustre la manière dont une application standard créée sur App Builder utilise ces fonctionnalités :

![Architecture](/help/assets/app-builder/firefly-architecture.jpeg)

Pour plus d’informations sur l’architecture du créateur d’applications, voir [Présentation de l’architecture](https://developer.adobe.com/app-builder/docs/guides/).

## Prise en main d’App Builder {#additional-resources}

Pour vous aider à prendre en main App Builder, Adobe a créé la documentation suivante :

* [Prise en main d’App Builder](https://developer.adobe.com/app-builder/docs/getting_started/)

## Poursuivre l’apprentissage avec la documentation {#appbuilder-documentation}

App Builder fournit des vidéos et de la documentation pour les développeurs, notamment des guides et une documentation de référence pour vous aider à développer vos propres applications personnalisées :

* [Documentation du générateur d’applications](https://developer.adobe.com/app-builder/docs/overview/)
* [Vidéos sur App Builder](https://www.youtube.com/playlist?list=PLcVEYUqU7VRfDij-Jbjyw8S8EzW073F_o)

## Essai d’une des applications d’exemple {#appbuilder-codesamples}

Prêts à se développer ? Le lien suivant contient des exemples d’applications pour vous aider à démarrer :

* [Labs de code du créateur d’applications sur le site web d’Adobe Developer](https://developer.adobe.com/app-builder/docs/resources/)

## Assistance {#support}

Pour les demandes d’assistance aux développeurs, utilisez le [Forum des Experience League](https://experienceleaguecommunities.adobe.com/t5/app-builder/ct-p/project-firefly) pour obtenir de l’aide.

