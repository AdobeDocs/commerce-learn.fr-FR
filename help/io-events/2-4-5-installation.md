---
title: Découvrez comment installer des événements d’E/S pour Adobe Commerce 2.4.5
description: Découvrez comment installer les modules nécessaires aux événements d’E/S dans Adobe Commerce 2.4.5 pour une utilisation dans Adobe Developer App Builder
landing-page-description: Découvrez comment installer plusieurs modules nécessaires à Adobe Commerce 2.4.5 à l’aide du compositeur.
short-description: Découvrez comment installer plusieurs modules nécessaires à Adobe Commerce 2.4.5 à l’aide du compositeur.
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.5
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
source-git-commit: edb98cf6544954d741c43beb39f4056326c7d26b
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 0%

---

# Installation d’Adobe Commerce 2.4.5

Découvrez comment installer plusieurs nouveaux modules dans Adobe Commerce à l’aide du compositeur pour la version 2.4.5. Cela configure les modules requis à utiliser dans l’application Adobe Commerce. Documentation supplémentaire disponible à l’adresse [Installation des événements d’Adobe I/O pour Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## Pour qui est cette vidéo ?

* Les développeurs découvrent Adobe Commerce et Adobe Developer App Builder à l’aide des événements I/O

## Contenu vidéo {#video-content}

* Installation des modules requis à l’aide du compositeur
* Commandes à exécuter pour l’hébergement on-premise
* Commandes à exécuter pour Adobe Commerce Cloud
* Modification requise du journal Adobe Commerce Cloud

>[!VIDEO](https://video.tv.adobe.com/v/3415794?quality=12&learn=on)

## Commandes utiles {#useful-commands}

Différentes commandes diffèrent légèrement selon que vous utilisez un environnement auto-hébergé ou Adobe Commerce Cloud.

### Hébergement On-Premise {#on-premise}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

bin/magento events:generate:module

bin/magento module:enable --all

bin/magento setup:upgrade && bin/magento setup:di:compile
```

### Adobe Commerce on Cloud {#adobe-commerce-cloud}

```bash
composer require magento/commerce-eventing=^1.0 --no-update

composer update

composer info magento/ece-tools
```

Commerce Cloud `.magento.env.yaml`:

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
