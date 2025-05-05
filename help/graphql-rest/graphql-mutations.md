---
title: Effectuer une mutation à l’aide de GraphQL
description: Découvrez comment effectuer une mutation avec GraphQL sur Adobe Commerce et  [!DNL Magento Open Source]. Effectuez votre première mutation en utilisant des appels POST.
landing-page-description: Découvrez comment effectuer une mutation avec GraphQL sur Adobe Commerce et  [!DNL Magento Open Source]. Effectuez votre première mutation en utilisant des appels POST.
short-description: Découvrez comment effectuer une mutation avec GraphQL sur Adobe Commerce et  [!DNL Magento Open Source]. Effectuez votre première mutation en utilisant des appels POST.
kt: 13938
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 6b82ffda-925f-4a81-8ca5-49a2b8ab4929
source-git-commit: 2041bbf1a2783975091b9806c12fc3c34c34582f
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 0%

---

# Mutations

Il s’agit de la troisième partie de la série pour GraphQL et Adobe Commerce. Les mutations permettent d’enregistrer, de mettre à jour et de renvoyer des valeurs à l’aide de GraphQL.


>[!VIDEO](https://video.tv.adobe.com/v/3441925?learn=on&captions=fre_fr)

## Vidéos et tutoriels connexes sur GraphQL dans cette série

* [Partie 1 - GraphQL - Introduction](../graphql-rest/intro-graphql.md)
* [Partie 2 - GraphQL - Requêtes](../graphql-rest/graphql-queries.md)
* [Partie 4 GraphQL - Schéma](../graphql-rest/graphql-schema.md)

## Exemple de mutation

Toute spécification d’API complète doit offrir la possibilité non seulement d’interroger des données, mais également de les créer et de les mettre à jour.

REST fait la distinction entre les requêtes qui modifient les données et celles qui ne sont pas du type de requête ou &quot;verb&quot; (GET ou POST ou PUT).
Lors de l’utilisation de GraphQL, les requêtes de modification des données sont distinguées par le mot-clé `mutation` qui correspond à un
type racine dans le schéma défini sur le serveur.

Regardez cet exemple de mutation pour l’ajout d’un produit au panier d’un utilisateur. (Cela nécessite un identifiant de panier généré.
pour la session du client connecté ou en utilisant la mutation `createEmptyCart`.)

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

Et enfin, vous pourriez recevoir une réponse comme celle-ci :

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

La principale chose à noter à propos de l’exemple ci-dessus est que, en plus de l’utilisation du mot-clé `mutation` au lieu de `query`,
la syntaxe est identique à celle d’une requête. Comme les requêtes, la mutation inclut :

* Un nom d’opération arbitraire (`doAddToCart`)
* Une liste de variables (par exemple, `$cartId`)
* Un champ initial (`addProductsToCart`) avec des arguments (par exemple, `cartId`, défini sur la valeur de `$cartId`) entre parenthèses
* Sous-sélection de champs entre accolades

La sous-sélection de champs vous permet de définir de manière flexible les champs que vous souhaitez renvoyer (à partir du type affecté comme
valeur renvoyée de `addProductsToCart` - `AddProductsToCartOutput`) une fois la mutation terminée.

Comme expliqué précédemment, les champs définis dans un schéma GraphQL démarrent sur un type racine pour les requêtes (généralement appelé `Query`). De même,
il existe un autre type racine pour les mutations (généralement appelé `Mutation`). `addProductsToCart` est un champ
sur ce type racine.

Voici quelques autres remarques concernant l’exemple ci-dessus :

* Le suffixe `!` de caractères `String` et `CartItemInput` indique que la variable est requise.
* Les crochets (`[]`) autour du type `CartItemInput` spécifié pour `$cartItems` indiquent une liste.
de ce type plutôt qu’une seule valeur.

{{$include /help/_includes/graphql-rest-related-links.md}}
