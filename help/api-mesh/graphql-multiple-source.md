---
title: Création d’un GraphQL source multiple à utiliser dans le maillage API
description: Découvrez comment utiliser plusieurs sources pour le maillage API sur Adobe Commerce et [!DNL Adobe App Builder]. Découvrez les erreurs courantes et comment les résoudre.
landing-page-description: Découvrez comment utiliser le maillage API sur Adobe Commerce et [!DNL Adobe App Builder]. Découvrez comment créer un message comportant plusieurs sources et comment résoudre certaines erreurs courantes.
short-description: Découvrez comment utiliser le maillage API sur Adobe Commerce et [!DNL Adobe App Builder]. Découvrez comment créer un message comportant plusieurs sources et comment résoudre certaines erreurs courantes.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: d788a068-9d20-4db0-a0eb-fd897873253d
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# Création d’un maillage avec plusieurs sources

Cette vidéo aide les développeurs à comprendre comment créer un maillage avec plusieurs sources dans le maillage API pour Adobe Developer App Builder. Cette vidéo montre comment créer un maillage avec plusieurs sources et identifier les erreurs. Pour plus d’informations et d’exemples de code, consultez la page [Création d’un maillage](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1){target="_blank"}.

## Pour qui est cette vidéo ?

* Toute personne qui découvre le maillage de l’API
* Développeurs intéressés par la combinaison de plusieurs sources API et GraphQL

## Contenu vidéo

* Utilisation [transformations](https://developer.adobe.com/graphql-mesh-gateway/gateway/transforms/){target="_blank"} modification du schéma source par défaut
* Comment résoudre les erreurs, telles que les conflits de noms, la disponibilité des schémas et d’autres problèmes de syntaxe de schéma
* Mise à jour de votre impression avec une configuration modifiée

>[!VIDEO](https://video.tv.adobe.com/v/3414125?quality=12&learn=on)

## Création du fichier de configuration json

Le maillage d’API utilise un fichier de configuration JSON pour définir vos gestionnaires de sources. Le fichier JSON contient une `sources` qui contient les sources de votre impression. Voici un exemple d’impression avec plusieurs sources.

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
