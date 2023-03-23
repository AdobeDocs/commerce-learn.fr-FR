---
title: Fichier app.config.yaml
description: Découvrez les types de fichiers du fichier app.config.yaml pour cet exemple d’application.
landing-page-description: Découvrez Adobe Developer App Builder utilisé avec Adobe Commerce et les types de fichiers contenus dans app.config.yaml.
kt: 12426
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
source-git-commit: e0371a8cefab0141318daa0e1be42bfbb9e5b608
workflow-type: tm+mt
source-wordcount: '105'
ht-degree: 0%

---


# Description et utilisation du fichier app.config.yaml {#app-config-yaml}

Ce fichier détermine la configuration de l’application.

## Pour qui est cette vidéo ?

* Les développeurs qui découvrent Adobe Commerce avec une expérience limitée du créateur d’applications Adobe et qui découvrent les `app.config.yaml` dans l’exemple d’application.

## Contenu vidéo

* Le `app.config.yaml` fichier discuté
* Comment les définitions sont-elles liées à d’autres `.js` files

>[!VIDEO](https://video.tv.adobe.com/v/3416592)

## Exemple de code

```bash
# Specify your secrets here
# This file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can also include commerce OAuth credentials, our sample module will use the following example credentials:
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

Vous pouvez voir ces valeurs statiques utilisées dans l’exemple de module dans le fichier . `actions/commerce.index.js`

```javascript
        const oauth = getCommerceOauthClient(
            {
                url: params.COMMERCE_BASE_URL,
                consumerKey: params.COMMERCE_CONSUMER_KEY,
                consumerSecret: params.COMMERCE_CONSUMER_SECRET,
                accessToken: params.COMMERCE_ACCESS_TOKEN,
                accessTokenSecret: params.COMMERCE_ACCESS_TOKEN_SECRET
            },
            logger
        )
```

{{$include /help/_includes/app-builder-first-app-related-links.md}}
