---
title: Découvrez comment fonctionne le pt-query-digest de Percona Toolkit et pourquoi il est utilisé
description: Analysez les requêtes MySQL à partir de fichiers journaux lents, généraux et binaires. Il peut également analyser les requêtes provenant de « SHOW PROCESSLIST » et les données du protocole MySQL de tcpdump.
kt: 13846
doc-type: video
activity: use
last-substantial-update: 2023-8-28
feature: Backend Development, Tools and External Services, Logs
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 77e91f1b-b3ae-4c6d-bb6d-4fd7ebbb0baf
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '98'
ht-degree: 0%

---

# Percona Toolkit pt-query-digest

Découvrez pourquoi vous utilisez pt-query-digest et quelques exemples réels pour aider à approfondir le raisonnement.

## À qui s&#39;adresse cette vidéo ?

- Architectes
- Développeur
- Opérations de développement

## Contenu vidéo

- En savoir plus sur l’utilisation de pt-query-digest
- Découvrez les avantages et les inconvénients de cette fonctionnalité Percona Toolkit
- Découvrez les résultats et les étapes de performances possibles à envisager

>[!VIDEO](https://video.tv.adobe.com/v/3452291?captions=fre_fr&learn=on)

## Références de code

```MYSQL
Be sure to change to match your logs and time frame

$ pt-query-digest mysql-slow.log.7 > mysql-slow.log.7.DIGEST
```

## Ressources utiles

- [Percona Toolkit](https://docs.percona.com/percona-toolkit/pt-query-digest.html){target="_blank"}
