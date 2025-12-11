---
title: Exécuter une requête à l’aide de GraphQL
description: Découvrez comment effectuer une requête à l’aide de GraphQL sur Adobe Commerce et  [!DNL Magento Open Source]. Cette section présente GraphQL à l’aide d’appels GET et POST.
landing-page-description: Découvrez comment effectuer une requête à l’aide de GraphQL sur Adobe Commerce et  [!DNL Magento Open Source]. Cette section présente GraphQL à l’aide d’appels GET et POST.
short-description: Découvrez comment effectuer une requête à l’aide de GraphQL sur Adobe Commerce et  [!DNL Magento Open Source]. Cette section présente GraphQL à l’aide d’appels GET et POST.
kt: 13937
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 443d711d-ec74-4e07-9357-fbbe0f774853
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '984'
ht-degree: 0%

---

# Requêtes GraphQL

Il s’agit de la partie 2 de la série pour GraphQL et Adobe Commerce. Dans ce tutoriel et cette vidéo, découvrez les requêtes GraphQL et comment les exécuter par rapport à Adobe Commerce.

>[!VIDEO](https://video.tv.adobe.com/v/3450060?captions=fre_fr&learn=on)

## Vidéos et tutoriels connexes sur GraphQL dans cette série

* [Partie 1 - GraphQL - Introduction](../graphql-rest/intro-graphql.md)
* [Partie 3 - GraphQL - Mutations](../graphql-rest/graphql-mutations.md)
* [Partie 4 - GraphQL - Schéma](../graphql-rest/graphql-schema.md)

## Exemple de syntaxe GraphQL

Plongeons directement dans la syntaxe de requête de GraphQL avec un exemple complet. (Souvenez-vous que vous pouvez essayer vous-même contre https://venia.magento.com/graphql.)

Observez la requête GraphQL suivante, qui est répartie morceau par morceau :

```graphql
{
    country (
        id: "US"
    ) {
        id
        full_name_english
    }

    categories(
        filters: {
            name: {
                match: "Tops"
            }
        }
    ) {
        items {
            name
            products(
                pageSize: 10,
                currentPage: 2
            ) {
                items {
                    sku
                }
            }
        }
    }
}
```

Une réponse plausible d’un serveur GraphQL pour la requête ci-dessus pourrait être :

```json
{
  "data": {
    "country": {
      "id": "US",
      "full_name_english": "United States"
    },
    "categories": {
      "items": [
        {
          "name": "Tops",
          "products": {
            "items": [
              {
                "sku": "VSW06"
              },
              {
                "sku": "VT06"
              },
              {
                "sku": "VSW07"
              },
              {
                "sku": "VT07"
              },
              {
                "sku": "VSW08"
              },
              {
                "sku": "VT08"
              },
              {
                "sku": "VSW09"
              },
              {
                "sku": "VT09"
              },
              {
                "sku": "VSW10"
              },
              {
                "sku": "VT10"
              }
            ]
          }
        }
      ]
    }
  }
}
```

L’exemple ci-dessus repose sur le schéma GraphQL prêt à l’emploi pour Adobe Commerce, défini sur le serveur. Dans cette requête, vous
interroger plusieurs types de données à la fois ; La requête exprime exactement les champs souhaités et les données renvoyées sont formatées
de la même manière que pour la requête elle-même.

>[!NOTE]
>
>Les clients GraphQL obscurcissent la forme de la requête HTTP réelle envoyée, mais cela est facile à découvrir. Si vous utilisez un client basé sur un navigateur, observez l’onglet [!UICONTROL Network] lorsqu’une requête est envoyée. Vous constatez que la requête contient un corps brut composé de « requête : `{string}` », où `{string}` est simplement la chaîne brute de l’ensemble de votre requête. Si la requête est envoyée en tant que GET, elle peut être codée dans le paramètre de chaîne de requête « query » à la place. Contrairement à REST, le type de requête HTTP n’a pas d’importance, seul le contenu de la requête importe.


## Effectuer une requête pour obtenir ce que vous souhaitez

`country` et `categories` dans l’exemple représentent deux « requêtes » différentes pour deux types de données différents. Contrairement à un paradigme d’API traditionnel comme REST, qui définirait des points d’entrée distincts et explicites pour chaque type de données. GraphQL vous offre la possibilité d’interroger un seul point d’entrée avec une expression qui peut récupérer plusieurs types de données à la fois.

De même, la requête spécifie exactement les champs souhaités à la fois pour `country` (`id` et `full_name_english`) et `categories` (`items`, qui comporte elle-même une sous-sélection de champs), et les données que vous recevez reflètent cette spécification de champ. Il y a probablement beaucoup plus de champs disponibles pour ces types de données, mais vous ne recevez que ce que vous avez demandé.


>[!NOTE]
>
>Vous remarquerez peut-être que la valeur renvoyée pour `items` est en fait un _tableau_ valeurs , mais vous sélectionnez néanmoins directement des sous-champs pour celui-ci. Lorsque le type d’un champ est une liste, GraphQL comprend implicitement que des sous-sélections doivent s’appliquer à chaque élément de la liste.

## Arguments

Bien que les champs que vous souhaitez renvoyer soient spécifiés entre accolades de chaque type, les arguments nommés et leurs valeurs sont spécifiés entre parenthèses après le nom du type. Les arguments sont souvent facultatifs et affectent souvent la manière dont les résultats de la requête sont filtrés, formatés ou transformés.

Vous transmettez un argument `id` à `country`, spécifiant le pays particulier à interroger, ainsi qu’un argument `filters` pour `categories`.

## Champs jusqu’au bas

Bien que vous ayez tendance à considérer `country` et `categories` comme des requêtes ou des entités distinctes, l’arborescence entière exprimée dans votre requête ne se compose en fait que de champs. L&#39;expression de `products` n&#39;est pas syntaxiquement différente de celle de `categories`. Les deux sont des champs, et il n&#39;y a aucune différence entre leur construction.

Tout graphique de données GraphQL comporte un seul type « racine » (généralement appelé `Query`) pour démarrer l’arborescence. Les types souvent considérés comme des entités sont affectés aux champs de cette racine. L’exemple de requête crée en fait une requête générique pour le type racine et sélectionne les champs `country` et `categories`. Il sélectionne ensuite des sous-champs de ces champs, etc., potentiellement à plusieurs niveaux de profondeur. Lorsque le type de valeur renvoyée d’un champ est un type complexe (par exemple, un champ doté de ses propres champs, plutôt qu’un type scalaire), continuez à sélectionner les champs de votre choix.

Ce concept de champs imbriqués explique également pourquoi vous pouvez transmettre des arguments pour `products` (`pageSize` et `currentPage`) de la même manière que pour le champ de `categories` de niveau supérieur.

Arborescence de champs de GraphQL ![](../assets/graphql-field-tree.png)

## Variables

Essayons une autre requête :

```graphql
query getProducts(
    $search: String
) {
    products(
        search: $search
    ) {
        items {
            ...productDetails
            related_products {
                ...productDetails
            }
        }
    }
}

fragment productDetails on ProductInterface {
    sku
    name
}
```

La première chose à noter est le mot-clé `query` ajouté avant l’accolade d’ouverture de la requête, avec un nom d’opération (`getProducts`). Le nom de cette opération est arbitraire ; il ne correspond à rien dans le schéma du serveur. Cette syntaxe a été ajoutée pour prendre en charge l’introduction de variables.

Dans la requête précédente, vous avez codé en dur des valeurs pour les arguments de vos champs directement, sous forme de chaînes ou d’entiers. La spécification GraphQL, cependant, prend en charge de première classe pour séparer l’entrée utilisateur de la requête principale à l’aide de variables.

Dans la nouvelle requête, vous utilisez des parenthèses avant l’accolade ouvrante de l’ensemble de la requête pour définir une variable `$search` (les variables utilisent toujours la syntaxe du préfixe du signe dollar). C’est cette variable qui est fournie à l’argument `search` pour `products`.

Lorsqu’une requête contient des variables, la requête GraphQL doit inclure un dictionnaire de valeurs codé JSON distinct avec la requête elle-même. Pour la requête ci-dessus, vous pouvez envoyer le fichier JSON suivant de valeurs de variable en plus du corps de la requête :

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>Si vous testez ces requêtes sur l’exemple de site Venia plutôt que sur votre propre instance Adobe Commerce, les résultats renvoyés seront probablement vides pour `related_products`.

Dans tout client compatible avec GraphQL que vous utilisez pour les tests (comme Altair et GraphiQL), l’interface utilisateur prend en charge la saisie des variables JSON séparément de la requête.

Tout comme vous avez vu que la requête HTTP réelle pour une requête GraphQL contient « query: `{string}` » dans son corps, toute requête contenant un dictionnaire de variables inclut simplement un « variables: `{json}` » supplémentaire dans ce même corps, où `{json}` est la chaîne JSON avec les valeurs de variable.

La nouvelle requête utilise également un _fragment_ (`productDetails`) pour réutiliser la même sélection de champ à plusieurs endroits. [En savoir plus sur les fragments](https://graphql.org/learn/queries/#fragments){target="_blank"} dans la documentation de GraphQL.

{{$include /help/_includes/graphql-rest-related-links.md}}
