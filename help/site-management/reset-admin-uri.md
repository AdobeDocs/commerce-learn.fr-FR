---
title: Réinitialiser l’URI d’administrateur à l’aide de l’interface de ligne de commande
description: Découvrez comment réinitialiser l’URI d’administrateur dans l’interface de ligne de commande Adobe Commerce Cloud. Cette méthode est pratique lorsque les modifications de l’URL d’administration provoquent des problèmes d’accès.
feature: Admin Workspace, Console
topic: Administration, Commerce
role: Developer, User
level: Beginner
doc-type: Technical Video
duration: 123
last-substantial-update: 2024-10-14T00:00:00Z
jira: KT-16338
exl-id: dbc155d7-8ce9-4622-abfb-1d8077c3a975
source-git-commit: 25ee35b730cc6265665a87c9c37d24e88c41b60e
workflow-type: tm+mt
source-wordcount: '107'
ht-degree: 0%

---

# Réinitialiser l’URI d’administration à l’aide de l’interface de ligne de commande

Découvrez comment réinitialiser l’URI d’administration à l’aide de la commande de l’interface de ligne de commande Adobe Commerce Cloud. Cela s’avère utile si l’URL d’administration a été modifiée à partir de l’administration, mais qu’une erreur a été commise et que vous ne pouvez plus accéder à l’administration.

>[!VIDEO](https://video.tv.adobe.com/v/3435066/?learn=on)

## Certaines commandes utilisées dans le tutoriel

Modifiez la configuration pour attendre une URL de chemin d’administration personnalisée à 0 :

`$ php bin/magento config:set admin/url/use_custom 0`

Remplacez la configuration de l’URL de chemin d’accès d’administration personnalisé par 0

`$ php bin/magento config:set admin/url/use_custom_path 0`
