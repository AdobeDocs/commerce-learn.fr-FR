---
title: Intégration du dernier kilomètre dans le kit de démarrage de l’intégration Commerce.
description: Intégration du dernier kilomètre dans Commerce, en mettant en évidence les points d’extension tels que la validation, la transformation, le prétraitement, l’envoi et le post-traitement. ​
landing-page-description: Découvrez la structure et les fonctions des hooks d’extensibilité dans l’intégration du dernier kilomètre pour les systèmes Commerce.
kt: 15869
doc-type: video
duration: 465
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: aed143b96f13a413f85fc461e11f358b4c657015
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 0%

---

# Intégration du dernier kilomètre à l’aide du kit de démarrage Adobe

Découvrez les éléments à prendre en compte lors du démarrage de l’intégration du dernier kilomètre avec Adobe Commerce en mettant l’accent sur l’utilisation de crochets d’extensibilité pour améliorer la connectivité avec des systèmes tiers. Cette vidéo décrit une approche structurée selon laquelle divers hooks tels que la validation, la transformation, le prétraitement, l’envoi et le post-traitement assurent un flux de données transparent et une synchronisation du système. Chaque point d’extension remplit un rôle distinct, notamment :

* Validation des données entrantes par rapport aux schémas
* Transformation des objets de données entre les systèmes
* Exécution de calculs avant d’envoyer des informations pertinentes
* Envoi des données au système de destination

Il est important de conserver des fichiers JavaScript distincts pour chaque bloc afin de préserver l’intégrité de la logique commerciale et de faciliter les futures mises à niveau de la structure, en assurant une configuration d’intégration robuste et adaptable.

Découvrez l’importance des activités de post-traitement par le biais du point d’extension post-traitement, qui permet aux utilisateurs d’effectuer des actions supplémentaires après la synchronisation des données, comme ajouter des commentaires aux commandes ou stocker des identifiants externes. La vidéo comprend des bonnes pratiques telles que l’encapsulation des requêtes d’API dans des bibliothèques spécifiques afin de rationaliser les connexions à des systèmes tiers. Vous découvrirez également des cas d’utilisation standard pour chaque crochet et des conseils sur la gestion de différents scénarios.

## Audience

* Les développeurs qui souhaitent découvrir la structure et les fonctionnalités des hooks d’extensibilité, ainsi que la manière dont ces hooks peuvent améliorer la connectivité avec des systèmes tiers.
* Les développeurs qui souhaitent découvrir des cas d’utilisation standard et les bonnes pratiques associées à chaque crochet d’extensibilité, tels que la validation, la transformation, le prétraitement, l’envoi et le post-traitement, afin de faciliter un flux de données transparent, la synchronisation du système et une maintenance efficace de la configuration de l’intégration. &#x200B;

## Contenu vidéo

* Découvrez la structure des actions invoquées dans l’intégration last-mile.
* Comprenez les cas d’utilisation standard dans le point d’extension de validation, notamment la validation des données entrantes par rapport aux schémas et l’exclusion d’événements spécifiques selon certains critères. &#x200B;
* Découvrez le rôle du point de transformation dans la transformation des objets de données entre les systèmes d’origine et de destination.
* Découvrez l’importance du point d’extension d’envoi pour faciliter l’envoi réel de données vers le système de destination.

>[!VIDEO](https://video.tv.adobe.com/v/3451921?learn=on&captions=fre_fr)

{{$include /help/_includes/starter-kit-related-links.md}}
