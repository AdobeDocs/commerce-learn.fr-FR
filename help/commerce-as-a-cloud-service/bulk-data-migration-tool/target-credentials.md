---
title: Outil de migration de données en bloc - Informations d’identification de Target
description: Découvrez comment configurer les URL des instances cibles, les informations d’identification Adobe IMS et les paramètres CDMS dans votre fichier .env avant d’exécuter l’outil de migration de données en bloc.
role: Developer
level: Intermediate
doc-type: Technical Video
topic: Migration
feature: Data Import/Export
duration: 226
last-substantial-update: 2026-07-21T00:00:00Z
jira: KT-22107
source-git-commit: b3c029f7c1080550900cbc5838478cd7a4137a20
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 0%

---

# Configuration des informations d’identification de Target pour l’outil de migration de données en bloc

Définissez les URL de l’instance cible, les informations d’identification Adobe IMS et les paramètres CDMS dans votre fichier `.env` avant d’exécuter l’outil de migration de données en bloc. Assurez-vous que votre URL Adobe IMS, votre URL cible et votre hôte CDMS correspondent tous au même niveau d’environnement (évaluation ou production).

## À qui s&#39;adresse cette vidéo ?

* Architecte de solutions
* Ingénieur DevOps
* Développeur ou développeuse back-end

## Contenu vidéo

* Définissez les URL REST et GraphQL de l’instance cible et l’ID du client cible dans le fichier `.env`, à l’aide des valeurs du panneau d’informations de l’instance sur experience.adobe.com.
* Définissez l’URL Adobe IMS pour qu’elle corresponde à votre niveau d’environnement (évaluation ou production) et à votre région.
* Récupérez l’identifiant client et le secret client Adobe IMS depuis **Project** > **OAuth de serveur à serveur** dans Adobe Developer Console.
* Copiez l’ID d’organisation cible et configurez les paramètres de l’hôte, du port et du serveur local du système de gestion de contenu de documents pour qu’ils correspondent à votre environnement.

>[!VIDEO](https://video.tv.adobe.com/v/3496167?learn=on)
