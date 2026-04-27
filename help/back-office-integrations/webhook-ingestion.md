---
title: Configuration, déploiement et personnalisation d’un Webhook d’ingestion
description: Découvrez comment configurer et personnaliser un webhook d’ingestion pour faciliter la communication entre Commerce et un système back-office tiers.
landing-page-description: Découvrez comment utiliser le kit de démarrage de l’intégration Commerce pour intégrer Commerce à un système back-office tiers à l’aide d’un webhook d’ingestion.
kt: 15870
doc-type: video
duration: 697
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: f2654873-256e-4c1b-abed-8bfbc4db3fbb
TQID: https://experienceleague.adobe.com/nUXdrsjzeD939jOjZS8ywPV3OeOaxpZCmeuveACtYrY
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 432
ht-degree: 0%

---

# Configuration, déploiement et personnalisation d’un webhook d’ingestion

Découvrez la configuration et la personnalisation d’un webhook d’ingestion pour intégrer Commerce à un système back-office tiers. &#x200B; Cette vidéo explique comment le webhook peut résoudre les problèmes de communication d’événement entre les systèmes en fournissant un point d’entrée accessible au public pour adapter les messages du système tiers à l’API d’événement d’Adobe IO. Le processus implique de configurer le webhook dans le fichier `actions.config.yaml`, de l’activer dans le fichier `app.config.yaml` et de le déployer pour garantir son bon fonctionnement.

La vidéo décrit les étapes à suivre pour modifier le code webhook afin de traduire les événements tiers en formats compatibles avec les types d’événements abonnés à l’intégration. Elle aborde l’ajout d’un fichier `event-mapping.json` pour faciliter cette traduction et souligne l’importance de redéployer l’action d’exécution après avoir apporté des modifications. &#x200B; La vidéo souligne également l’importance de valider et de transformer les payloads d’événement entrant pour s’aligner sur le schéma prévu, afin d’assurer un traitement réussi et une intégration à l’API Commerce pour créer des clients.

## Audience

* Développeurs qui souhaitent configurer un webhook d’ingestion
* Toute personne souhaitant personnaliser le code pour la traduction d’événements
* Développeurs et architectes qui souhaitent comprendre l’importance de l’authentification et de la gestion de la payload

## Contenu vidéo

* Configuration et déploiement : la vidéo souligne l’importance de configurer le webhook d’ingestion dans le fichier `actions.config.yaml` et de l’activer dans le fichier `app.config.yaml`. Elle souligne également la nécessité de redéployer le projet après avoir apporté des modifications pour garantir le bon fonctionnement du webhook.
* Personnalisation pour la compatibilité : il est essentiel de personnaliser le code webhook pour traduire les événements tiers en formats conformes aux types d’événements avec abonnement dans l’intégration. &#x200B; Cette personnalisation garantit une communication transparente entre les systèmes et un traitement réussi des événements.
* Implémentation de l’authentification : les entreprises sont chargées de mettre en œuvre des mécanismes d’authentification adaptés à leurs besoins pour empêcher les requêtes non autorisées lors de l’utilisation du webhook d’ingestion. Cette étape est essentielle pour maintenir la sécurité et l’intégrité de l’intégration.
* Validation et transformation de la payload : la validation et la transformation des payloads d’événement entrant pour qu’elles correspondent au schéma prévu sont essentielles pour le succès du traitement et de l’intégration à l’API Commerce. &#x200B; En réduisant et en mappant les champs de manière appropriée, l’intégration peut fonctionner efficacement avec les données nécessaires.

>[!VIDEO](https://video.tv.adobe.com/v/3431694?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}

## Exemples de code

* [Webhook d’ingestion personnalisé](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/customize-ingestion-webhook)
* [Ajouter le planificateur d’ingestion](https://github.com/adobe/adobe-commerce-samples/tree/main/starter-kit/add-ingestion-scheduler)
