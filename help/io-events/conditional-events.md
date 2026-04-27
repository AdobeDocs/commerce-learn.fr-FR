---
title: Découvrez comment utiliser des événements conditionnels dans Adobe Commerce
description: Découvrez comment utiliser des événements conditionnels à utiliser dans Adobe Developer App Builder.
landing-page-description: Découvrez comment utiliser les événements conditionnels Adobe Commerce.
short-description: Découvrez comment utiliser les événements conditionnels Adobe Commerce.
kt: 11890
doc-type: tutorial
duration: 421
audience: all
last-substantial-update: 2023-02-21T00:00:00.000Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 03787aa3-051b-4a35-b2e8-ecf6762b5eb4
TQID: https://experienceleague.adobe.com/GuN9--5xQaBnbFvQrkGuqMcDMMe-1tVGPk-hvqs4-UY
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 144
ht-degree: 0%

---

# Événements conditionnels Adobe Commerce

Découvrez les événements conditionnels dans Adobe Commerce qui peuvent être utilisés dans Adobe Developer App Builder. Consultez la documentation supplémentaire disponible dans [Installation de Adobe I/O Events pour Adobe Commerce](https://developer.adobe.com/commerce/extensibility/events/conditional-events/){target="_blank"}.

## À qui s&#39;adresse cette vidéo ?

* Les développeurs qui découvrent Adobe Commerce et Adobe Developer App Builder à l’aide d’événements I/O doivent créer un projet Adobe App Builder.

## Contenu vidéo {#video-content}

* Découvrez les événements conditionnels
* Découvrez l’utilisation appropriée du nouveau fichier XML io_events.xml
* Découvrez comment configurer des événements conditionnels
* Définir des règles à utiliser dans des événements conditionnels
* Découvrez comment enregistrer des événements dans le `app/etc/config.php` des instances Commerce

>[!VIDEO](https://video.tv.adobe.com/v/3419799?captions=fre_fr&learn=on)

## Commandes utiles {#useful-commands}

```bash
bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id

bin/magento events:subscribe plugin.magento.catalog.model.resource_model.product.save_low_stock --parent=plugin.magento.catalog.model.resource_model.product.save --fields=sku --fields=qty --fields=category_id --rules="qty|lessThan|20" --rules="category_id|in|3,4,5"

cat app/etc/config.php

bin/magento events:list

bin/magento events:list -v
```

{{$include /help/_includes/io-events-related-links.md}}
