---
title: Analysez les requêtes MySQL avec Percona Toolkit pt-query-digest
description: Découvrez comment utiliser pt-query-digest pour analyser les requêtes MySQL à partir de fichiers journaux lents, généraux et binaires, ainsi que les données de protocole SHOW PROCESSLIST et MySQL de tcpdump.
doc-type: Technical Video
duration: 506
last-substantial-update: 2023-08-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
role: Developer
level: Intermediate
jira: KT-13846
exl-id: 77e91f1b-b3ae-4c6d-bb6d-4fd7ebbb0baf
TQID: https://experienceleague.adobe.com/lh-fBjlhZO6W-K08KNb-KaG-N2slLZVpNOSg6LAp0n8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
source-git-commit: add3e29f8841ca4ca99f4c40afc656f00e93ec36
workflow-type: tm+mt
source-wordcount: 105
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

Découvrez pourquoi utiliser pt-query-digest et quelques exemples pratiques pour améliorer l’analyse.

## Audience prévue

* Architectes
* Développeur
* Opérations de développement

## Contenu vidéo

* En savoir plus sur l’utilisation de pt-query-digest
* Découvrez les avantages et les inconvénients de cette fonctionnalité Percona Toolkit
* Découvrez les résultats et les étapes de performances possibles à envisager

>[!VIDEO](https://video.tv.adobe.com/v/3423480?learn=on)

## Références de code

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## Ressources utiles

* [Percona Toolkit](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
