---
title: Découvrez comment installer des événements d’E/S pour Adobe Commerce 2.4.6
description: Découvrez comment installer les modules nécessaires aux événements d’E/S dans Adobe Commerce 2.4.6 pour une utilisation dans Adobe Developer App Builder
landing-page-description: Découvrez comment installer plusieurs modules nécessaires à Adobe Commerce 2.4.6.
short-description: Learn how to install several modules needed for Adobe Commerce 2.4.6.
kt: 11887
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: "Adobe Commerce 2.4.6"
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Installation d’Adobe Commerce 2.4.6

Découvrez comment installer plusieurs nouveaux modules dans Adobe Commerce à l’aide du compositeur pour la version 2.4.6. Vous trouverez une documentation supplémentaire à l’adresse [Installation des événements d’Adobe I/O pour Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## Pour qui est cette vidéo ?

* Les développeurs découvrent Adobe Commerce et Adobe Developer App Builder à l’aide des événements I/O.

## Contenu vidéo {#video-content}

* Commandes à exécuter pour l’hébergement on-premise
* Commandes à exécuter pour Adobe Commerce Cloud
* Modification requise du journal Adobe Commerce Cloud

>[!VIDEO](https://video.tv.adobe.com/v/3415795?quality=12&learn=on)

## Commandes utiles {#useful-commands}

Différentes commandes diffèrent légèrement selon que vous utilisez un environnement auto-hébergé ou Adobe Commerce Cloud.

### Hébergement On-Premise {#on-premise}

```bash
bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce on Cloud {#adobe-commerce-cloud}

```bash
composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
