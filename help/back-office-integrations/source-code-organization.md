---
title: Organisation du code Source dans le kit de démarrage Commerce
description: Découvrez l’organisation du code source dans le kit de démarrage de l’intégration Commerce, y compris les dossiers clés tels que les actions et les scripts, les scripts d’automatisation et la gestion des événements.
doc-type: Technical Video
duration: 534
last-substantial-update: 2024-07-30
feature: Best Practices, Backend Development, Integration
topic: Architecture, Commerce, Development
role: Developer
level: Intermediate
jira: KT-15868
exl-id: 678f4d2b-c57e-4afb-a535-1048a88bc3b1
TQID: https://experienceleague.adobe.com/P6-sK18TcpC91YXJcXohIvzmii3N66ZKh3nZha-RYQY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2:
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 9568f37b026d0e659e8092282cb923c7ecde58ac
workflow-type: tm+mt
source-wordcount: 252
ht-degree: 0%

---

# Organisation du code Source pour le kit de démarrage Adobe

Découvrez l’organisation du code source dans le kit de démarrage de l’intégration d’&#x200B; explorez la structure du projet, en mettant en évidence les dossiers clés tels que `actions` et `scripts`, ainsi que leur contenu respectif. &#x200B; Le dossier « actions » contient des sous-dossiers tels que `ingestion` et `webhook` qui contiennent le code essentiel pour la gestion et le suivi des événements. Vous découvrirez également les dossiers `starter-kit-info` et `scripts`. Le dossier `scripts` se concentre sur les scripts d’automatisation tels que les `commerce-event-subscribe` et les `onboarding` qui rationalisent la configuration des événements et des fournisseurs dans le projet.
&#x200B;
Explorez la logique derrière la structure du code source, en détaillant la manière dont les dossiers `commerce` et `external` sous chaque dossier d’entité gèrent les événements provenant de différents systèmes. La vidéo explique le rôle du dossier `consumer` dans la distribution des événements à l’action d’exécution de gestionnaire d’événements appropriée, assurant ainsi un traitement transparent. La vidéo couvre également le mécanisme de reprise implémenté dans les actions d’exécution pour gérer efficacement les événements en échec. &#x200B;Découvrez l’organisation et les fonctionnalités du code source dans le kit de démarrage de l’intégration Adobe Commerce, offrant des informations précieuses sur la gestion des événements, les scripts d’automatisation et les configurations.

## Audience

* Les développeurs qui souhaitent comprendre comment le code source est organisé en dossiers clés tels que `actions` et `scripts`.
* Découvrez que le dossier `actions` contient des sous-dossiers tels que `ingestion` et ` webhook` qui contiennent le code essentiel pour la gestion des événements et le suivi des déploiements.
* Les développeurs et développeuses qui souhaitent en savoir plus sur le dossier `actions` qui comprend des dossiers pour des entités telles que `customer`, `order`, `product` et `stock`.

## Contenu vidéo

* Gardez à l’esprit que les quatre dossiers principaux : `actions`, `scripts`, `test` et `utils`, avec un focus sur les dossiers `actions` et `scripts` au cours de la session. &#x200B;
* Découvrez le dossier `actions` et comment il contient des sous-dossiers essentiels tels que `ingestion` et `webhook`.
* Explorez le dossier `actions` et pourquoi il existe des dossiers spécifiques pour des entités telles que `customer`, `order`, `product` et `stock`, chacun contenant des actions d’exécution structurées en dossiers `commerce` et `external` pour gérer efficacement les événements de Commerce et de systèmes tiers. &#x200B;
* Découvrez l’importance de ne pas modifier le code dans le dossier `starter-kit-info`, qui contient une action d’exécution utilisée par Adobe pour effectuer le suivi des déploiements de projet en fonction du kit de démarrage. &#x200B;
* Découvrez le dossier `scripts` qui contient des scripts d’automatisation tels que `commerce-event-subscribe` et `onboarding`, qui automatisent la configuration des événements, la configuration des fournisseurs et la configuration du module Adobe I/O Events dans Commerce. &#x200B;

>[!VIDEO](https://video.tv.adobe.com/v/3431691?learn=on)

{{$include /help/_includes/starter-kit-related-links.md}}
