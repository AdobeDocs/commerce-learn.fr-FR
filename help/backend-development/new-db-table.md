---
title: Ajouter une table à une base de données
description: "[!DNL Commerce] dispose d’un mécanisme spécial qui permet de créer des tables de base de données, de modifier les tables existantes et même d’y ajouter des données."
kt: 5613
doc-type: video
activity: use
last-substantial-update: 2023-2-10
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, Developer
level: Beginner, Intermediate
exl-id: fb222752-5689-4f87-94cf-a61ed7005e6b
source-git-commit: 79529c8d77df74e6f77ab3a01b45541a38dbf680
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Ajouter une table à une base de données

>[!IMPORTANT]
>
>Ce n’est plus recommandé. Reportez-vous à https://developer.adobe.com/commerce/php/development/components/declarative-schema/


[!DNL Commerce] dispose d’un mécanisme spécial qui permet de créer des tables de base de données, de modifier les tables existantes et même d’y ajouter certaines données, comme les données de configuration, qui doivent être ajoutées lorsqu’un module est installé. Ce mécanisme permet de transférer ces modifications entre différentes installations.

Au lieu d’effectuer des opérations SQL manuelles à plusieurs reprises lors de la réinstallation du système, les développeurs créent un script d’installation (ou de mise à niveau) contenant les données. Le script s’exécute chaque fois qu’un module est installé.

## Pour qui est cette vidéo ?

- Développeurs

## Contenu vidéo

- Création d’un module
- Création d’un script InstallSchema
- Création d’un script InstallData
- Ajouter un nouveau module et vérifier qu’une table contenant des données a été créée
- Création d’un script UpgradeSchema
- Création d’un script UpgradeData
- Exécutez les scripts de mise à niveau et vérifiez que le tableau a été modifié.

>[!VIDEO](https://video.tv.adobe.com/v/35791?quality=12&learn=on)

## Ressources utiles

- [Migration des scripts d’installation/de mise à niveau vers un schéma déclaratif](https://developer.adobe.com/commerce/php/development/components/declarative-schema/migration-scripts/)
