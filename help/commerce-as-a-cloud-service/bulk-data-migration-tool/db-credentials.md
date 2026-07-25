---
title: Outil de migration en bloc de données - Informations d’identification de base de données
description: Découvrez comment configurer la connexion à la base de données source dans votre fichier .my.cnf à l’aide de l’interface de ligne de commande de Magento Cloud ou d’un ID de projet avant d’exécuter l’outil de migration.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 161
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22105
source-git-commit: 0dcb41e9138a36528f10333b0b5a9a9b2a39ed40
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
