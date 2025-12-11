---
title: Découvrez comment installer des événements IO pour Adobe Commerce 2.4.5
description: Découvrez comment installer les modules nécessaires aux événements IO dans Adobe Commerce 2.4.5 pour les utiliser dans Adobe Developer App Builder
landing-page-description: Découvrez comment installer plusieurs modules nécessaires à Adobe Commerce 2.4.5 à l’aide du compositeur.
short-description: Découvrez comment installer plusieurs modules nécessaires à Adobe Commerce 2.4.5 à l’aide du compositeur.
kt: 11886
doc-type: tutorial
audience: all
last-substantial-update: 2023-02-22T00:00:00Z
badge: Adobe Commerce 2.4.5
feature: App Builder, Eventing
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: e0adfd85-5a3d-44ba-aab5-ecd7c61715cf
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 0%

---

# Installation d’Adobe Commerce 2.4.5

Découvrez comment installer plusieurs nouveaux modules dans Adobe Commerce à l’aide du compositeur pour la version 2.4.5. Cela permet de configurer les modules nécessaires à l’utilisation de l’application Adobe Commerce. Consultez la documentation supplémentaire disponible dans [Installation de Adobe I/O Events pour Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## À qui s&#39;adresse cette vidéo ?

* Développeurs débutants avec Adobe Commerce et Adobe Developer App Builder à l’aide d’événements I/O

## Contenu vidéo {#video-content}

* Installation des modules requis à l’aide du compositeur
* Commandes à exécuter pour l’hébergement on-premise
* Commandes à exécuter pour Adobe Commerce Cloud
* Modification requise pour l’yaml Adobe Commerce Cloud

>[!VIDEO](https://video.tv.adobe.com/v/3419827?captions=fre_fr&quality=12&learn=on)

## Commandes utiles {#useful-commands}

Plusieurs commandes sont légèrement différentes selon que vous utilisez un environnement auto-hébergé ou Adobe Commerce Cloud.

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

`.magento.env.yaml` COMMERCE CLOUD :

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}
