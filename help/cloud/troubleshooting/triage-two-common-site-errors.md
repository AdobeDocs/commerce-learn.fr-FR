---
title: Diagnostiquer et corriger quelques erreurs Commerce Cloud courantes
description: Résolvez deux erreurs courantes de projet Adobe Cloud qui empêchent le chargement du site.
feature: Cloud, Site Management
topic: Commerce, Development
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 260
last-substantial-update: 2024-10-30T00:00:00Z
jira: KT-16419
exl-id: 4c21b6a6-783a-422f-9071-3534ed68e8be
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 0%

---

# Diagnostiquer et corriger le service indisponible et une erreur s’est produite

Découvrez comment trier et résoudre deux erreurs courantes affichées sur les projets Adobe Commerce Cloud.  Découvrez comment et pourquoi ces erreurs se produisent et quelles sont les étapes recommandées pour les résoudre.

## À qui s’adresse cette vidéo ?

- Développeurs et professionnels de l’informatique
- Administrateurs système

## Contenu vidéo

- Diagnostic et résolution des problèmes de stockage :
- Gérer le mode de maintenance
- Conseils de dépannage efficaces

>[!VIDEO](https://video.tv.adobe.com/v/3435766?learn=on)


## Commandes utilisées dans la vidéo

Recherchez les 5 dernières lignes du journal des exceptions mentionnées dans le message de réponse.

```SHELL
 tail -n 5 ~/var/log/exception.log
```

Pour vérifier l&#39;espace disque. Faites attention à la ligne dev/mapper/xxxx

```SHELL
df -h
```

Permet de trouver les 15 fichiers les plus volumineux

```SHELL
find -type f -exec du -Sh {} + | sort -rh | head -n 15
```

Affichage de l’état du mode de maintenance

```SHELL
php bin/magento maintenance:status
```

Désactivation du mode de maintenance

```SHELL
php bin/magento maintenance:disable 
```
