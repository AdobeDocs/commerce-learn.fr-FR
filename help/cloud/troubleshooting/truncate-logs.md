---
title: Tronquer les logs
description: Découvrez comment trier un déploiement ayant échoué en raison d’un disque dur saturé en tronquant les fichiers journaux volumineux.
feature: Cloud, Site Management
topic: Commerce, Development
role: Architect, Developer
level: Beginner, Intermediate
doc-type: Technical Video
duration: 206
last-substantial-update: 2025-3-25
jira: KT-17595
source-git-commit: b90aa9eb8759391a16dfb29ca25b0d2d271956ed
workflow-type: tm+mt
source-wordcount: '187'
ht-degree: 0%

---

# Tronquer les logs

Découvrez comment effectuer un triage et un déploiement ayant échoué en raison d’un disque dur saturé. Découvrez comment rechercher et quelles commandes peuvent être exécutées pour libérer de l’espace dans votre environnement Adobe Commerce Cloud.

Si vous pensez avoir besoin de ces fichiers journaux, vous pouvez les `rsync` ou utiliser d’autres méthodes pour obtenir une copie disponible sur le serveur avant de les tronquer.

## À qui s’adresse cette vidéo ?

- Développeurs et professionnels de l’informatique
- Administrateurs système

## Contenu vidéo

- Diagnostiquer et résoudre un déploiement ayant échoué
- Emplacement de certains grands fichiers journaux courants
- Méthode rapide pour tronquer un fichier journal

>[!VIDEO](https://video.tv.adobe.com/v/3454572?learn=on)


## Commandes utilisées dans la vidéo

Pour vérifier l&#39;espace disque `df -h`. Faites attention à la ligne dev/mapper/xxxx

```SHELL
df -h

Filesystem                              Size  Used Avail Use% Mounted on
/dev/loop217                           1016M 1016M     0 100% /
none                                    492K  4.0K  488K   1% /dev
fake-sysfs                              120G     0  120G   0% /sys
tmpfs                                   120G     0  120G   0% /sys/fs/cgroup
tmpfs                                   384M     0  384M   0% /dev/shm
tmpfs                                    50M  460K   50M   1% /run
tmpfs                                   5.0M     0  5.0M   0% /run/lock
/dev/loop236                            144M  144M     0 100% /app
/dev/mapper/hyjh5nlaoabqtxxnh4opgjqzpu  4.9G  4.9G     0 100% /mnt
/dev/loop14                             8.0G  403M  7.6G   5% /tmp
/dev/mapper/platform-lxc                5.0T   69G  4.7T   2% /run/shared
```


Affichez les fichiers et leur taille dans un format lisible par l’utilisateur, tel que ko, mo et go à l’aide de la `ls -lah` de commande

```SHELL
ls -lah

total 4.7G
drwxr-xr-x 2 web web 4.0K Jul 16  2024 .
drwxr-xr-x 6 web web 4.0K Jan 10  2024 ..
-rw-rw-r-- 1 web web 487K Jul  5  2024 cache.log
-rw-r--r-- 1 web web 1.2K Jul 16  2024 cloud.error.log
-rw-r--r-- 1 web web 328K Mar 25 14:09 cloud.log
-rw-rw-r-- 1 web web 2.4G Jul  5  2024 cron.log
-rw-rw-r-- 1 web web  363 Dec  6  2023 debug.log
-rw-rw-r-- 1 web web  15K Jan 10  2024 indexation.log
-rw-r--r-- 1 web web 229K Jan 10  2024 install_upgrade.log
-rw-r--r-- 1 web web 2.9K Jul 16  2024 patch.log
-rw-rw-r-- 1 web web 2.4G Mar 25 15:36 support_report.log
-rw-rw-r-- 1 web web  516 Dec  6  2023 system.log
```

## Exemples de journal tronqué

Après avoir envoyé le fichier dans le projet et l’environnement appropriés, accédez au répertoire `var/log` . Vous pouvez ensuite tronquer un fichier avec un élément similaire à `> some-log-file.log`

```BASH
> support_report.log 
> cron.log 
```

## Documentation connexe

- [Notifications d’intégrité](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/dev-tools/integrations/health-notifications){target="_blank"}
