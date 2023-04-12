---
title: Découvrez comment créer un module dans Adobe Commerce pour utiliser les événements.
description: Découvrez comment créer un module Commerce pour utiliser les événements.
landing-page-description: Découvrez comment créer un module Adobe Commerce pour utiliser les événements.
short-description: Découvrez comment créer un module Adobe Commerce pour utiliser les événements.
kt: 11891
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 0%

---

# Développement de module Adobe Commerce

Découvrez comment enregistrer des événements, rechercher des événements pris en charge et utiliser un nouveau fichier XML `io_events.xml` dans le développement de modules personnalisés. La vidéo montrera également aux développeurs comment trouver des événements enregistrés qui peuvent être utilisés et désabonner les événements qui peuvent déjà être définis. Documentation supplémentaire disponible à l’adresse [Installation des événements d’Adobe I/O pour Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## Pour qui est cette vidéo ?

* Les développeurs découvrent Adobe Commerce et Adobe Developer App Builder à l’aide d’événements I/O.

## Contenu vidéo {#video-content}

* Enregistrement d’événements dans Commerce en vue d’une utilisation dans Adobe Developer App Builder
* Identifier les événements qui peuvent être enregistrés
* Découvrez comment enregistrer des événements dans io_events.xml
* Découvrez comment enregistrer des événements dans les instances de commerce `app/etc/config.php`
* Découvrez comment vous désabonner d’un événement

>[!VIDEO](https://video.tv.adobe.com/v/3415802?quality=12&learn=on)

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
