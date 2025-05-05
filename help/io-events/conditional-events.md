---
title: Découvrez comment utiliser des événements conditionnels dans Adobe Commerce
description: Découvrez comment utiliser des événements conditionnels dans Adobe Developer App Builder.
landing-page-description: Découvrez comment utiliser les événements conditionnels Adobe Commerce.
short-description: Découvrez comment utiliser les événements conditionnels Adobe Commerce.
kt: 11890
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
source-git-commit: e02da0901c01871360bcc556666e310acd7c7224
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 0%

---

# Événements conditionnels Adobe Commerce

Découvrez les événements conditionnels dans Adobe Commerce pouvant être utilisés dans Adobe Developer App Builder. Documentation supplémentaire disponible à l’adresse [Installation des événements d’Adobe I/O pour Adobe Commerce](https://developer.adobe.com/commerce/extensibility/events/conditional-events/){target="_blank"}.

## Pour qui est cette vidéo ?

* Les développeurs découvrent Adobe Commerce et Adobe Developer App Builder à l’aide d’événements I/O et doivent créer un projet App Builder d’Adobe.

## Contenu vidéo {#video-content}

* En savoir plus sur les événements conditionnels
* Découvrez l’utilisation appropriée du nouveau fichier XML io_events.xml
* Découvrez comment configurer des événements conditionnels
* Définition de règles à utiliser dans des événements conditionnels
* Découvrez comment enregistrer des événements dans les instances Commerce `app/etc/config.php`

>[!VIDEO](https://video.tv.adobe.com/v/3419799?quality=12&learn=on&captions=fre_fr)

## Commandes utiles {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
