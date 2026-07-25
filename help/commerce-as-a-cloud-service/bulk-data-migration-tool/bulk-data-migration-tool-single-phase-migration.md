---
title: Outil de migration de données en bloc - Migration monophasée
description: Découvrez comment exécuter une migration en une seule phase avec l’outil de migration de données en bloc pour les exécutions à sec et les environnements où la source peut rester active pendant l’extraction.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 737
last-substantial-update: 2026-07-24T00:00:00Z
jira: KT-22139
source-git-commit: 838387ffddbd8bee3ef3ec22694818eb2de5fe2d
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---

# Exécution d’une migration monophasée avec l’outil de migration de données en bloc

Exécutez une migration en une seule phase lorsque votre environnement source peut rester actif pendant l’extraction, ce qui est idéal pour les exécutions d’essai et les environnements de développement ou de sandbox. Si vous avez besoin d’une source figée, par exemple une transition de production où les nouvelles commandes ne peuvent pas arriver au milieu de la migration, regardez la vidéo sur la migration par phases dans cette série à la place.

## À qui s&#39;adresse cette vidéo ?

* Architecte de solutions
* Ingénieur DevOps
* Développeur ou développeuse back-end

## Contenu vidéo

* Créer l’image Docker avec `bin console build` — ne la réexécutez que si le fichier Docker est modifié.
* Pour lancer le gestionnaire de conteneurs de l’interface de ligne de commande CDMS, exécutez `bin console start`, puis ouvrez un conteneur une fois pour télécharger ses dépendances.
* Pour exécuter le pipeline complet en dix étapes, exécutez `bin console migration` : vérifiez la configuration, initialisez l’environnement, ouvrez les tunnels PaaS, exécutez des tests d’intégration, enregistrez-vous avec CDMS, analysez le schéma cible, générez des données de test, extrayez les données sources, chargez dans ACCS, vérifiez les sommes de contrôle, nettoyez et résumez.
* Vérifiez le rapport de synthèse de la migration : l’étape 8 (vérification de l’intégrité des données) consigne les échecs sans arrêter le pipeline, de sorte qu’une exécution terminée ne garantit pas une vérification correcte.
* Cette commande monophasée est un pipeline complet et autonome. N’utilisez pas cette commande comme étape dans le workflow du mode de maintenance (migration par phases), qui possède ses propres commandes dédiées.

>[!VIDEO](https://video.tv.adobe.com/v/3496318?captions=fre_fr&learn=on)
