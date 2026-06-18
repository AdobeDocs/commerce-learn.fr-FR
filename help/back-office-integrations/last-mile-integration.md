---
title: Intégration du dernier kilomètre dans le kit de démarrage Commerce
description: Découvrez l’intégration du dernier kilomètre dans Commerce à l’aide de hooks d’extensibilité pour la validation, la transformation, le prétraitement, l’envoi et le post-traitement.
doc-type: Technical Video
duration: 557
last-substantial-update: 2024-07-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15869
exl-id: e86e8c7b-d5d2-484d-90a2-9c5309c7ea1d
TQID: https://experienceleague.adobe.com/TCR23A98L8XrVDEQeqLQoOXKQPBQu-Wb7YnGUkBXgak
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 342
ht-degree: 0%

---

# Intégration du dernier kilomètre à l’aide du kit de démarrage Adobe

Découvrez les éléments à prendre en compte lors du démarrage de l’intégration du dernier kilomètre avec Adobe Commerce, en vous concentrant sur les hooks d’extensibilité pour améliorer la connectivité avec les systèmes tiers. Cette vidéo décrit une approche structurée où divers points d’extension tels que la validation, la transformation, le prétraitement, l’envoi et le post-traitement assurent un flux de données transparent et une synchronisation du système. Chaque hook a un objectif distinct, notamment :

* Validation des données entrantes par rapport aux schémas
* Transformation d’objets de données entre des systèmes
* Effectuer des calculs avant d’envoyer les informations pertinentes
* Envoyer les données au système de destination

Il est important de conserver des fichiers JavaScript distincts pour chaque bloc afin de garantir l’intégrité de la logique commerciale et de faciliter les futures mises à niveau du framework, en assurant une configuration d’intégration robuste et adaptable.

Découvrez l’importance des activités de post-traitement par le biais du hook de post-traitement, qui permet aux utilisateurs d’effectuer des actions supplémentaires après la synchronisation des données, comme ajouter des commentaires aux commandes ou stocker des identifiants externes. La vidéo comprend les bonnes pratiques telles que l’encapsulation des requêtes d’API dans des bibliothèques spécifiques afin de rationaliser les connexions aux systèmes tiers. Vous découvrirez également des cas d’utilisation standard pour chaque hook et des conseils sur la gestion de différents scénarios.

## Audience

* Développeurs qui souhaitent découvrir la structure et les fonctionnalités des hooks d’extensibilité, ainsi que la manière dont ces hooks peuvent améliorer la connectivité avec des systèmes tiers.
* Développeurs qui souhaitent découvrir les cas d’utilisation standard et les bonnes pratiques associées à chaque hook d’extensibilité, telles que la validation, la transformation, le prétraitement, l’envoi et le post-traitement, afin de faciliter le flux de données transparent, la synchronisation du système et la maintenance efficace de la configuration de l’intégration. &#x200B;

## Contenu vidéo

* Découvrez la structure des actions appelées dans l’intégration du dernier kilomètre.
* Comprenez les cas d’utilisation standard dans le hook de validation, y compris la validation des données entrantes par rapport aux schémas et l’omission d’événements spécifiques en fonction de certains critères. &#x200B;
* Découvrez le rôle du crochet de transformation dans la transformation des objets de données entre les systèmes d’origine et de destination.
* Découvrez l’importance du hook d’envoi pour faciliter l’envoi réel des données au système de destination.

>[!VIDEO](https://video.tv.adobe.com/v/3431692?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
