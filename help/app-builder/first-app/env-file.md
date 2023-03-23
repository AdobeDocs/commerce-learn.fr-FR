---
title: Le fichier .env
description: Découvrez les types de fichiers du fichier .env pour cet exemple d’application
landing-page-description: Découvrez Adobe Developer App Builder utilisé avec Adobe Commerce et les types de contenu utilisés dans le fichier .env .
kt: 12423
doc-type: tutorial
audience: all
last-substantial-update: 2023-03-13T00:00:00Z
source-git-commit: e0371a8cefab0141318daa0e1be42bfbb9e5b608
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---


# Génération et configuration du fichier .env {#env-file}

Le `.env` est un fichier spécial qui ne fait pas partie de l’exemple de module, mais qui est important pour votre application Adobe Developer App Builder. Ce fichier contient des secrets et d&#39;autres informations. Évitez de soumettre ce fichier à un référentiel de code.

## Pour qui est cette vidéo ?

* Les développeurs qui découvrent Adobe Commerce avec une expérience limitée de l’utilisation d’Adobe App Builder pour en savoir plus sur le `.env` fichier .

## Contenu vidéo

* Présentation du fichier .env et de son objet
* Génération du fichier .env
* Comment ajouter le fichier pour ajouter de nouveaux secrets
* Évitez de soumettre ce fichier, car il contient des informations sensibles.

>[!VIDEO](https://video.tv.adobe.com/v/3416593)

## Exemple de code

```bash
# Specify your secrets here
# The .env file must not be committed to source control
## Adobe I/O Runtime credentials
AIO_runtime_auth=abcd1234-aaa-bbb-ccc-12345:Abcdd12345asdfadsfadsfee2323232323232
AIO_runtime_namespace=12345-someworkspace-stage
AIO_runtime_apihost=https://adobeioruntime.net
## Adobe I/O Console service account credentials (JWT) Api Key
SERVICE_API_KEY=

# You can include some commerce OAUTH credentials too, our sample module will use this
#COMMERCE_BASE_URL=https://somecommercewebsite.com/
#COMMERCE_CONSUMER_KEY=abcebdme5bvafnemk0mdeeiyfq123
#COMMERCE_CONSUMER_SECRET=ffff86sqws3pss5hhuofiqgq4t04rrr11
#COMMERCE_ACCESS_TOKEN=gdddfccronj098r4m04zyq773s5o64
#COMMERCE_ACCESS_TOKEN_SECRET=ggg7nb19jhr5gi9jzfan9ggzipe8yrus
```

Vous pouvez voir ces valeurs statiques utilisées dans l’exemple de module dans le fichier . `actions/commerce.index.js`.

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
