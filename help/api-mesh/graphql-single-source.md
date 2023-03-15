---
title: Création d’un maillage source unique GraphQL dans le maillage API
description: Découvrez comment utiliser le maillage API sur Adobe Commerce et [!DNL Adobe App Builder]. Découvrez comment créer un maillage ayant une source unique.
landing-page-description: Découvrez comment utiliser le maillage API sur Adobe Commerce et [!DNL Adobe App Builder]. Découvrez comment créer un maillage ayant une source unique.
short-description: Discover how to use API Mesh on Adobe Commerce and [!DNL Adobe App Builder]. Learn about creating a mesh that has one source.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: 67d21ca23cdccc87cdeed4a08a3ebb48e5bd1030
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Création d’un maillage à partir d’une source unique

Cette vidéo aide les développeurs à comprendre comment créer un maillage avec une seule source dans le maillage API pour Adobe Developer App Builder. Pour que cet exemple de base fonctionne comme prévu, vous avez besoin d’une API accessible publiquement ou d’un point de terminaison GraphQL. La vidéo explique également comment créer une `mesh.json` à utiliser avec votre instance Commerce. Pour plus d’informations et d’exemples de code, consultez la page [Création d’un maillage](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## Pour qui est cette vidéo ?

* Toute nouvelle personne qui découvre le maillage API
* Développeurs intéressés par la combinaison de plusieurs sources GraphQL et API
* Quiconque doit savoir comment filtrer l’onglet réseau et filtrer par GraphQL

## Contenu vidéo

* Utilisation du maillage d’API comme proxy inverse
* Création d’un maillage à partir d’un fichier de configuration JSON
* Accès au point de terminaison GraphQL nouvellement créé

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## Création du fichier de configuration json

Le maillage d’API utilise un fichier de configuration JSON pour définir vos gestionnaires de sources. Le fichier JSON contient une `sources` qui contient les sources de votre impression. Voici un exemple d’impression à partir d’une seule source.

```json
{
"meshConfig": {
    "sources": [
      {
        "name": "Commerce",
        "handler": {
          "graphql": {
            "endpoint": "https://venia.magento.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
