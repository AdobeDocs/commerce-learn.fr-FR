---
title: Configuration, déploiement et personnalisation d’un webhook d’ingestion pour intégrer Commerce à un système tiers
description: Découvrez comment configurer et personnaliser un webhook d’ingestion pour faciliter la communication entre Commerce et un système back-office tiers.
landing-page-description: Découvrez comment utiliser le kit de démarrage de l’intégration Commerce pour intégrer Commerce à un système back-office tiers à l’aide d’un webhook d’ingestion.
kt: 15870
doc-type: video
duration: 593
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: aed143b96f13a413f85fc461e11f358b4c657015
workflow-type: tm+mt
source-wordcount: '413'
ht-degree: 0%

---

# Configuration, déploiement et personnalisation d’un webhook d’ingestion

Découvrez la configuration et la personnalisation d’un webhook d’ingestion pour intégrer Commerce à un système back-office tiers. &#x200B; Cette vidéo explique comment le webhook peut résoudre les limitations de communication d’événement entre les systèmes en fournissant un point de terminaison disponible pour le public afin d’adapter les messages du système tiers à l’API Adobe IO Eventing. Le processus implique la configuration du webhook dans le fichier `actions.config.yaml`, son activation dans le fichier `app.config.yaml` et son déploiement pour garantir le bon fonctionnement.

La vidéo décrit les étapes à suivre pour modifier le code webhook afin de traduire des événements tiers dans des formats compatibles avec les types d’événements abonnés de l’intégration. Il aborde l’ajout d’un fichier `event-mapping.json` pour faciliter cette traduction et souligne l’importance du redéploiement de l’action d’exécution après avoir apporté des modifications. &#x200B; La vidéo met également en évidence l’importance de la validation et de la transformation des payloads d’événement entrant pour s’aligner sur le schéma attendu, afin d’assurer un traitement et une intégration réussis avec l’API Commerce pour la création de clients.

## Audience

* Développeurs qui souhaitent configurer un webhook d’ingestion
* Toute personne qui souhaite personnaliser le code pour la traduction d’événement
* Développeurs et architectes qui souhaitent comprendre l’importance de l’authentification et de la gestion de la charge utile

## Contenu vidéo

* Configuration et déploiement : la vidéo met l’accent sur l’importance de la configuration du webhook d’ingestion dans le fichier `actions.config.yaml` et de son activation dans le fichier `app.config.yaml`. Il souligne également la nécessité de redéployer le projet après avoir apporté des modifications pour s’assurer que le webhook fonctionne correctement.
* Personnalisation de la compatibilité : il est essentiel de personnaliser le code webhook pour traduire des événements tiers dans des formats conformes aux types d’événements abonnés de l’intégration. &#x200B; Cette personnalisation garantit une communication transparente entre les systèmes et un traitement des événements réussi.
* Mise en oeuvre de l’authentification : les entreprises sont chargées de mettre en oeuvre des mécanismes d’authentification adaptés à leurs besoins afin d’empêcher les demandes non autorisées lors de l’utilisation du webhook d’ingestion. Cette étape est essentielle au maintien de la sécurité et de l’intégrité de l’intégration.
* Validation et transformation de la payload : la validation et la transformation des payloads d’événement entrant pour qu’elles correspondent au schéma attendu sont essentielles pour un traitement et une intégration réussis avec l’API Commerce. &#x200B; En rognant et en mappant les champs de manière appropriée, l’intégration peut fonctionner efficacement avec les données nécessaires.

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Exemples de code

* [webhook d’ingestion personnalisé](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [Ajouter un planificateur d’ingestion](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)
