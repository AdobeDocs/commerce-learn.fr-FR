---
title: Exécution d’une requête à l’aide de GraphQL
description: Découvrez comment effectuer une requête à l’aide de GraphQL sur Adobe Commerce et [!DNL Magento Open Source]. Cette section présente GraphQL à l’aide d’appels GET et POST.
landing-page-description: Découvrez comment effectuer une requête à l’aide de GraphQL sur Adobe Commerce et [!DNL Magento Open Source]. Cette section présente GraphQL à l’aide d’appels GET et POST.
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
source-git-commit: 9dc530107470617f88992d8eb2ed9feb017a6530
workflow-type: tm+mt
source-wordcount: '916'
ht-degree: 0%

---

# Requêtes GraphQL

Examinons la syntaxe des requêtes GraphQL avec un exemple complet. (N’oubliez pas que vous pouvez essayer par vous-même contre https://venia.magento.com/graphql.)

Observez la requête GraphQL suivante, qui est ventilée pièce par pièce :

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

Une réponse plausible d’un serveur GraphQL pour la requête ci-dessus peut être :

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

L’exemple ci-dessus repose sur le schéma GraphQL prêt à l’emploi pour Magento, défini sur le serveur. Dans cette requête, vous interrogez plusieurs types de données à la fois. La requête exprime exactement les champs que vous souhaitez et les données renvoyées sont formatées de la même manière que la requête elle-même.

>[!NOTE]
>
>Les clients GraphQL obscurcissent le formulaire de la requête HTTP réelle envoyée, mais cela est facile à découvrir. Si vous utilisez un client basé sur un navigateur, observez la variable [!UICONTROL Network] lorsqu’une requête est envoyée. Vous voyez que la requête contient un corps brut constitué de &quot;requête : `{string}`&quot;, où `{string}` est simplement la chaîne brute de votre requête entière. Si la demande est envoyée en tant que GET, la requête peut être codée dans le paramètre de chaîne de requête &quot;query&quot; à la place. Contrairement à REST, le type de requête HTTP n’a pas d’importance, mais seulement le contenu de la requête.


## Requête pour ce que vous souhaitez

`country` et `categories` dans l’exemple, représentent deux &quot;requêtes&quot; différentes, pour deux types de données différents. Contrairement à un paradigme d’API traditionnel tel que REST, qui définissait des points de terminaison distincts et explicites pour chaque type de données, GraphQL vous offre la possibilité d’interroger un point de terminaison unique avec une expression qui peut récupérer simultanément de nombreux types de données.

De même, la requête spécifie exactement les champs souhaités pour les deux `country` (`id` et `full_name_english`) et `categories` (`items`, qui comporte lui-même une sous-sélection de champs), et les données que vous recevez en retour reflètent cette spécification de champ. Il y a probablement beaucoup plus de champs disponibles pour ces types de données, mais vous ne récupérez que ce que vous avez demandé.


>[!NOTE]
>
>Vous pouvez remarquer que la valeur renvoyée pour `items` est en fait un _tableau_ de valeurs, mais vous sélectionnez néanmoins directement des sous-champs. Lorsque le type d’un champ est une liste, GraphQL comprend implicitement les sous-sélections à appliquer à chaque élément de la liste.

## Arguments

Bien que les champs que vous souhaitez renvoyer soient spécifiés entre les accolades de chaque type, les arguments et valeurs nommés sont spécifiés entre parenthèses après le nom du type. Les arguments sont souvent facultatifs et affectent souvent la manière dont les résultats des requêtes sont filtrés, formatés ou transformés.

Vous transmettez une `id` argument vers `country`, en spécifiant le pays particulier sur lequel nous voulons effectuer des requêtes, et un `filters` argument pour `categories`.

## Champs tout en bas

Tandis que vous avez tendance à penser à `country` et `categories` en tant que requêtes ou entités distinctes, l’arborescence entière exprimée dans votre requête ne se compose en fait que de champs. L’expression de `products` n’est pas différent syntaxiquement de `categories`. Ce sont des champs, et il n&#39;y a pas de différence entre leur construction.

Tout graphique de données GraphQL possède un seul type &quot;root&quot; (généralement référencé `Query`) pour démarrer l’arborescence, et les types souvent considérés comme des entités sont simplement affectés aux champs de cette racine. Notre exemple de requête crée en fait une requête générique pour le type racine et sélectionne les champs. `country` et `categories`. Il sélectionne ensuite les sous-champs de ces champs, etc., potentiellement à plusieurs niveaux de profondeur. Si le type de retour d’un champ est complexe (par exemple, un champ ayant ses propres champs plutôt qu’un type scalaire), continuez à sélectionner les champs de votre choix.

Ce concept de champs imbriqués est également la raison pour laquelle vous pouvez transmettre des arguments pour `products` (`pageSize` et `currentPage`) de la même manière que pour le niveau supérieur `categories` champ .

![Arborescence de champ GraphQL](../assets/graphql-field-tree.png)

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

La première chose à noter est que le mot-clé a été ajouté. `query` avant l’accolade d’ouverture de la requête, avec un nom d’opération (`getProducts`). Ce nom d’opération est arbitraire ; il ne correspond à rien dans le schéma du serveur. Cette syntaxe a été ajoutée pour prendre en charge l’introduction de variables.

Dans la requête précédente, vous avez codé en dur les valeurs des arguments de vos champs directement, sous la forme de chaînes ou d’entiers. Toutefois, la spécification GraphQL prend en charge la première classe pour séparer les entrées utilisateur de la requête principale à l’aide de variables.

Dans la nouvelle requête, vous utilisez des parenthèses avant l’accolade d’ouverture de l’ensemble de la requête pour définir une `$search` (les variables utilisent toujours la syntaxe du préfixe dollar) et c’est cette variable qui est fournie à la variable `search` argument pour `products`.

Lorsqu’une requête contient des variables, il est prévu que la requête GraphQL inclue un dictionnaire de valeurs codé JSON distinct en même temps que la requête elle-même. Pour la requête ci-dessus, vous pouvez envoyer le JSON suivant de valeurs de variable en plus du corps de la requête :

```json
{
    "search": "VT01"
}
```

>[!NOTE]
>
>Si vous testez ces requêtes sur le site d’exemple Venia plutôt que sur votre propre instance de Magento, vous n’obtiendrez probablement aucun résultat pour `related_products`.

Dans tout client compatible avec GraphQL que vous utilisez pour les tests (comme Altair et GraphiQL), l’interface utilisateur prend en charge la saisie des variables JSON séparément de la requête.

Comme vous l’avez vu, la requête HTTP réelle pour une requête GraphQL contient &quot;query: `{string}`&quot; dans son corps, toute requête contenant un dictionnaire de variables inclut simplement une variable &quot;supplémentaire : `{json}`&quot; dans le même corps, où `{json}` est la chaîne JSON avec les valeurs de variable.

La nouvelle requête utilise également un _fragment_ (`productDetails`) pour réutiliser la même sélection de champ à plusieurs endroits. [En savoir plus sur les fragments](https://graphql.org/learn/queries/#fragments) dans la documentation de GraphQL.
