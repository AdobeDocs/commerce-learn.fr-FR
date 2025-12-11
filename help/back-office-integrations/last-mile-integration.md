---
title: Intégration du dernier kilomètre dans le kit de démarrage de l’intégration Commerce.
description: Intégration du dernier kilomètre dans Commerce, mettant en évidence les points d’extension tels que la validation, la transformation, le prétraitement, l’envoi et le post-traitement​
landing-page-description: Découvrez la structure et les fonctions des hooks d’extensibilité dans l’intégration Last Mile pour les systèmes Commerce.
kt: 15869
doc-type: video
duration: 465
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: e86e8c7b-d5d2-484d-90a2-9c5309c7ea1d
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 0%

---

# Intégration du dernier kilomètre à l’aide du kit de démarrage Adobe

Découvrez les éléments à prendre en compte lors du démarrage de l’intégration du dernier kilomètre avec Adobe Commerce, en mettant l’accent sur l’utilisation de crochets d’extensibilité pour améliorer la connectivité avec des systèmes tiers. Cette vidéo décrit une approche structurée où divers points d’extension tels que la validation, la transformation, le prétraitement, l’envoi et le post-traitement assurent un flux de données transparent et une synchronisation du système. Chaque hook a un objectif distinct, notamment :

* Validation des données entrantes par rapport aux schémas
* Transformation d’objets de données entre des systèmes
* Effectuer des calculs avant d’envoyer les informations pertinentes
* Envoyer les données au système de destination

Il est important de conserver des fichiers JavaScript distincts pour chaque bloc afin de garantir l’intégrité de la logique commerciale et de faciliter les futures mises à niveau du framework, en assurant une configuration d’intégration robuste et adaptable.

Découvrez l’importance des activités de post-traitement par le biais du hook de post-traitement, qui permet aux utilisateurs d’effectuer des actions supplémentaires après la synchronisation des données, comme ajouter des commentaires aux commandes ou stocker des identifiants externes. La vidéo comprend les bonnes pratiques telles que l’encapsulation des requêtes d’API dans des bibliothèques spécifiques afin de rationaliser les connexions aux systèmes tiers. Vous découvrirez également des cas d’utilisation standard pour chaque hook et des conseils sur la gestion de différents scénarios.

## Audience

* Développeurs qui souhaitent découvrir la structure et les fonctionnalités des hooks d’extensibilité, ainsi que la manière dont ces hooks peuvent améliorer la connectivité avec des systèmes tiers.
* Développeurs qui souhaitent découvrir les cas d’utilisation standard et les bonnes pratiques associées à chaque hook d’extensibilité, telles que la validation, la transformation, le prétraitement, l’envoi et le post-traitement, afin de faciliter le flux de données transparent, la synchronisation du système et une maintenance efficace de la configuration de l’intégration. &#x200B;

## Contenu vidéo

* Découvrez la structure des actions appelées dans l’intégration du dernier kilomètre.
* Comprenez les cas d’utilisation standard dans le hook de validation, y compris la validation des données entrantes par rapport aux schémas et l’omission d’événements spécifiques en fonction de certains critères. &#x200B;
* Découvrez le rôle du crochet de transformation dans la transformation des objets de données entre les systèmes d’origine et de destination.
* Découvrez l’importance du hook d’envoi pour faciliter l’envoi réel des données au système de destination.

>[!VIDEO](https://video.tv.adobe.com/v/3431692?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
