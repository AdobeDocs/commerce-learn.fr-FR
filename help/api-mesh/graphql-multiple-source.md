---
title: Créer un GraphQL source multiple à utiliser dans le maillage API
description: Découvrez comment utiliser plusieurs sources pour le maillage API sur Adobe Commerce et  [!DNL Adobe App Builder]. Découvrez certaines erreurs courantes et comment les résoudre.
landing-page-description: Découvrez comment utiliser le maillage API sur Adobe Commerce et  [!DNL Adobe App Builder]. Découvrez comment créer un maillage ayant plusieurs sources et comment résoudre certaines erreurs courantes.
short-description: Découvrez comment utiliser le maillage API sur Adobe Commerce et  [!DNL Adobe App Builder]. Découvrez comment créer un maillage ayant plusieurs sources et comment résoudre certaines erreurs courantes.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# Création d’un maillage avec plusieurs sources

Cette vidéo aide les développeurs à comprendre comment créer un maillage avec plusieurs sources dans le maillage API pour Adobe Developer App Builder. Cette vidéo montre comment créer un maillage avec plusieurs sources et identifier les erreurs. Pour plus d’informations et d’exemples de code, consultez [Création d’un maillage](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## À qui s&#39;adresse cette vidéo ?

* Toute personne qui découvre le maillage API
* Les développeurs et développeuses intéressés par la combinaison de plusieurs sources API et GraphQL

## Contenu vidéo

* Utilisation des [transformations](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"} pour modifier le schéma source par défaut
* Comment résoudre les erreurs telles que les conflits de nom, la disponibilité des schémas et d’autres problèmes de syntaxe de schéma
* Mise à jour de votre maillage avec une configuration modifiée

>[!VIDEO](https://video.tv.adobe.com/v/3414125?quality=12&learn=on)

## Créer le fichier de configuration json

Le maillage API utilise un fichier de configuration JSON pour définir vos gestionnaires sources. Le fichier JSON contient un tableau `sources` qui contient les sources de votre maillage. Voici un exemple de maillage avec plusieurs sources.

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
      },
      {
        "name": "Example",
        "handler": {
          "graphql": {
            "endpoint": "https://www.example.com/graphql/"
          }
        }
      }
    ]
  }
}
```

{{$include /help/_includes/api-mesh-related-links.md}}
