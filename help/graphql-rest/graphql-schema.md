---
title: Langage de schéma avec GraphQL
description: Découvrez le schéma associé à GraphQL. Lisez une description du schéma, ainsi que quelques modèles et méthodes intéressants pour lire le schéma.
landing-page-description: Ceci est une introduction à GraphQL. Comprendre le schéma et comment interpréter certains des éléments
short-description: Ceci est une introduction à GraphQL. Comprendre le schéma et comment interpréter certains des éléments
kt: 13939
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
old-role: Architect, Developer
role: Developer
level: Beginner, Intermediate
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '430'
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

La déclaration `input` est à tous égards semblable à un `type`, mais elle définit un type qui peut être utilisé comme entrée pour un argument. Notez également la déclaration `interface`. Cela sert une fonction plus ou moins identique aux interfaces en PHP. D’autres types héritent de cette interface.

La syntaxe `[CartItemInput!]!` semble délicate, mais est assez intuitive à la fin. Le `!` _within_ le crochet déclare que chaque valeur du tableau doit être non nulle, tandis que celui _external_ déclare que la valeur du tableau elle-même doit être non nulle (par exemple, un tableau vide).

>[!NOTE]
>
>La logique de récupération et de formatage des données en fonction d’un schéma, ainsi que la manière dont une telle logique est mappée à des types particuliers, dépendent de l’implémentation du runtime GraphQL. Toutefois, les implémentations doivent suivre un flux conceptuel logique à la lumière d’une compréhension des champs imbriqués : une opération de résolution associée au `Query` racine ou au type de `Mutation` est effectuée, qui examine chaque champ spécifié dans la requête. Pour chaque champ qui correspond à un type complexe, une résolution similaire est effectuée pour ce type, et ainsi de suite, jusqu’à ce que tout soit résolu en valeurs scalaires.

{{$include /help/_includes/graphql-rest-related-links.md}}
