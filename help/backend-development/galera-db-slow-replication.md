---
title: Découvrez comment trouver des requêtes lentes dans les journaux de requête lents mysql et pourquoi la méthode de conception de réplication Galera DB peut en être la raison.
description: Galera DB dispose d’une méthode de conception qui rend la réplication des données vers des bases de données secondaires plus longue que la Principale. Découvrez comment trouver ces événements dans le log de requête lente mysql, et la raison sous-jacente pour laquelle vous voyez des entrées dans les logs de requête lente et peut-être comment les empêcher à l’avenir.
kt: 13635
doc-type: video
activity: use
last-substantial-update: 2023-7-18
feature: Backend Development, Logs, Services
topic: Commerce, Development
role: Architect, Developer
level: Intermediate
source-git-commit: ae85993d98fb8620b763a212e5a2d7e53596870f
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 0%

---

# En savoir plus sur la réplication Galera DB et les requêtes lentes MySQL associées

Les grappes Galera aident à améliorer les performances et l’évolutivité. Lorsque vous prenez en compte les bases de données secondaires, il est important de comprendre comment la réplication des données se produit différemment de la Principale. La base de données Principale peut effectuer des opérations en bloc. Lorsque la réplication se produit pour toutes les bases de données secondaires, elles effectuent des actions une par une. Si, par exemple, vous avez 67 000 000 éléments à supprimer, chacun d’eux se produit un par un dans les bases de données secondaires. Lorsque vous examinez les journaux de requête lente Mysql, vous constatez que cette action peut prendre beaucoup de temps. Comme les bases de données secondaires exécutent des tâches une par une, c’est une raison pour laquelle les éléments ne sont pas synchronisés et les impacts sur les performances peuvent être détectés.

Si possible, vous pouvez par lots vos opérations volumineuses afin d’aider les bases de données secondaires à rester synchronisées avec la Principale. En effectuant des actions par lots, vous pouvez exécuter les actions en temps voulu et réduire au minimum les impacts sur les performances.

## Pour qui est cette vidéo ?

- Architectes
- Développeurs
- Ops de développement

## Contenu vidéo

- Réplication Galera vers la base de données secondaire
- En savoir plus sur le contrôle de flux
- Recherche de numéros de thread dans les logs de requête lente mysql
- Les exécutions en bloc ne se produisent que sur la Principale. Les réplications se produisent 1 à la fois
- Lancer vos validations volumineuses pour aider la réplication à suivre la Principale

>[!VIDEO](https://video.tv.adobe.com/v/3421688?learn=on)

## Ressources utiles

- [Grappe Galera](https://galeracluster.com/)
