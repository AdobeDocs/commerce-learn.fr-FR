---
title: Découvrez comment créer un module dans Adobe Commerce pour utiliser les événements.
description: Découvrez comment créer un module Commerce pour utiliser les événements.
landing-page-description: Découvrez comment créer un module Adobe Commerce pour utiliser les événements.
short-description: Découvrez comment créer un module Adobe Commerce pour utiliser les événements.
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Développement de modules Adobe Commerce

Découvrez comment enregistrer des événements, trouver des événements pris en charge et comment utiliser un nouveau `io_events.xml` de fichier XML dans le développement de modules personnalisés. La vidéo explique également aux développeurs comment rechercher des événements enregistrés qui peuvent être utilisés, ainsi que comment désabonner tous les événements qui peuvent déjà être définis. Consultez la documentation supplémentaire disponible dans [Installation de Adobe I/O Events pour Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## À qui s&#39;adresse cette vidéo ?

* Développeurs débutants avec Adobe Commerce et Adobe Developer App Builder utilisant des événements I/O.

## Contenu vidéo {#video-content}

* Enregistrement d’événements dans Commerce en vue de leur utilisation dans Adobe Developer App Builder
* Identifier les événements pouvant être enregistrés
* Découvrez comment enregistrer des événements dans io_events.xml
* Découvrez comment enregistrer des événements dans le `app/etc/config.php` des instances Commerce
* Découvrez comment vous désabonner d’un événement.

>[!VIDEO](https://video.tv.adobe.com/v/3419835?captions=fre_fr&quality=12&learn=on)

## Commandes utiles {#useful-commands}

```bash
bin/magento events:list:all Magento_Catalog

bin/magento events:info plugin.magento.catalog.api.category_repository.save

bin/magento events:subscribe observer.catalog_category_save_after --fields=entity_id --fields=parent_id

cat app/etc/config.php

bin/magento events:unsubscribe observer.catalog_category_save_after

bin/magento events:list
```

{{$include /help/_includes/io-events-related-links.md}}
