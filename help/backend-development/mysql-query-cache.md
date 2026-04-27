---
title: Learn how mysql query caching
description: Sometimes mysql queries get backed up waiting for a lock. This tutorial explains what is query caching and some recommendations for settings if you have issues.
kt: 13690
doc-type: video
duration: 444
activity: use
last-substantial-update: 2023-7-27
feature: Backend Development, Cache, Logs
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 8d3b0ec2-e80c-4457-b924-69e8b8cedf03
TQID: https://experienceleague.adobe.com/W91-fJGZtgfpp03ZtYmSh97oNqmCFpF8AvkTYDaOB-g
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 165
ht-degree: 0%

---

# Learn about mysql query caching

Learn what MySQL query cache is and some basic understanding for how it works. Learn how to detect an issue with mysql query caching, by finding &quot;waiting for query cache lock&quot; appearing in a high volume in the mysql slow query logs.

## À qui s&#39;adresse cette vidéo ?

* Architectes
* Développeur
* Opérations de développement

## Contenu vidéo

* Learn about query caching
* How to detect if your query cache settings may be an issue by finding &quot;waiting for query cache lock&quot;
* See how the SQL is saved and used in finding a matching query cache
* Some tips on configuration settings

>[!VIDEO](https://video.tv.adobe.com/v/3422015?learn=on)

## Ressources utiles

* [General MySQL guidelines](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql.html?lang=en){target="_blank"}
* [Galera replication and slow queries](https://experienceleague.adobe.com/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication.html){target="_blank"}
