---
title: Créer un produit groupé
description: Découvrez comment créer un produit groupé à l’aide de l’API REST et de l’administrateur Commerce.
kt: 14585
doc-type: video
duration: 979
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00.000Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 3ad7125b-ef6d-4ea0-9fa7-8fc9eb399ec1
TQID: 'https://experienceleague.adobe.com/nosJh3ytiC54wmNWaUmSu9qjZCN-ssjolNZD702EpEg'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
    internal-label: Catalog management
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: cc04bd17-78d5-5120-8c3d-1b8a57e49280
    internal-label: Integration
  - id: e7dae43f-215c-4cdf-90d3-c5a461a6e669
    internal-label: Admin tools and workspace
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
subfeature_v2:
  - id: bb2df8be-afdd-4818-b6b5-95ca1dd3bc3a
    internal-label: Admin workspace
  - id: e0ca0e7a-9738-48d1-b98b-615468ab4aaf
    internal-label: REST API
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '559'
ht-degree: 0%
---
# Créer un produit groupé

Un produit regroupé se compose de produits simples autonomes présentés sous la forme d’un groupe. Vous pouvez proposer des variations d’un seul produit ou les regrouper par saison ou thème. Avant de créer un produit regroupé, vérifiez que tous les produits simples à inclure dans le groupe sont disponibles dans Adobe Commerce et créez-en d’autres qui n’existent pas.

Dans ce tutoriel, vous apprendrez à créer un produit groupé à l’aide de l’API REST et de l’administrateur Adobe Commerce.

Utilisez l’API REST pour créer un produit de groupe dans Admin :

1. Créez un produit groupé vide.
1. Créez des produits simples à utiliser dans le produit regroupé.
1. Renseignez le produit groupé vide avec des produits simples.
1. Créez un produit groupé vide et associez les produits simples.

   Lorsque vous associez des produits simples au produit regroupé, l’attribut d’ordre de tri (`position`) dans la payload est utilisé par le front-end pour afficher les produits associés dans l’ordre souhaité. Si l’attribut `position` n’est pas spécifié, les produits sont affichés dans l’ordre dans lequel ils ont été ajoutés au produit regroupé.

Lors de la création de produits groupés à partir de l’administrateur Adobe Commerce, commencez par créer les produits simples. Lorsque vous êtes prêt à créer le produit regroupé, associez les produits simples en les affectant au produit regroupé dans un lot.

## À qui s&#39;adresse cette vidéo ?

* Gestionnaires de site web
* Marchandiseurs eCommerce
* Les nouveaux développeurs Adobe Commerce qui souhaitent apprendre à créer des produits groupés dans Adobe Commerce à l’aide de l’API REST.

## Contenu vidéo

>[!VIDEO](https://video.tv.adobe.com/v/3454040?captions=fre_fr&learn=on)

## Paramétrage pour le produit groupé

Dans cet exemple, il existe trois produits simples (créés en premier) et un produit regroupé. Deux produits simples sont associés au produit regroupé, puis le troisième produit simple est ajouté au produit regroupé.

## Créer le premier produit simple à l’aide de cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-one",
    "name": "Simple product one",
    "attribute_set_id": 4,
    "price": 1.11,
    "type_id": "simple"
  }
}
```

## Créer le second produit simple à l’aide de cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-two",
    "name": "Simple Product two",
    "attribute_set_id": 4,
    "price": 2.22,
    "type_id": "simple"
  }
}
```

## Créer le troisième produit simple à l’aide de cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=a42da0096288718c9556f77549c6305f; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "product-sku-three",
    "name": "Simple product three",
    "attribute_set_id": 4,
    "price": 3.33,
    "type_id": "simple"
  }
}
```

## Créer un produit groupé vide à l’aide de cURL

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "product":{
        "sku":"my-new-grouped-product",
        "name":"This is my New Grouped Product",
        "attribute_set_id":4,
        "type_id":"grouped",
        "visibility":4
    }
}
'
```

## Ajouter les premier et deuxième produits simples au produit regroupé

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=28a020734488eef2600f8d5c7f302b53; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
    "items":[
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-one",
            "linked_product_type":"simple",
            "position":1,
            "extension_attributes":{
            "qty":1
            }
        },
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
    ]
}
'
```

## Ajouter le troisième produit simple au produit groupé existant

Indiquez le numéro de position approprié (tout sauf `1` ou `2`), utilisé pour les deux premiers produits initialement associés au produit regroupé. Pour cet exemple, la position est `4`.

```bash
curl --location --request PUT '{{your.url.here}}/rest/default/V1/products/my-new-grouped-product/links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2' \
--data '{
    "entity":{
        "sku":"my-new-grouped-product",
        "link_type":"associated",
        "linked_product_sku":"product-sku-three",
        "linked_product_type":"simple",
        "position":4,
        "extension_attributes":{
            "qty":1
        }
    }
}

'
```

## Supprimer un produit simple d’un produit groupé

Pour [supprimer un produit simple](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/) d’un produit regroupé, utilisez : `DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}`.

Pour découvrir les éléments à utiliser comme `{type}`, utilisez xdebug pour capturer la requête et évaluer $linkTypes : `related`, `crosssell`, `uupsell` et `associated`.
![Types de liens de produit groupés - autre texte](/help/assets/site-management/catalog/grouped-types.png "Types de liens de produit groupés capturés au cours de la session xdebug")

Lors de la liaison des produits simples au produit regroupé, la payload contenait quelques sections similaires à :

```bash
        {
            "sku":"my-new-grouped-product",
            "link_type":"associated",
            "linked_product_sku":"product-sku-two",
            "linked_product_type":"simple",
            "position":2,
            "extension_attributes":{
            "qty":1
            }
        }
```

Dans la payload, la valeur `link_type` `associated` fournit la valeur `{type}` requise dans la requête DELETE. L’URL de la requête sera similaire à `/V1/products/my-new-grouped-product/links/associated/product-sku-three`.

Voir la requête cURL pour supprimer le produit simple avec le SKU `product-sku-three` du produit regroupé avec le SKU `my-new-grouped-product` :

```bash
curl --location --request DELETE '{{your.url.here}}rest/default/V1/products/my-new-grouped-product/links/associated/product-sku-three' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=9e61396705e9c17423eca2bdf2deefb2'
```

## Obtenir un produit groupé à l’aide de cURL

```bash
curl --location '{{your.url.here}}rest/default/V1/products/some-grouped-product-sku' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=3b797a6cc3c5c71f2193109fb9838b12'
```

## Ressources supplémentaires

* [Créer et gérer des produits groupés](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
* [Produit groupé](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html?lang=fr){target="_blank"}
* [Tutoriels REST Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
