---
title: Création d’un maillage source unique GraphQL dans le maillage API
description: Découvrez comment utiliser le maillage API sur Adobe Commerce et  [!DNL Adobe App Builder]. Découvrez comment créer un maillage ayant une seule source.
landing-page-description: Découvrez comment utiliser le maillage API sur Adobe Commerce et  [!DNL Adobe App Builder]. Découvrez comment créer un maillage ayant une seule source.
short-description: Découvrez comment utiliser le maillage API sur Adobe Commerce et  [!DNL Adobe App Builder]. Découvrez comment créer un maillage ayant une seule source.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 0%

---

# Création d’un maillage avec une seule source

Cette vidéo aide les développeurs à comprendre comment créer un maillage avec une seule source dans le maillage API pour Adobe Developer App Builder. Pour que cet exemple de base fonctionne comme prévu, vous avez besoin d’une API ou d’un point d’entrée GraphQL accessible au public. La vidéo explique également comment créer un fichier `mesh.json` simple à utiliser avec votre instance Commerce. Pour plus d’informations et d’exemples de code, consultez [Création d’un maillage](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## À qui s&#39;adresse cette vidéo ?

* Toute personne qui découvre le maillage API
* Les développeurs et développeuses intéressés par la combinaison de plusieurs sources GraphQL et API
* Toute personne ayant besoin de savoir comment filtrer l’onglet réseau et filtrer par GraphQL

## Contenu vidéo

* Utilisation du maillage API comme proxy inverse
* Créer un maillage à partir d’un fichier de configuration JSON
* Accès au point d’entrée GraphQL nouvellement créé

>[!VIDEO](https://video.tv.adobe.com/v/3414124?quality=12&learn=on)

## Créer le fichier de configuration json

Le maillage API utilise un fichier de configuration JSON pour définir vos gestionnaires sources. Le fichier JSON contient un tableau `sources` qui contient les sources de votre maillage. Voici un exemple de maillage avec une seule source.

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
