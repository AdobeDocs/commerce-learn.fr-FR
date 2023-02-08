---
title: Création d’une requête source unique GraphQL à utiliser dans le maillage API
description: Découvrez comment utiliser le maillage API sur Adobe Commerce et [!DNL Adobe App Builder]. Découvrez comment créer une requête qui possède une source.
landing-page-description: Découvrez comment utiliser le maillage API sur Adobe Commerce et [!DNL Adobe App Builder]. Découvrez comment créer une requête qui possède une source.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Création d’un maillage d’API GraphQL source unique

La vidéo aide un développeur à comprendre comment créer un proxy inverse GraphQL et disposer d’une source unique. N’oubliez pas que pour que GraphQL Mesh fonctionne comme prévu, une URL accessible publiquement avec un schéma GraphQL valide est requise. La vidéo explique également comment configurer le fichier json initial à utiliser avec votre site web commercial. Pour consulter des exemples de code de base utilisés dans la vidéo, rendez-vous sur la page [Création d’un maillage](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## Pour qui est cette vidéo ?

* Toute personne qui découvre le maillage de l’API
* Développeurs intéressés par l’utilisation de plusieurs sources graphql
* Quiconque doit savoir comment filtrer l’onglet réseau et filtrer par graphql

## Contenu vidéo

* Utilisation de l’API comme proxy inverse
* Configuration JSON avec l’interface de ligne de commande Adobe Developer
* Accès au point de terminaison GraphQL nouvellement créé

>[!VIDEO](https://video.tv.adobe.com/v/3414124)

## Création du fichier de configuration json

Pour qu’Adobe App Builder sache quelles sont vos sources, vous pouvez les définir dans une configuration JSON. Chaque source est un élément d’un tableau et vous pouvez en avoir un ou plusieurs. Voici un exemple d’une source unique

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
