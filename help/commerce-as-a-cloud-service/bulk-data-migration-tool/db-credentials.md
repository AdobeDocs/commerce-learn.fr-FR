---
title: Outil de migration en bloc de données - Informations d’identification de base de données
description: Découvrez comment configurer la connexion à la base de données source dans votre fichier .my.cnf à l’aide de l’interface de ligne de commande de Magento Cloud ou d’un identifiant de projet avant d’exécuter l’outil de migration.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00.000Z
jira: KT-22105
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
source-wordcount: '151'
ht-degree: 0%
---
# Configuration des informations d’identification de base de données pour l’outil de migration de données en bloc

Configurez la connexion à la base de données source dans votre fichier `.my.cnf` avant d’exécuter l’outil de migration de données en bloc. Les étapes varient selon que votre environnement source est On-premise ou Adobe Commerce as a Cloud Service (PaaS).

## À qui s&#39;adresse cette vidéo ?

* Architecte de solutions
* Ingénieur DevOps
* Développeur ou développeuse back-end

## Contenu vidéo

* Copiez `.my.cnf.example` dans `.my.cnf` et créez une section nommée en fonction de votre connexion source.
* Définissez l’ID de projet dans `.my.cnf` si votre source est Adobe Commerce as a Cloud Service (PaaS).
* Utilisez les commandes du tunnel de l’interface de ligne de commande de Magento Cloud pour obtenir les valeurs de l’hôte, de l’utilisateur, du mot de passe, du port et de la base de données.
* Vérifiez la connectivité de l’hôte et du port avant d’exécuter l’outil si votre source est locale.

>[!VIDEO](https://video.tv.adobe.com/v/3496159?captions=fre_fr&learn=on)
