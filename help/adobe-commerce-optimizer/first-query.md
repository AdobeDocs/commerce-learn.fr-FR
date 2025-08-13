---
title: Comment interroger des données
description: Découvrez comment interroger des données pour une instance Adobe Commerce Optimizer.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 182
last-substantial-update: 2025-08-13T00:00:00Z
jira: KT-18548
source-git-commit: c598e46f7119ebdb1575e41c65d6285109fd9af9
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Adobe Commerce Optimizer des données de requête

Découvrez comment interroger des données à l’aide de GraphQL sur une instance Adobe Commerce Optimizer.

## À qui s&#39;adresse cette vidéo ?

* Architecte de solution Commerce et développeurs

## Contenu vidéo

* Interroger des données à l’aide de GraphQL
* Utilisation de json pour faciliter la lecture du fichier json

>[!VIDEO](https://video.tv.adobe.com/v/3470800?learn=on&enablevpops)

## Exemples de code

Veillez à échanger des valeurs telles que `{{insert-your-graphql-endpoint-url}}`, `{{insert-your-ac-source-locale}}` et `{{your-search-query-string}}` avec les valeurs nécessaires sur votre requête.

Exemple de requête de base

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

Exemple de requête de base à l’aide de `jq` pour imprimer la sortie avec soin

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-Source-Locale: {{insert-your-ac-source-locale}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## Contenu connexe

* [Prise en main de l’API de marchandisage](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer]  Guide ](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}
