---
title: Découvrez comment créer un module dans Adobe Commerce pour utiliser les événements.
description: Découvrez comment créer un module Commerce pour utiliser les événements.
landing-page-description: Découvrez comment créer un module Adobe Commerce pour utiliser les événements.
short-description: Découvrez comment créer un module Adobe Commerce pour utiliser les événements.
kt: 11891
doc-type: tutorial
duration: 348
audience: all
last-substantial-update: 2023-02-21T00:00:00.000Z
feature: App Builder, Eventing, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: e8103fe0-116a-499c-ae0a-3ad0511f44d0
TQID: https://experienceleague.adobe.com/bRnOh6fnsyTY-21f81vIXV4-eeitLXQWsAbjg2rx-Is
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: 173
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

>[!VIDEO](https://video.tv.adobe.com/v/3415802?learn=on)

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

