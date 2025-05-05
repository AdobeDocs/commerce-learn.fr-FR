---
title: Découvrez comment trouver des requêtes lentes dans les journaux de requête lents mysql et pourquoi la méthode de conception de réplication Galera DB peut en être la raison.
description: Galera DB dispose d’une méthode de conception qui rend la réplication des données vers des bases de données secondaires plus longue que la principale. Découvrez comment trouver ces événements dans le log de requête lente mysql, et la raison sous-jacente pour laquelle vous voyez des entrées dans les logs de requête lente et peut-être comment les empêcher à l’avenir.
kt: 13635
doc-type: video
activity: use
last-substantial-update: 2023-7-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
exl-id: 4a8a2df1-8cac-4bd9-851f-0eaae011b76c
source-git-commit: 598bff1fd2cefdc449d5ae3431401aec1e796313
workflow-type: tm+mt
source-wordcount: '304'
ht-degree: 0%

---

# Découvrez la réplication Galera DB et les requêtes lentes MySQL associées

Les grappes Galera aident à améliorer les performances et l’évolutivité. Lorsque vous prenez en compte les bases de données secondaires, il est important de comprendre comment la réplication des données se produit différemment de la base principale. La base de données principale peut effectuer des opérations en bloc. Lorsque la réplication se produit pour toutes les bases de données secondaires, elles effectuent des actions une par une. Si, par exemple, vous avez 67 000 000 éléments à supprimer, chacun d’eux se produit un par un dans les bases de données secondaires. Lorsque vous examinez les journaux de requête lente Mysql, vous constatez que cette action peut prendre beaucoup de temps. Comme les bases de données secondaires exécutent des tâches une par une, cela explique que les éléments ne soient pas synchronisés et que les impacts sur les performances peuvent être détectés.

Si possible, vous pouvez par lots vos opérations volumineuses afin d’aider les bases de données secondaires à rester synchronisées avec l’instance principale. En effectuant des actions par lots, vous pouvez exécuter les actions en temps voulu et limiter au minimum les impacts sur les performances.

## Pour qui est cette vidéo ?

- Architectes
- Développeurs
- Ops de développement

## Contenu vidéo

- Réplication Galera vers la base de données secondaire
- En savoir plus sur le contrôle de flux
- Recherche de numéros de thread dans les logs de requête lente mysql
- Les exécutions en bloc ne se produisent que sur l’instance principale. Les réplications se produisent 1 à la fois
- Lancer vos validations volumineuses pour aider la réplication à suivre l’activité principale

>[!VIDEO](https://video.tv.adobe.com/v/3423539?learn=on&captions=fre_fr)

## Ressources utiles

- [Grappe Galera](https://galeracluster.com/)
