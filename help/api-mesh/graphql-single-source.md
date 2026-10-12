---
title: Création d’un maillage source unique GraphQL dans le maillage API
description: Découvrez comment utiliser le maillage API sur Adobe Commerce et Adobe App Builder. Découvrez comment créer un maillage avec une source GraphQL unique et accéder au nouveau point d’entrée.
jira: KT-11804
doc-type: Tutorial
duration: 485
last-substantial-update: 2023-02-08T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 9a78457a-1539-49c0-ac69-4bbfc6786137
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: ce84ce08-883f-4337-ae83-6bb1855ca732
    internal-label: API Mesh
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 0%
---
# Création d’un maillage avec une seule source

Cette vidéo aide les développeurs à comprendre comment créer un maillage avec une seule source dans le maillage API pour Adobe Developer App Builder. Pour que cet exemple de base fonctionne, vous avez besoin d’une API ou d’un point d’entrée GraphQL accessible au public. La vidéo explique également comment créer un fichier `mesh.json` simple à utiliser avec votre instance Commerce. Pour plus d’informations et d’exemples de code, consultez [Création d’un maillage](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/create-mesh){target="_blank"}.

## À qui s&#39;adresse cette vidéo ?

* Toute personne qui découvre le maillage API
* Les développeurs et développeuses intéressés par la combinaison de plusieurs sources GraphQL et API
* Toute personne ayant besoin de savoir comment filtrer l’onglet réseau et filtrer par GraphQL

## Contenu vidéo

* Utilisation du maillage API comme proxy inverse
* Créer un maillage à partir d’un fichier de configuration JSON
* Accès au point d’entrée GraphQL nouvellement créé

>[!VIDEO](https://video.tv.adobe.com/v/3414124?learn=on)

## Créer le fichier de configuration JSON

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
