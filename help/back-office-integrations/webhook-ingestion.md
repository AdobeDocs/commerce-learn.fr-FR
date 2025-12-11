---
title: Configuration, déploiement et personnalisation d’un Webhook d’ingestion
description: Découvrez comment configurer et personnaliser un webhook d’ingestion pour faciliter la communication entre Commerce et un système back-office tiers.
landing-page-description: Découvrez comment utiliser le kit de démarrage de l’intégration Commerce pour intégrer Commerce à un système back-office tiers à l’aide d’un webhook d’ingestion.
kt: 15870
doc-type: video
duration: 593
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 0%

---

# Configuration, déploiement et personnalisation d’un webhook d’ingestion

Découvrez la configuration et la personnalisation d’un webhook d’ingestion pour intégrer Commerce à un système back-office tiers&#x200B; Cette vidéo explique comment le webhook peut résoudre les problèmes de communication d’événements entre les systèmes en fournissant un point d’entrée accessible au public pour adapter les messages du système tiers à l’API d’événement d’Adobe IO. Le processus implique de configurer le webhook dans le fichier `actions.config.yaml`, de l’activer dans le fichier `app.config.yaml` et de le déployer pour garantir son bon fonctionnement.

La vidéo décrit les étapes à suivre pour modifier le code webhook afin de traduire les événements tiers en formats compatibles avec les types d’événements abonnés à l’intégration. Il explique comment ajouter un fichier `event-mapping.json` pour faciliter cette traduction et souligne l’importance de redéployer l’action d’exécution après avoir apporté des modifications&#x200B; La vidéo souligne également l’importance de valider et de transformer les payloads d’événement entrant pour s’aligner sur le schéma prévu, afin d’assurer un traitement réussi et une intégration à l’API Commerce pour créer des clients.

## Audience

* Développeurs qui souhaitent configurer un webhook d’ingestion
* Toute personne souhaitant personnaliser le code pour la traduction d’événements
* Développeurs et architectes qui souhaitent comprendre l’importance de l’authentification et de la gestion de la payload

## Contenu vidéo

* Configuration et déploiement : la vidéo souligne l’importance de configurer le webhook d’ingestion dans le fichier `actions.config.yaml` et de l’activer dans le fichier `app.config.yaml`. Elle souligne également la nécessité de redéployer le projet après avoir apporté des modifications pour garantir le bon fonctionnement du webhook.
* Personnalisation pour la compatibilité : il est essentiel de personnaliser le code webhook pour traduire les événements tiers en formats conformes aux types d’événements avec abonnement dans l’intégration. &#x200B; Cette personnalisation garantit une communication transparente entre les systèmes et un traitement réussi des événements.
* Implémentation de l’authentification : les entreprises sont chargées de mettre en œuvre des mécanismes d’authentification adaptés à leurs besoins pour empêcher les requêtes non autorisées lors de l’utilisation du webhook d’ingestion. Cette étape est essentielle pour maintenir la sécurité et l’intégrité de l’intégration.
* Validation et transformation de la payload : la validation et la transformation des payloads d’événement entrant pour qu’elles correspondent au schéma prévu sont essentielles pour le succès du traitement et de l’intégration avec l’API Commerce. &#x200B; En réduisant et en mappant les champs de manière appropriée, l’intégration peut fonctionner efficacement avec les données nécessaires.

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Exemples de code

* [Webhook d’ingestion personnalisé](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [Ajouter un planificateur d’ingestion](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)
