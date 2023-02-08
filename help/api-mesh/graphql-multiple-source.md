---
title: Création d’un GraphQL source multiple à utiliser dans le maillage API
description: Découvrez comment utiliser plusieurs sources pour le maillage API sur Adobe Commerce et [!DNL Adobe App Builder]. Découvrez les erreurs courantes et comment les résoudre.
landing-page-description: Découvrez comment utiliser le maillage API sur Adobe Commerce et [!DNL Adobe App Builder]. Découvrez comment créer une requête qui comporte plusieurs sources et comment résoudre certaines erreurs courantes.
kt: 11804
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Création de plusieurs mailles d’API GraphQL source

La vidéo aide un développeur à comprendre comment créer un proxy inverse GraphQL avec plusieurs sources. Cette vidéo montre comment assembler différentes sources, identifier les erreurs et enregistrer les modifications apportées à git. Pour consulter des exemples de code de base utilisés dans la vidéo, rendez-vous sur la page [Création d’un maillage](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/#create-a-mesh-1).

## Pour qui est cette vidéo ?

* Toute personne qui découvre le maillage de l’API
* Développeurs intéressés par l’utilisation de plusieurs sources graphql
* Quiconque doit savoir comment filtrer l’onglet réseau et filtrer par graphql

## Contenu vidéo

* Comment le schéma d’API Attributs personnalisés complexes d’une seconde source peut remplacer le schéma source par défaut
* Modification de la configuration de l’impression d’api afin de prendre en compte le deuxième schéma de remplacement
* Comment résoudre les erreurs qui peuvent se produire dans le processus, comme les conflits de noms, la disponibilité des schémas et d’autres syntaxes SDL
* Exemple d&#39;erreurs courantes après les tentatives de groupement de schémas
* Reconstruire l’impression après modification
* Enregistrement des modifications dans Git après modification de la configuration du maillage de l’API

>[!VIDEO](https://video.tv.adobe.com/v/3414125)

## Création du fichier de configuration json

Pour qu’Adobe App Builder sache quelles sont vos sources, vous pouvez les définir dans une configuration JSON. Chaque source est un élément d’un tableau et vous pouvez en avoir un ou plusieurs. Voici un exemple de requête source multiple qui sont regroupées pour former une seule réponse.

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
        "name": "ERP",
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
