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
exl-id: bad3d926-2952-4bac-b685-adb16f009f8d
source-git-commit: 5d34c2e3b93c937139e88fa2f75dc6046f7093fc
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

>[!VIDEO](https://video.tv.adobe.com/v/3470802?learn=on&enablevpops&captions=fre_fr)

## Exemples de code

Veillez à échanger des valeurs telles que `{{insert-your-graphql-endpoint-url}}`, `{{insert-your-ac-view-id}}` et `{{your-search-query-string}}` avec les valeurs nécessaires sur votre requête.

Exemple de requête de base

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}'
```

Exemple de requête de base à l’aide de `jq` pour imprimer la sortie avec soin

```bash
curl '{{insert-your-graphql-endpoint-url}}' \
-H 'Content-Type: application/json' \
-H 'AC-View-ID: {{insert-your-ac-view-id}}' \
-d '{"query": "query ProductSearch($search: String!) { productSearch( phrase: $search, page_size: 10, current_page: 2) { items { productView { sku name description shortDescription images { url } ... on SimpleProductView { attributes { label name value } price { regular { amount { value currency } } roles } } } } } }", "variables": { "search": "{{your-search-query-string}}"}}' | jq .
```

## Contenu connexe

* [Prise en main de l’API de marchandisage](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
* [[!DNL Adobe Commerce Optimizer]  Guide ](https://experienceleague.adobe.com/fr/docs/commerce/optimizer/overview){target="_blank"}
