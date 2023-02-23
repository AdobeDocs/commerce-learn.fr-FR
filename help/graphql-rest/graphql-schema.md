---
title: Langage de schéma avec GraphQL
description: Découvrez le schéma impliqué dans GraphQL. Lisez une description du schéma, ainsi que des schémas intéressants et des méthodes de lecture du schéma.
landing-page-description: Cette section présente GraphQL. Comprendre le schéma et comment interpréter certains éléments
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
exl-id: 6b59db07-b99e-47ae-9ccb-d4904afc8251
source-git-commit: 0fa7ba038f542172c47bea859f8712759fcc52f7
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---

# Langage de schéma

Les requêtes et les mutations utilisées reposent sur un graphique de données spécifique mis en oeuvre sur le serveur, que l’exécution GraphQL utilise pour résoudre la requête. La spécification GraphQL définit un langage agnostique pour exprimer les types et relations de votre graphique de données.

Voici un schéma de type abrégé qui prend en charge les requêtes et les mutations que vous avez étudiées jusqu’à présent :

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

Vous pouvez explorer [la documentation de GraphQL ;](https://graphql.org/learn/schema/){target="_blank"} pour en savoir plus sur le système de type, y compris la syntaxe de certains concepts qui ne sont pas représentés ici. L&#39;exemple ci-dessus, cependant, est explicatif. (Notez également à quel point la syntaxe est similaire à celle de la requête.) La définition d’un schéma GraphQL consiste simplement à exprimer les arguments et champs disponibles d’un type donné, ainsi que les types de ces champs. Chaque type de champ complexe doit lui-même comporter une définition, etc., par le biais de l’arborescence, jusqu’à ce que vous obteniez des types scalaires simples tels que `String`.

Le `input` une déclaration est à tous égards `type` mais définit un type qui peut être utilisé comme entrée pour un argument. Notez également que `interface` déclaration. Cette fonction fonctionne plus ou moins de la même manière que les interfaces en PHP. D’autres types héritent de cette interface.

La syntaxe `[CartItemInput!]!` a l&#39;air délicat mais est assez intuitif au final. Le `!` _inside_ le crochet déclare que chaque valeur du tableau doit être non nulle, tandis que la valeur _external_ déclare que la valeur du tableau elle-même doit être non nulle (par exemple, un tableau vide).

>[!NOTE]
>
>La logique de la manière dont les données sont récupérées et formatées selon un schéma, ainsi que la manière dont cette logique est mappée à des types particuliers, dépend de l’implémentation du runtime GraphQL. Toutefois, les implémentations doivent suivre un flux conceptuel logique à la lumière d’une compréhension autour des champs imbriqués : Une opération de résolution associée à la racine `Query` ou `Mutation` est exécuté, qui examine chaque champ spécifié dans la requête. Pour chaque champ qui correspond à un type complexe, une résolution similaire est effectuée pour ce type, etc., jusqu’à ce que tout ait été résolu en valeurs scalaires.

{{$include /help/_includes/graphql-rest-related-links.md}}
