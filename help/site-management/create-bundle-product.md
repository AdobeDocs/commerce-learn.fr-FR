---
title: Créer un produit groupé
description: Découvrez comment créer un bundle de produits à l’aide de l’API REST et de l’administrateur Commerce.
kt: 14589
doc-type: video
duration: 1335
audience: all
activity: use
last-substantial-update: 2024-1-8
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 5d688e6a-ae8c-4a55-b16c-5d3ae2d1bfd5
source-git-commit: 9aa4d70ee6a3825f027aa2a9c6a1ac0f876ed59f
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---

# Créer un produit groupé

Un produit groupé est un moyen de regrouper plusieurs produits sous un produit parent. Ces produits enfants peuvent être un ensemble défini de produits ou offrir quelques variations qui offrent des options de configuration flexibles aux clients. La configuration des types de produits groupés prend un peu plus de temps et vous devez effectuer une certaine planification avant de les configurer. Cependant, l’offre groupée améliore l’expérience d’achat en permettant aux clients de personnaliser plus facilement leurs sélections de produits.

Par exemple, vous pouvez proposer une offre groupée de produits appelée `Learning to surf` dans votre boutique en ligne. Le lot est le produit parent qui sert de conteneur pour les produits enfants affectés qui spécifient les options disponibles :

* Une planche de surf standard
* Une laisse de planche de surf typique
* Nageoires rouges pour planche de surf

Lorsqu’une flexibilité supplémentaire est souhaitée, il est recommandé d’autoriser plusieurs options de produits enfants. Cela nécessite une utilisation plus complexe des options et des produits enfants. Pour développer l’exemple précédent, les dernières options sont les suivantes :

* Une planche de surf standard
* Une laisse de planche de surf typique
* Choix de la couleur des nageoires :
   * Rouge
   * Bleu
   * Jaune

Qu’il s’agisse d’un groupe statique de produits simples ou de plusieurs produits avec des variations, les options de configuration flexibles font des types de produits groupés un outil de marchandisage unique et puissant pour la boutique Adobe Commerce.

Avant de créer un produit groupé, vérifiez que tous les produits simples à inclure dans le produit groupé sont disponibles dans Adobe Commerce. Créez les qui n’existent pas.

Dans ce tutoriel, apprenez à créer un bundle de produits à l’aide de l’API REST et de l’administrateur Adobe Commerce.

Utilisez l’API REST pour définir un produit groupé :

1. Créez des produits simples à utiliser dans le produit groupé.
1. Créez un produit groupé et associez les produits simples.
1. Obtenez le produit groupé et toutes les options associées.
1. Supprimez une option d’une section du lot de produits.
1. Restaurez les options d’un produit groupé.

Lors de la création de lots de produits à partir de l’administration Adobe Commerce, vous pouvez soit commencer par créer les produits simples, soit utiliser l’outil automatisé pour créer des produits simples à l’aide d’un assistant.

## À qui s&#39;adresse cette vidéo ?

* Gestionnaires de site web
* Marchandiseurs eCommerce
* Nouveaux développeurs Adobe Commerce qui souhaitent apprendre à créer des produits groupés dans Adobe Commerce à l’aide de l’API REST.

## Contenu vidéo

