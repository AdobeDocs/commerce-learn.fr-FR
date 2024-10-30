---
title: Diagnostic et correction de quelques erreurs de Commerce Cloud courantes
description: Résolvez deux erreurs courantes du projet Adobe Cloud qui empêchent le chargement du site.
feature: Cloud, Site Management
topic: Commerce, Development
role: Architect, Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 260
last-substantial-update: 2024-10-30T00:00:00Z
jira: KT-16419
source-git-commit: 27c1715dd42853013181d9c729549a5a32bc2af0
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---


# Service de diagnostic et de correction indisponible et une erreur s’est produite

Découvrez comment trier et résoudre deux erreurs courantes affichées sur les projets Adobe Commerce Cloud.  Découvrez comment et pourquoi ces erreurs se produisent et quelles sont les étapes recommandées pour les résoudre.

## Pour qui cette vidéo est-elle ?

- Développeurs et professionnels de l’informatique
- Administrateurs système

## Contenu vidéo

- Diagnostic et résolution des problèmes de stockage :
- Gestion du mode de maintenance
- Conseils de dépannage efficaces

>[!VIDEO](https://video.tv.adobe.com/v/3435766?learn=on)


## Commandes utilisées dans la vidéo

Recherchez les 5 dernières lignes du journal des exceptions mentionnées dans le message de réponse.

```SHELL
 tail -n 5 ~/var/log/exception.log
```

Pour vérifier l’espace du disque dur. Attention à la ligne dev/mapper/xxxx

```SHELL
df -h
```

Recherche les 15 fichiers les plus volumineux

```SHELL
find -type f -exec du -Sh {} + | sort -rh | head -n 15
```

Afficher le statut du mode de maintenance

```SHELL
php bin/magento maintenance:status
```

Désactiver le mode de maintenance

```SHELL
php bin/magento maintenance:disable 
```
