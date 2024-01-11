---
title: Création d’un produit de lot
description: Découvrez comment créer un produit groupé à l’aide de l’API REST et de l’administrateur Commerce.
kt: 14589
doc-type: video
audience: all
activity: use
last-substantial-update: 2024-1-8
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
source-git-commit: e02540438df1cc85e6be7440351a72e77cfc1bf2
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---


# Création d’un produit de lot

Un produit groupé permet de regrouper plusieurs produits sous un produit parent. Ces produits enfants peuvent être un ensemble défini de produits ou proposer quelques variantes qui offrent des options de configuration flexibles aux clients. La configuration des types de produits par lots prend un peu plus de temps et vous devez planifier leur configuration. Cependant, l’offre de produits groupés améliore l’expérience d’achat en facilitant la personnalisation des sélections de produits par les clients.

Par exemple, vous pouvez proposer un groupe de produits appelé `Learning to surf` dans votre boutique Web. Le lot est le produit parent qui sert de conteneur pour les produits enfants attribués qui spécifient les options disponibles :

- Une planche de surf standard
- Une laisse de surf typique
- ailerons de surf rouge

Lorsque vous avez besoin d’une flexibilité supplémentaire, nous vous recommandons d’autoriser plusieurs options de produits enfants. Cela nécessite une utilisation plus complexe des options et des produits enfants. Pour développer l’exemple précédent, les options finales sont les suivantes :

- Une planche de surf standard
- Une laisse de surf typique
- Choix de la couleur de la fin :
   - Rouge
   - bleu
   - Jaune

Que le lot soit un groupe statique de produits simples ou plusieurs produits avec des variantes, les options de configuration flexibles font des types de produits du lot un outil de marchandisage puissant et unique pour la boutique Adobe Commerce.

Avant de créer un produit groupé, vérifiez que tous les produits simples à inclure dans le produit groupé sont disponibles dans Adobe Commerce. Créez tout qui n’existe pas.

Dans ce tutoriel, découvrez comment créer un produit en bundle à l’aide de l’API REST et de l’administrateur Adobe Commerce.

Utilisez l’API REST pour définir un produit de lot :

1. Créez des produits simples à utiliser dans le produit du lot.
1. Créez un lot de produits et associez les produits simples.
1. Obtenez le produit du lot et toutes les options associées.
1. Supprimez une option d’une section des produits du lot.
1. Restaurez les options d’un produit groupé.

Lors de la création de lots de produits à partir de l’administrateur Adobe Commerce, vous pouvez soit créer les produits simples en premier, soit utiliser l’outil automatisé pour créer des produits simples à l’aide d’un assistant.

## Pour qui est cette vidéo ?

- Chargés de site web
- Marchandisers e en eCommerce
- Nouveaux développeurs Adobe Commerce qui souhaitent apprendre à créer des produits en bundle dans Adobe Commerce à l’aide de l’API REST

## Contenu vidéo

>[!VIDEO](https://video.tv.adobe.com/v/3426797?learn=on)

## Création de produits avec REST

Les commandes suivantes créent tous les produits requis pour définir le produit du lot dans cet exemple : cinq produits simples et un produit du lot qui comporte trois options.

Avant d’envoyer la requête, mettez à jour l’exemple avec les valeurs de votre environnement.

- Modifier `"attribute-set": 4` à remplacer `4` avec l’identifiant du jeu d’attributs de votre environnement.

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

## Créez un produit groupé et affectez les produits simples comme options.

Créez un produit groupé en envoyant la demande de POST suivante.

Avant d’envoyer la requête, mettez à jour l’exemple avec les valeurs de votre environnement.

- Modifier `"attribute_set_id": 4,` et remplacer `4` avec l’identifiant du jeu d’attributs de votre environnement.

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

## Suppression d’une option d’un produit groupé

Supprimez un produit enfant d’un produit groupé sans supprimer le produit du catalogue en envoyant la demande de DELETE suivante à l’aide de cURL.

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/bundle-products/beginner-surfboard/options/35/children/blue-fins-and-fin-plugs' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: private_content_version=41d94540f5bd8b691017a850bc3b82b3'
```

## Restaurer les options de produit

Lors de la mise à jour des options de produit groupé, veillez à inclure toutes les options que vous souhaitez associer à ce produit. Si votre ensemble d’options d’origine contenait trois produits et qu’un a été supprimé, incluez les trois options dans la demande de POST pour vous assurer que le lot de produits spécifie toutes les options. Si vous avez inclus uniquement l’option que vous avez supprimée, le lot de produits mis à jour inclut uniquement cette option.

Recherchez l’ID d’option en examinant la réponse de création pour le produit du lot. Dans cette réponse, la variable `option_id` is `35`.

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

Mettez à jour le lot de produits pour ajouter l’option que vous avez supprimée en envoyant la demande de POST suivante.

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

- [Tutoriel sur la création d’un produit de lot](https://developer.adobe.com/commerce/webapi/rest/tutorials/bundle-product/){target="_blank"}
- [Produit groupé](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-bundle.html){target="_blank"}
- [Tutoriels Adobe Developer REST](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
