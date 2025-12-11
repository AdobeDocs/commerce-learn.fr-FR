---
title: Configuration d’Adobe Commerce
description: Découvrez comment configurer Adobe Commerce pour autoriser l’utilisation d’événements dans Adobe Developer App Builder.
landing-page-description: Découvrez comment configurer Adobe Commerce pour utiliser le mécanisme d’événement pour la consommation par Adobe Developer App Builder.
short-description: Découvrez comment configurer Adobe Commerce pour utiliser le mécanisme d’événement pour la consommation par Adobe Developer App Builder.
kt: 11889
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-21T00:00:00Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# Configuration d’Adobe Commerce

Découvrez comment configurer Adobe Commerce pour exposer les événements. Consultez la documentation supplémentaire disponible dans [Installation de Adobe I/O Events pour Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## À qui s&#39;adresse cette vidéo ?

* Les développeurs qui découvrent Adobe Commerce et Adobe Developer App Builder à l’aide d’événements I/O doivent créer un projet Adobe App Builder.

## Contenu vidéo {#video-content}

* Configuration des événements Adobe I/O dans l’administration Commerce
* Enregistrement d’une clé privée dans l’administrateur Commerce
* Enregistrer l’identifiant unique dans l’administrateur Commerce
* Créer un fournisseur d’événements

>[!VIDEO](https://video.tv.adobe.com/v/3419713?captions=fre_fr&quality=12&learn=on)

## Commandes utiles {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
