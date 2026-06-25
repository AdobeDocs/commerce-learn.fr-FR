---
title: Découvrez comment installer des événements IO pour Adobe Commerce 2.4.6
description: Découvrez comment installer les modules nécessaires aux événements IO dans Adobe Commerce 2.4.6 pour les utiliser dans Adobe Developer App Builder
landing-page-description: Découvrez comment installer plusieurs modules nécessaires pour Adobe Commerce 2.4.6.
short-description: Découvrez comment installer plusieurs modules nécessaires pour Adobe Commerce 2.4.6.
kt: 11887
doc-type: tutorial
duration: 167
audience: all
last-substantial-update: 2023-02-22T00:00:00.000Z
badge: Adobe Commerce 2.4.6
feature: App Builder, Eventing
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 41b31ed8-04c5-4d50-aaff-abc3718b5957
TQID: https://experienceleague.adobe.com/19IVX54xo-RAJAOuXNIABdy11ExmBRAbWugi4eZ0cF0
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: 166
ht-degree: 0%

---

# Installation d’Adobe Commerce 2.4.6

Découvrez comment installer plusieurs nouveaux modules dans Adobe Commerce à l’aide du compositeur pour la version 2.4.6. Consultez la documentation supplémentaire disponible dans [Installation de Adobe I/O Events pour Adobe Commerce](https://developer.adobe.com/commerce/events/get-started/installation/){target="_blank"}.

## À qui s&#39;adresse cette vidéo ?

* Les développeurs qui découvrent Adobe Commerce et Adobe Developer App Builder à l’aide d’événements I/O

## Contenu vidéo {#video-content}

* Commandes à exécuter pour l’hébergement on-premise
* Commandes à exécuter pour Adobe Commerce Cloud
* Modification requise pour l’yaml Adobe Commerce Cloud

>[!VIDEO](https://video.tv.adobe.com/v/3415795?learn=on)

## Commandes utiles {#useful-commands}

Plusieurs commandes sont légèrement différentes selon que vous utilisez un environnement auto-hébergé ou Adobe Commerce Cloud.

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

`.magento.env.yaml` COMMERCE CLOUD :

```yaml
stage:
  global:
    ENABLE_EVENTING: true
```

{{$include /help/_includes/io-events-related-links.md}}

