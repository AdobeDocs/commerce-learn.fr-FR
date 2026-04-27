---
title: Configuration d’Adobe Commerce
description: Découvrez comment configurer Adobe Commerce pour autoriser l’utilisation d’événements dans Adobe Developer App Builder.
landing-page-description: Découvrez comment configurer Adobe Commerce pour utiliser le mécanisme d’événement pour la consommation par Adobe Developer App Builder.
short-description: Découvrez comment configurer Adobe Commerce pour utiliser le mécanisme d’événement pour la consommation par Adobe Developer App Builder.
kt: 11889
doc-type: tutorial
duration: 299
audience: all
last-substantial-update: 2023-02-21T00:00:00.000Z
feature: App Builder, Configuration, Backend Development
topic: Commerce, Architecture
old-role: Architect, Developer
role: Developer, User
level: Beginner, Intermediate
exl-id: b8062042-2e90-4750-92ef-d55a76f2d842
TQID: https://experienceleague.adobe.com/6FBcDMDst5LBS7EyqA8zINr2Vs3bC--M7CXzIKf6EeQ
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 151
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

>[!VIDEO](https://video.tv.adobe.com/v/3415799?learn=on)

## Commandes utiles {#useful-commands}

```bash
bin/magento events:create-event-provider --label "my_provider" --description "Provides out-of-process extensibility for Adobe Commerce"

bin/magento events:subscribe observer.catalog_product_save_after --fields=name --fields=price
```

{{$include /help/_includes/io-events-related-links.md}}
