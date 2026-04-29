---
title: 'Création d’un PDC de paiement partagé : App Builder et outils d’IA'
description: Découvrez une preuve de concept de paiement partagé avec App Builder et Commerce PaaS, y compris les objectifs, l’architecture et ce que cette première session couvre.
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Technical Video
duration: 260
jira: KT-20791
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '574'
ht-degree: 0%

---

# Création d’un PDC de paiement partagé : App Builder et outils d’IA

Il s’agit du premier d’une série de tutoriels qui vous présentent l’utilisation du développement assisté par l’IA pour vous aider à créer une preuve de concept de paiement fractionné. Vous travaillez avec Adobe App Builder et Adobe Commerce en mode cloud (PaaS) ou sur site, en passant d’une présentation de cette session aux tutoriels pratiques qui suivent. Attendez-vous à des objectifs, une architecture de haut niveau et une feuille de route pour découvrir tout ce que couvre le reste de la série.

## Vidéo

>[!VIDEO](https://video.tv.adobe.com/v/3483935?captions=fre_fr&learn=on)

## Détails importants


Ce tutoriel permet de combler le fossé entre l’emplacement actuel de la plupart des équipes d’Adobe Commerce (en PHP) et ce qu’Adobe souhaite leur donner : une logique commerciale événementielle et sans serveur s’exécutant en dehors de Commerce dans Adobe App Builder. Pour ce faire, il s’agit d’une véritable fonctionnalité de travail plutôt que d’un exemple de jouet.

### Ce que vous allez réellement construire

Système de paiement fractionné dans lequel les clients paient en combinant l&#39;option Contre remboursement et le crédit de magasin. Une fois la commande passée, un opérateur (ou un système ERP) confirme ou refuse le paiement en espèces via un tableau de bord autonome, sans jamais ouvrir Commerce Admin. L’ensemble du workflow d’acceptation/refus s’exécute dans App Builder.

#### La leçon d&#39;architecture (enseignement de base)

Le tutoriel présente un cadre de décision délibéré et répétable :

* **Que doit-on garder dans PHP :** tout ce qui s&#39;exécute de manière synchrone dans le cycle de requête de Commerce, ou qui appelle des API internes à Commerce sans surface externe propre
* **Ce qui passe à App Builder :** tout le reste : traitement des événements, workflow des opérateurs, intégrations externes et outils pour les opérateurs.

Il ne s’agit pas de « tout déplacer vers App Builder ». Il s’agit d’un point de départ pratique et honnête pour les équipes qui doivent commencer la transition sans avoir à la réécrire.

### Pourquoi le code PHP n&#39;est pas inclus

L’approche d’invite de l’IA est en fait meilleure que l’exemple de code pour ce cas d’utilisation, car l’invite capture les contrats comportementaux et le raisonnement architectural que l’exemple de code seul ne peut pas transmettre. Un développeur qui exécute l’invite et lit la sortie comprend pourquoi le code est mis en forme tel qu’il est, et pas seulement à quoi il ressemble.

### Éléments Inclus

* Complétez le code de l’application App Builder (cohérent dans tous les projets — utilisez-le directement ou comme référence).
* Un ensemble complet d’invites d’IA numérotées conçues pour Cursor et Claude, couvrant le module Commerce, l’orchestrateur App Builder, le tableau de bord de l’opérateur, les tests et le chemin vers la production
* Un script de simulation permettant de tester le flux d’acceptation/refus par rapport à un site Commerce en ligne sans avoir besoin d’un véritable ERP
* Documentation sur l’architecture expliquant chaque décision

### À Qui Ceci Est Destiné

Développeurs sur Adobe Commerce On-premise ou Commerce Cloud qui démarrent leur première véritable intégration à App Builder. Pas pour le déploiement API-first entièrement découplé, en particulier pour les équipes en transition, exécutant un storefront traditionnel, qui veulent voir comment décharger la logique commerciale réelle vers App Builder sans abandonner leur investissement PHP existant.

### Légende des prérequis

Adobe Commerce version 2.4.5 ou ultérieure, accès à une organisation Adobe Developer Console avec un projet App Builder et événements d’E/S activés. Un thème Luma propre est supposé être utilisé pour l’interface utilisateur de passage en caisse ; les thèmes personnalisés nécessiteront d’ajuster l’invite avant de l’exécuter.

### Réflexions finales

Cela devrait être considéré comme une discussion d&#39;entrée de gamme sur le développement assisté par l&#39;IA. Ce tutoriel est une démonstration de l’utilisation des outils d’IA dans un workflow de développement Commerce, et pas seulement un tutoriel sur le fractionnement de paiements.

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
