---
title: Créer un produit groupé
description: Découvrez comment créer un produit groupé à l’aide de l’API REST et de l’administrateur Commerce.
kt: 14585
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-30T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: eec9a85198f963404f5ba82fc2fc76315a82f964
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 0%

---


# Créer un produit groupé

Un produit groupé est constitué de produits autonomes simples présentés sous la forme d’un groupe. Vous pouvez proposer des variantes d’un seul produit ou les regrouper par saison ou thème. Avant de créer un produit groupé, vérifiez que tous les produits simples à inclure dans le groupe sont disponibles dans Adobe Commerce et créez-en d’autres qui n’existent pas. Dans ce tutoriel, vous apprendrez à créer un produit groupé à l’aide de l’API REST et de l’administrateur Adobe Commerce.

Utilisez l’API REST pour créer un produit de groupe dans l’Admin :

1. Créez un produit groupé vide.
1. Créez des produits simples à utiliser dans le produit groupé.
1. Renseignez le produit groupé vide avec des produits simples.
1. Créez un produit groupé vide et associez les produits simples.

Lors de la création de produits groupés dans l’administrateur Adobe Commerce, il est recommandé de créer d’abord des produits simples. Lorsque vous êtes prêt à créer le produit groupé, associez les produits simples en les affectant au produit groupé dans un seul lot.

L’attribut d’ordre de tri dans la payload est obligatoire et est utilisé par le front-end pour afficher les produits associés dans l’ordre souhaité.

## Pour qui est cette vidéo ?

- Chargés de site web
- Marchandisers e en eCommerce
- Nouveaux développeurs Adobe Commerce qui souhaitent apprendre à créer des produits groupés dans Adobe Commerce à l’aide de l’API REST.

## Contenu vidéo

>[!VIDEO](https://video.tv.adobe.com/v/3425920?learn=on)

## Configuration du produit groupé

Dans cet exemple, il existe trois produits simples (créés en premier) et un produit groupé. Deux produits simples sont associés au produit groupé, puis le troisième produit simple est ajouté au produit groupé.

## Création du premier produit simple à l’aide de cURL

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

## Création du second produit simple à l’aide de cURL

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

## Ajouter les premier et deuxième produits simples au produit groupé

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

Inclure le numéro de position approprié (sauf `1` ou `2`), qui sont utilisés pour les deux premiers produits associés à l’origine au produit groupé. Dans cet exemple, la position est : `4`.

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

## Supprimer un produit simple d’un produit regroupé

À [suppression d’un produit simple](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/) à partir d’un produit groupé, utilisez : `DELETE /V1/products/{sku}/links/{type}/{linkedProductSku}`.

Pour découvrir comment utiliser `{type}`, utilisez xdebug pour capturer la requête et évaluer $linkTypes : `related`, `crosssell`, `uupsell`, et `associated`.
![Types de lien de produit groupé - texte de remplacement](/help/assets/site-management/catalog/grouped-types.png "Types de lien de produit regroupés capturés au cours de la session de débogage")

Lors de la liaison des produits simples au produit groupé, la payload contenait quelques sections similaires à :

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

Dans la payload, la variable `link_type` value `associated` fournit la variable `{type}` valeur requise dans la requête du DELETE. L’URL de demande est similaire à `/V1/products/my-new-grouped-product/links/associated/product-sku-three`.

Consultez la requête cURL pour supprimer le produit simple avec la fonction `product-sku-three` SKU du produit regroupé avec la variable `my-new-grouped-product` SKU :

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

- [Créer et gérer des produits groupés](https://developer.adobe.com/commerce/webapi/rest/tutorials/grouped-product/){target="_blank"}
- [Produit groupé](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-grouped.html){target="_blank"}
- [Tutoriels Adobe Developer REST](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
