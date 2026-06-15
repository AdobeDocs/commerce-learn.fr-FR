---
title: Comment interroger des données
description: Découvrez comment interroger les données de produit Adobe Commerce Optimizer à l’aide de GraphQL, notamment comment formater des réponses JSON avec des variables de requête json et structurer.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 204
last-substantial-update: 2025-08-13
jira: KT-18548
exl-id: bad3d926-2952-4bac-b685-adb16f009f8d
TQID: https://experienceleague.adobe.com/IxrS6rwleWgU0-jtwu4hUavQuZesbQ6h5z7zVZR2xCo
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: bfe282e4f1ef04985cffb109bce90bc05a70fda0
workflow-type: tm+mt
source-wordcount: 127
ht-degree: 0%

---

# Adobe Commerce Optimizer des données de requête

Découvrez comment interroger des données à l’aide de GraphQL sur une instance Adobe Commerce Optimizer.

## À qui s&#39;adresse cette vidéo ?

* Architecte de solution Commerce et développeurs

## Contenu vidéo

* Interroger des données à l’aide de GraphQL
* Utilisation de json pour faciliter la lecture du fichier json

>[!VIDEO](https://video.tv.adobe.com/v/3470802?captions=fre_fr&learn=on)

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

* [Prise en main de l’API de marchandisage](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}
* [Guide [!DNL Adobe Commerce Optimizer]](https://experienceleague.adobe.com/fr/docs/commerce/optimizer/overview){target="_blank"}