>[!VIDEO](https://video.tv.adobe.com/v/3426797?learn=on)

## Créer des produits avec REST

Les commandes suivantes créent tous les produits requis pour définir le produit groupé dans cet exemple : cinq produits simples et un produit groupé qui comporte trois options.

Avant d’envoyer la requête, mettez à jour l’exemple avec des valeurs pour votre environnement.

* Modifiez `"attribute-set": 4` pour remplacer `4` par l’ID de jeu d’attributs de votre environnement.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "10-ft-beginner-surfboard",
    "name": "10 Foot Beginner surfboard",
    "attribute_set_id": 4,
    "price": 100,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "8-ft-surboard-leash",
    "name": "8 Foot surboard leash",
    "attribute_set_id": 4,
    "price": 50,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "red-fins-and-fin-plugs",
    "name": "Red fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "blue-fins-and-fin-plugs",
    "name": "Blue fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "yellow-fins-and-fin-plugs",
    "name": "Yellow fins and fin plugs",
    "attribute_set_id": 4,
    "price": 15,
    "type_id": "simple",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      }
    }
  }
}
'
```

## Créez un produit groupé et affectez les produits simples en tant qu’options

Créez un produit groupé en envoyant la requête POST suivante.

Avant d’envoyer la requête, mettez à jour l’exemple avec des valeurs pour votre environnement.

* Modifiez le `"attribute_set_id": 4,` et remplacez `4` par l’ID de jeu d’attributs de votre environnement.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "product": {
    "sku": "beginner-surfboard",
    "name": "Beginner Surfboard",
    "attribute_set_id": 4,
    "status": 1,
    "visibility": 4,
    "type_id": "bundle",
    "extension_attributes": {
      "stock_item": {
        "qty": 100,
        "is_in_stock":true
      },
      "bundle_product_options": [
        {
          "option_id": 0,
          "position": 1,
          "sku": "surfboard-essentials",
          "title": "Beginners 10ft Surfboard",
          "type": "checkbox",
          "required": true,
          "product_links": [
            {
              "sku": "10-ft-beginner-surfboard",
              "option_id": 1,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 100,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        },
        {
          "option_id": 1,
          "position": 2,
          "sku": "surboard-leash",
          "title": "Surfboard leash",
          "type": "checkbox",
          "required": true,
          "product_links": [
            {
              "sku": "8-ft-surboard-leash",
              "option_id": 2,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 50,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        },
        {
          "option_id": 3,
          "position": 2,
          "sku": "surfboard-color",
          "title": "Choose a color for the fins and fin plugs",
          "type": "radio",
          "required": true,
          "product_links": [
            {
              "sku": "red-fins-and-fin-plugs",
              "option_id": 2,
              "qty": 1,
              "position": 1,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            },
            {
              "sku": "blue-fins-and-fin-plugs",
              "option_id": 2,
              "qty": 1,
              "position": 2,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            },
            {
              "sku": "yellow-fins-and-fin-plugs",
              "option_id": 3,
              "qty": 1,
              "position": 3,
              "is_default": false,
              "price": 0,
              "price_type": 0,
              "can_change_quantity": 0
            }
          ]
        }
      ]
    },
    "custom_attributes": [
      {
        "attribute_code": "price_view",
        "value": "0"
      }
    ]
  },
  "saveOptions": true
}
'
```

## Supprimer une option d’un produit groupé

Supprimez un produit enfant d’un produit groupé sans supprimer le produit du catalogue en envoyant la requête DELETE suivante à l’aide de cURL.

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/bundle-products/beginner-surfboard/options/35/children/blue-fins-and-fin-plugs' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3'
```

## Restaurer les options du produit

Lors de la mise à jour des options de l’offre groupée, veillez à inclure toutes les options que vous souhaitez associer à ce produit. Si votre jeu d’options d’origine contenait trois produits et que l’un d’eux a été supprimé, incluez les trois options dans la requête POST pour vous assurer que le bundle de produits spécifie toutes les options. Si vous avez inclus uniquement l’option que vous avez supprimée, le bundle de produits mis à jour inclut uniquement cette option.

Recherchez l’ID de l’option en examinant la réponse de la création pour le produit groupé. Dans cette réponse, la `option_id` est `35`.

```json
...
,
            {
                "option_id": 35,
                "title": "added back color options for fins and fin plugs",
                "required": true,
                "type": "radio",
                "position": 2,
                "sku": "beginner-surfboard",
                "product_links": [
                    {
                        "id": "77",
                        "sku": "red-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 1,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    },
                    {
                        "id": "78",
                        "sku": "blue-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 2,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    },
                    {
                        "id": "79",
                        "sku": "yellow-fins-and-fin-plugs",
                        "option_id": 35,
                        "qty": 1,
                        "position": 3,
                        "is_default": false,
                        "price": 15,
                        "price_type": null,
                        "can_change_quantity": 0
                    }
                ]
            }
...
```

Mettez à jour l’offre groupée de produits pour ajouter l’option que vous avez supprimée en envoyant la requête POST suivante.

```bash
curl --location '{{your.url.here}}/rest/default/V1/bundle-products/options/add' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3' \
--data '{
  "option": {
    "option_id": 35,
    "title": "Choose a color for the fins and fin plugs",
    "required": true,
    "type": "radio",
    "position": 2,
    "sku": "beginner-surfboard",
    "product_links": [
      {
        "sku": "red-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 1,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      },
      {
        "sku": "blue-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 2,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      },
      {
        "sku": "yellow-fins-and-fin-plugs",
        "option_id": 35,
        "qty": 1,
        "position": 3,
        "is_default": false,
        "price": 0,
        "price_type": null,
        "can_change_quantity": 0,
        "extension_attributes": {}
      }
    ],
    "extension_attributes": {}
  }
}'
```

## Ressources supplémentaires

* [Tutoriel sur la création d’une offre groupée](https://developer.adobe.com/commerce/webapi/rest/tutorials/bundle-product/){target="_blank"}
* [Produit groupé](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-bundle.html?lang=fr){target="_blank"}
* [Tutoriels Adobe Developer REST](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
