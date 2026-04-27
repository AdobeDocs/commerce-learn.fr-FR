---
title: Langage de schéma avec GraphQL
description: Découvrez le schéma associé à GraphQL. Lisez une description du schéma, ainsi que quelques modèles et méthodes intéressants pour lire le schéma.
landing-page-description: Ceci est une introduction à GraphQL. Comprendre le schéma et comment interpréter certains des éléments
short-description: Ceci est une introduction à GraphQL. Comprendre le schéma et comment interpréter certains des éléments
kt: 13939
doc-type: video
duration: 363
audience: all
last-substantial-update: 2023-10-12T00:00:00.000Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
TQID: https://experienceleague.adobe.com/P9gopqc6BD8qZJlc1-gvVem4Gq3fqpX8AsRvglLAQYI
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 436
ht-degree: 0%

---

# Langage du schéma

Il s’agit de la partie 4 de la série pour GraphQL et Adobe Commerce. Les requêtes et les mutations utilisées reposent sur un graphique de données spécifique implémenté sur le serveur, que l’exécution de GraphQL consomme et utilise pour résoudre la requête. La spécification GraphQL définit un langage agnostique pour exprimer les types et les relations de votre graphique de données.

>[!VIDEO](https://video.tv.adobe.com/v/3424123?learn=on)

## Vidéos et tutoriels connexes sur GraphQL dans cette série

* [Partie 1 - GraphQL - Introduction](../graphql-rest/intro-graphql.md)
* [Partie 2 - GraphQL - Requêtes](../graphql-rest/graphql-queries.md)
* [Partie 3 - GraphQL - Mutations](../graphql-rest/graphql-mutations.md)

## Exemple de schéma

Voici un schéma de type abrégé qui prend en charge les requêtes et les mutations que vous avez examinées jusqu’à présent :

```graphql
input FilterMatchTypeInput {
  match: String
}

type Money {
  value: Float
}

type Country {
  id: String
  full_name_english: String
}

interface ProductInterface {
  sku: String
  name: String
  related_products: [ProductInterface]
}

type CategoryFilterInput {
  name: FilterMatchTypeInput
}

type CategoryProducts {
  items: [ProductInterface]
}

type CategoryTree {
  name: String
  products(pageSize: Int, currentPage: Int): CategoryProducts
}

type CategoryResult {
  items: [CategoryTree]
}

type Products {
  items: [ProductInterface]
}

type Query {
  country (id: String): Country
  categories (filters: CategoryFilterInput): CategoryResult
  products (search: String): Products
}

input CartItemInput {
  sku: String!
  quantity: Float!
}

type CartPrices {
  grand_total: Money
}

type Cart {
  prices: CartPrices
  total_quantity: Float!
}

type AddProductsToCartOutput {
  cart: Cart!
}

type Mutation {
  addProductsToCart(cartId: String!, cartItems: [CartItemInput!]!): AddProductsToCartOutput
}
```

Plongez-vous dans [la documentation de GraphQL](https://graphql.org/learn/schema/){target="_blank"} pour en savoir plus sur les détails du système de type, y compris la syntaxe de certains concepts qui ne sont pas représentés ici. L’exemple ci-dessus s’explique par lui-même. (Notez également à quel point la syntaxe est similaire à la syntaxe de requête.) La définition d’un schéma GraphQL consiste simplement à exprimer les arguments et champs disponibles d’un type donné, ainsi que les types de ces champs. Chaque type de champ complexe doit lui-même avoir une définition, etc., dans l’arborescence, jusqu’à ce que vous obteniez des types scalaires simples comme `String`.

La déclaration `input` est à tous égards semblable à un `type`, mais elle définit un type qui peut être utilisé comme entrée pour un argument. Notez également la déclaration `interface`. This serves a function more or less the same as interfaces in PHP. Other types inherit from this interface.

The syntax `[CartItemInput!]!` looks tricky but is fairly intuitive in the end. The `!` _inside_ the bracket declares that every value in the array must be non-null, while the one _outside_ declares that the array value itself must be non-null (for example, an empty array).

>[!NOTE]
>
>The logic for how data is fetched and formatted according to a schema, and how such logic is mapped to particular types, is up to the GraphQL runtime implementation. Implementations, however, should follow a conceptual flow that makes sense in light of an understanding around nested fields: A resolve operation associated with the root `Query` or `Mutation` type is performed, which examines each field specified in the request. For each field that resolves to a complex type, a similar resolve is done for that type, and so on, until everything has resolved into scalar values.

{{$include /help/_includes/graphql-rest-related-links.md}}
