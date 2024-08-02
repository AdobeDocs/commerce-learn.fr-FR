---
title: Découvrez le kit de démarrage de l’intégration Commerce avec des dossiers clés et des scripts d’automatisation expliqués
description: Découvrez l’organisation du code source dans le kit de démarrage de l’intégration Commerce. ​
landing-page-description: Exploration de l’organisation du code Source dans un kit de démarrage d’intégration Commerce
kt: 15868
doc-type: video
duration: 420
audience: all
last-substantial-update: 2024-7-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: f0c6e9262a2bf2de3144255de1fc78d6972b6d33
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 0%

---

# Organisation du code Source pour le kit de démarrage d’Adobe

Découvrez l’organisation du code source dans le kit de démarrage de l’intégration Adobe Commerce. &#x200B; Explorez la structure du projet, en mettant en surbrillance les dossiers clés tels que `actions` et `scripts`, ainsi que leur contenu respectif. &#x200B; Le dossier &quot;actions&quot; contient des sous-dossiers tels que `ingestion` et `webhook` qui contiennent du code essentiel pour la gestion et le suivi des événements. Vous en apprendrez également davantage sur les dossiers `starter-kit-info` et `scripts`. Le dossier `scripts` se concentre sur les scripts d’automatisation tels que `commerce-event-subscribe` et `onboarding` qui rationalisent la configuration des événements et la configuration des fournisseurs dans le projet.
&#x200B;
Explorez la logique derrière la structure du code source, en détaillant la manière dont les dossiers `commerce` et `external` sous chaque dossier d’entité gèrent les événements provenant de différents systèmes. La vidéo explique le rôle du dossier `consumer` dans la distribution des événements à l’action d’exécution du gestionnaire d’événements appropriée, ce qui permet un traitement transparent. La vidéo couvre également le mécanisme de reprise implémenté dans les actions d’exécution pour gérer efficacement les événements d’échec. &#x200B;Découvrez l’organisation et les fonctionnalités du code source dans le kit de démarrage de l’intégration Adobe Commerce, offrant des informations précieuses sur la gestion des événements, les scripts d’automatisation et les configurations.

## Audience

* Les développeurs qui souhaitent comprendre comment le code source est organisé en dossiers clés tels que `actions` et `scripts`.
* Découvrez le dossier `actions` contient des sous-dossiers tels que `ingestion` et ` webhook` qui contiennent du code essentiel pour la gestion des événements et le suivi des déploiements.
* Les développeurs qui souhaitent en savoir plus sur le dossier `actions` qui comprend des dossiers pour des entités telles que `customer`, `order`, `product` et `stock`.

## Contenu vidéo

* Comprenez que les quatre dossiers principaux : `actions`, `scripts`, `test` et `utils`, avec un focus sur les dossiers `actions` et `scripts` pendant la session. &#x200B;
* Découvrez le dossier `actions` et comment il contient des sous-dossiers essentiels tels que `ingestion` et `webhook`.
* Explorez le dossier `actions` et pourquoi il existe des dossiers spécifiques pour des entités telles que `customer`, `order`, `product` et `stock`, chacun contenant des actions d’exécution structurées en dossiers `commerce` et `external` pour gérer efficacement les événements à partir de Commerce et de systèmes tiers. &#x200B;
* Découvrez l’importance de ne pas modifier le code du dossier `starter-kit-info`, qui contient une action d’exécution utilisée par Adobe pour effectuer le suivi des déploiements de projet en fonction du kit de démarrage. &#x200B;
* Comprenez le dossier `scripts` qui contient des scripts d’automatisation tels que `commerce-event-subscribe` et `onboarding`, qui automatisent la configuration des événements, la configuration du fournisseur et la configuration du module Événements d’Adobe I/O dans Commerce. &#x200B;

  >[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
