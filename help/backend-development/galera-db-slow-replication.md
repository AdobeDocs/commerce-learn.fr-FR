---
title: Découvrez comment trouver les requêtes lentes dans les journaux de requêtes lentes mysql et pourquoi la méthode de conception de réplication Galera DB peut en être la raison
description: La base de données Galera possède une méthode de conception qui fait que la réplication des données vers des bases de données secondaires prend plus de temps que la base de données principale. Découvrez comment trouver ces événements dans le journal de requêtes lentes mysql, et la raison sous-jacente pour laquelle vous voyez des entrées dans les journaux de requêtes lentes et peut-être comment les empêcher à l’avenir.
kt: 13635
doc-type: video
activity: use
last-substantial-update: 2023-7-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Découvrez la réplication de la base de données Galera et les requêtes lentes MySQL associées

Les clusters Galera améliorent les performances et l’évolutivité. Lorsque vous envisagez des bases de données secondaires, il est important de comprendre que le mode de réplication des données est différent de celui utilisé sur la base principale. La base de données principale peut effectuer des opérations en bloc. Lorsque la réplication se produit pour toutes les bases de données secondaires, elles effectuent des actions une par une. Par exemple, si une suppression porte sur 67 000 000 d’éléments, dans les bases de données secondaires, chacun d’eux se produit un par un. En examinant les journaux de requêtes lentes Mysql, vous constatez que cette action peut prendre beaucoup de temps. Comme les bases de données secondaires exécutent des tâches une par une, cela explique que les tâches ne soient pas synchronisées et que des impacts sur les performances puissent être détectés.

Si possible, la solution consiste à regrouper les opérations volumineuses pour aider les bases de données secondaires à rester synchronisées avec la base de données principale. En procédant par lots, les actions peuvent être exécutées en temps opportun et les impacts sur les performances sont réduits au minimum.

## À qui s&#39;adresse cette vidéo ?

- Architectes
- Développeur
- Opérations de développement

## Contenu vidéo

- Réplication de la galerie vers la base de données secondaire
- En savoir plus sur le contrôle de flux
- Recherche de numéros de threads dans les logs de requêtes lentes mysql
- Les exécutions en bloc ne se produisent que sur l’instance principale. Les réplications se produisent 1 par
- Effectuez des validations par lots volumineuses pour aider la réplication à suivre le processus principal.

>[!VIDEO](https://video.tv.adobe.com/v/3423539?captions=fre_fr&learn=on)

## Ressources utiles

- [Cluster Galera](https://galeracluster.com/)
