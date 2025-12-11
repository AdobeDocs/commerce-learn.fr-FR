---
title: Découvrez le kit de démarrage de l’intégration Commerce avec les dossiers clés et les scripts d’automatisation expliqués
description: Découvrez l’organisation du code source dans le kit de démarrage de l’intégration Commerce. ​
landing-page-description: Exploration de l’organisation du code Source dans un kit de démarrage d’intégration Commerce
kt: 15868
doc-type: video
duration: 420
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 678f4d2b-c57e-4afb-a535-1048a88bc3b1
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Organisation du code Source pour le kit de démarrage Adobe

Découvrez l’organisation du code source dans le kit de démarrage de l’intégration Adobe Commerce&#x200B; Explorez la structure du projet, en mettant en évidence les dossiers clés tels que `actions` et `scripts`, ainsi que leurs contenus respectifs&#x200B; Le dossier « actions » contient des sous-dossiers tels que `ingestion` et `webhook`, qui contiennent le code essentiel pour la gestion et le suivi des événements. Vous découvrirez également les dossiers `starter-kit-info` et `scripts`. Le dossier `scripts` se concentre sur les scripts d’automatisation tels que les `commerce-event-subscribe` et les `onboarding` qui rationalisent la configuration des événements et des fournisseurs dans le projet.
&#x200B;
Explorez la logique derrière la structure du code source, en détaillant la manière dont les dossiers `commerce` et `external` sous chaque dossier d’entité gèrent les événements provenant de différents systèmes. La vidéo explique le rôle du dossier `consumer` dans la distribution des événements à l’action d’exécution de gestionnaire d’événements appropriée, assurant ainsi un traitement transparent. La vidéo couvre également le mécanisme de reprise implémenté dans les actions d’exécution pour gérer efficacement les événements en échec. &#x200B;Découvrez l’organisation et les fonctionnalités du code source dans le kit de démarrage de l’intégration Adobe Commerce, offrant des informations précieuses sur la gestion des événements, les scripts d’automatisation et les configurations.

## Audience

* Les développeurs qui souhaitent comprendre comment le code source est organisé en dossiers clés tels que `actions` et `scripts`.
* Découvrez que le dossier `actions` contient des sous-dossiers tels que `ingestion` et ` webhook` qui contiennent le code essentiel pour la gestion des événements et le suivi des déploiements.
* Les développeurs et développeuses qui souhaitent en savoir plus sur le dossier `actions` qui comprend des dossiers pour des entités telles que `customer`, `order`, `product` et `stock`.

## Contenu vidéo

* Gardez à l’esprit que les quatre dossiers principaux : `actions`, `scripts`, `test` et `utils`, avec un focus sur les dossiers `actions` et `scripts` au cours de la session. &#x200B;
* Découvrez le dossier `actions` et comment il contient des sous-dossiers essentiels tels que `ingestion` et `webhook`.
* Découvrez le dossier `actions` et pourquoi il existe des dossiers spécifiques pour des entités telles que `customer`, `order`, `product` et `stock`, chacun contenant des actions d’exécution structurées en dossiers `commerce` et `external` pour gérer efficacement les événements de Commerce et de systèmes tiers. &#x200B;
* Découvrez l’importance de ne pas modifier le code dans le dossier `starter-kit-info`, qui contient une action d’exécution utilisée par Adobe pour effectuer le suivi des déploiements de projet en fonction du kit de démarrage. &#x200B;
* Découvrez le dossier `scripts` qui contient des scripts d’automatisation tels que `commerce-event-subscribe` et `onboarding`, qui automatisent la configuration des événements, la configuration des fournisseurs et la configuration du module Adobe I/O Events dans Commerce. &#x200B;

>[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
