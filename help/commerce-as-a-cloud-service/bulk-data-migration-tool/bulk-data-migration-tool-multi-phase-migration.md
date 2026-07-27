---
title: Outil de migration de données en bloc - Migration multiphase
description: Découvrez comment exécuter une migration multiphase avec l’outil de migration de données en bloc à l’aide du mode de maintenance lorsque votre source doit rester figée pendant le basculement de production.
feature: Data Import/Export
topic: Migration
role: Developer
doc-type: Technical Video
duration: 211
last-substantial-update: 2026-07-27T00:00:00Z
jira: KT-22157
source-git-commit: c3b81a5ffc652bc7ce7640b67fe5529067607251
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---


# Exécution d’une migration multiphase avec l’outil de migration de données en bloc

Exécutez une migration en plusieurs phases lorsque votre environnement source doit être figé pendant l’extraction, ce qui est idéal pour les basculements de production où les nouvelles commandes ne peuvent pas intervenir en milieu de migration. Il utilise le mode de maintenance et comporte cinq phases qui doivent s’exécuter dans l’ordre. Si votre source peut rester en ligne, regardez la vidéo de migration monophasée dans cette série à la place.

## À qui s&#39;adresse cette vidéo ?

* Architecte de solutions
* Ingénieur DevOps
* Développeur ou développeuse back-end

## Contenu vidéo

* Une distinction clé avant de commencer : les commandes `bin/console` s’exécutent sur l’outil de migration lui-même ; les commandes `bin/magento maintenance` s’exécutent sur votre serveur Commerce source. L’outil n’active ni ne désactive le mode de maintenance ; il s’agit d’une étape manuelle.
* La première phase s’exécute alors que la source est toujours active ; `bin console migration:before-maintenance` vérifie la configuration, initialise l’environnement, se connecte au système de gestion de contenu par composant, enregistre la migration, exécute des tests fonctionnels et crée des données de test synthétiques. N’activez pas le mode de maintenance avant la fin de cette phase.
* La troisième phase consiste à extraire les données d&#39;un environnement figé : `bin/console migration:during-maintenance` rouvre les tunnels PaaS si nécessaire, extrait les données de la source, nettoie les vues d&#39;évaluation, charge la cible ACCS, exécute la vérification et nettoie les données de test sur la cible.

>[!VIDEO](https://video.tv.adobe.com/v/3496413?learn=on)
