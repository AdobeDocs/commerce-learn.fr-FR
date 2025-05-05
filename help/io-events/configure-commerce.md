---
title: Configuration d’Adobe Commerce
description: Découvrez comment configurer Adobe Commerce pour autoriser l’utilisation des événements dans Adobe Developer App Builder.
landing-page-description: Découvrez comment configurer Adobe Commerce pour utiliser le mécanisme d’événement à utiliser par Adobe Developer App Builder.
short-description: Découvrez comment configurer Adobe Commerce pour utiliser le mécanisme d’événement à utiliser par Adobe Developer App Builder.
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
role: Architect, Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# Configuration d’Adobe Commerce

Découvrez comment configurer Adobe Commerce pour exposer les événements. Documentation supplémentaire disponible à l’adresse [Installation des événements d’Adobe I/O pour Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## Pour qui est cette vidéo ?

* Les développeurs découvrent Adobe Commerce et Adobe Developer App Builder à l’aide d’événements I/O et doivent créer un projet App Builder d’Adobe.

## Contenu vidéo {#video-content}

* Configuration des événements d’Adobe I/O dans l’administrateur Commerce
* Enregistrement d’une clé privée dans l’administrateur Commerce
* Enregistrement de l’identifiant unique dans l’administrateur Commerce
* Création d’un fournisseur d’événements

>[!VIDEO](https://video.tv.adobe.com/v/3419713?quality=12&learn=on&captions=fre_fr)

## Commandes utiles {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
