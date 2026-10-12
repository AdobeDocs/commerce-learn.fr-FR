---
title: Outil de migration de données en bloc - Migration monophasée
description: Découvrez comment exécuter une migration en une seule phase avec l’outil de migration de données en bloc pour les exécutions à sec et les environnements où la source peut rester active pendant l’extraction.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 737
last-substantial-update: 2026-07-24T00:00:00.000Z
jira: KT-22139
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
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

>[!VIDEO](https://video.tv.adobe.com/v/3496316?learn=on)
