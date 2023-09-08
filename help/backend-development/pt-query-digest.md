---
title: Découvrez comment fonctionne Percona Toolkit pt-query-digest et pourquoi il est utilisé
description: Analysez les requêtes MySQL à partir de fichiers journaux lents, généraux et binaires. Il peut également analyser les requêtes provenant des données de protocole SHOW PROCESSLIST et MySQL issues de tcpdump.
kt: 13846
doc-type: video
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: 709619c8c1776c6da6575ae79b6ccb671700c365
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

Découvrez pourquoi vous utilisez pt-query-digest et quelques exemples concrets pour approfondir le raisonnement.

## Pour qui est cette vidéo ?

- Architectes
- Développeurs
- Ops de développement

## Contenu vidéo

- En savoir plus sur l’utilisation de pt-query-digest
- Découvrez les avantages et les défauts de cette fonctionnalité du kit d&#39;outils Percona
- Comprendre les résultats et découvrir quelles sont les étapes de performances possibles à envisager

>[!VIDEO](https://video.tv.adobe.com/v/3423480?learn=on)

## Références de code

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## Ressources utiles

- [Percona Toolkit](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
- [Deadlocks dans MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/deadlocks-in-mysql.html){target="_blank"}
