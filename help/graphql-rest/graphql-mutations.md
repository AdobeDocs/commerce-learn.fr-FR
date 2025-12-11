---
title: Réalisation d’une mutation à l’aide de GraphQL
description: Découvrez comment effectuer une mutation à l’aide de GraphQL sur Adobe Commerce et  [!DNL Magento Open Source]. Effectuez votre première mutation à l’aide d’appels POST.
landing-page-description: Découvrez comment effectuer une mutation à l’aide de GraphQL sur Adobe Commerce et  [!DNL Magento Open Source]. Effectuez votre première mutation à l’aide d’appels POST.
short-description: Découvrez comment effectuer une mutation à l’aide de GraphQL sur Adobe Commerce et  [!DNL Magento Open Source]. Effectuez votre première mutation à l’aide d’appels POST.
kt: 13938
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# Mutations

Il s’agit de la partie 3 de la série pour GraphQL et Adobe Commerce. Les mutations sont la possibilité d’enregistrer, de mettre à jour et de renvoyer des valeurs à l’aide de GraphQL.


>[!VIDEO](https://video.tv.adobe.com/v/3441925?captions=fre_fr&learn=on)

## Vidéos et tutoriels connexes sur GraphQL dans cette série

* [Partie 1 - GraphQL - Introduction](../graphql-rest/intro-graphql.md)
* [Partie 2 - GraphQL - Requêtes](../graphql-rest/graphql-queries.md)
* [Partie 4 - GraphQL - Schéma](../graphql-rest/graphql-schema.md)

## Exemple de mutation

Toute spécification d’API complète doit offrir la possibilité non seulement d’interroger les données, mais également de les créer et de les mettre à jour.

REST fait la distinction entre les requêtes qui modifient des données et celles qui ne le font pas avec le type de requête ou le « verbe » (GET ou POST ou PUT).
Lors de l’utilisation de GraphQL, les requêtes de modification des données sont distinguées par le mot-clé `mutation` qui correspond à un autre
Type racine dans le schéma défini sur le serveur.

Examinez cet exemple de mutation pour ajouter un produit au panier d’un utilisateur. (Nécessite un ID de panier qui a été généré
pour la session du client connecté ou à l’aide de la mutation `createEmptyCart`.)

```graphql
mutation doAddToCart(
    $cartId: String!,
    $cartItems: [CartItemInput!]!
) {
    addProductsToCart(
        cartId: $cartId
        cartItems: $cartItems
    ) {
        cart {
            total_quantity
            prices {
                grand_total {
                    value
                }
            }
        }
    }
}
```

Vous pouvez imaginer la mutation ci-dessus envoyée dans une requête avec le dictionnaire de variables suivant :

```json
{
  "cartId": "{cart-id}",
  "cartItems": [
    {
      "quantity": 1,
      "sku": "VT01-RN-XS"
    }
  ]
}
```

Enfin, vous pouvez recevoir une réponse de ce type :

```json
{
  "data": {
    "addProductsToCart": {
      "cart": {
        "total_quantity": 1,
        "prices": {
          "grand_total": {
            "value": 35.2
          }
        }
      }
    }
  }
}
```

L’élément principal à noter à propos de l’exemple ci-dessus est que, outre l’utilisation du mot-clé `mutation` au lieu de `query`,
la syntaxe est identique à celle d’une requête. Comme les requêtes, la mutation inclut :

* Nom d’opération arbitraire (`doAddToCart`)
* Une liste de variables (par exemple, `$cartId`)
* Champ initial (`addProductsToCart`) avec des arguments (par exemple, `cartId`, défini sur la valeur de `$cartId`) entre parenthèses
* Sous-sélection de champs entre accolades

La sous-sélection de champs vous permet de définir de manière flexible les champs que vous souhaitez renvoyer (à partir du type affecté comme
valeur renvoyée de `addProductsToCart` - `AddProductsToCartOutput`) une fois la mutation terminée.

Comme expliqué précédemment, les champs définis dans un schéma GraphQL commencent sur un type racine pour les requêtes (généralement appelé `Query`). De même,
il existe un autre type de racine pour les mutations (généralement appelées `Mutation`). `addProductsToCart` est un champ
sur ce type racine.

Quelques autres remarques sur l’exemple ci-dessus :

* Le suffixe de caractère `!` `String` et `CartItemInput` indique que la variable est requise.
* Les crochets (`[]`) autour du type de `CartItemInput` spécifié pour `$cartItems` une liste
de ce type plutôt qu’une valeur unique.

{{$include /help/_includes/graphql-rest-related-links.md}}
