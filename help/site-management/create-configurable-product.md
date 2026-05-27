---
title: Création d’un produit configurable
description: Découvrez comment créer un produit configurable à l’aide de l’API REST et de l’administrateur Commerce.
kt: 14586
doc-type: video
duration: 1760
audience: all
activity: use
last-substantial-update: 2023-12-15T00:00:00.000Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 112bec9a-0f8e-4252-8c52-f486a5e663b5
TQID: https://experienceleague.adobe.com/XAvtOnOIycqQ4z-uztWuVzzv0--eVit-I-QDnl67ba8
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 994
ht-degree: 0%

---

# Création d’un produit configurable

Un produit configurable est un produit parent de plusieurs produits simples. Définissez un produit configurable pour obliger l&#39;acheteur à faire un ou plusieurs choix pour sélectionner une variation de produit spécifique. Par exemple, si le produit est une chemise, l’acheteur doit choisir les options de taille et de couleur pour sélectionner la chemise.

Bien qu’un produit configurable utilise plus de SKU et puisse prendre un peu plus de temps à configurer au début, il peut vous faire gagner du temps à la fin. Si vous prévoyez faire croître votre entreprise, le type de produit configurable est un bon choix pour les produits à options multiples.

Avant de créer un produit configurable, vérifiez que tous les produits simples à inclure dans le produit configurable sont disponibles dans Adobe Commerce. Créez les qui n’existent pas.

Dans ce tutoriel, découvrez comment créer un produit configurable à l’aide de l’API REST et de l’administrateur Adobe Commerce.

Utilisez l’API REST pour créer un produit configurable :

1. Obtenez les attributs d’un [jeu d’attributs](https://experienceleague.adobe.com/docs/commerce-admin/catalog/product-attributes/create/attribute-sets.html?lang=fr) afin d’utiliser les numéros d’ID pour les appels d’API suivants.
1. Créez des produits simples à utiliser dans le produit configurable.
1. Créez un produit configurable vide et associez les produits simples.
1. Définissez les attributs de produit pour le produit configurable.
1. Renseignez le produit configurable vide avec des produits simples.
1. Obtenez le produit configurable et tous les attributs.
1. Obtenez les produits enfants attribués au produit configurable.
1. Supprimez l’association de produits simples à des produits configurables.

Lors de la création de produits configurables à partir de l’administration Adobe Commerce, vous pouvez soit commencer par créer les produits simples, soit utiliser l’outil automatisé qui crée de nouveaux produits simples à utiliser à l’aide de l’assistant.

## À qui s&#39;adresse cette vidéo ?

* Gestionnaires de site web
* Marchandiseurs eCommerce
* Les nouveaux développeurs Adobe Commerce qui souhaitent apprendre à créer des produits configurables dans Adobe Commerce à l’aide de l’API REST.

## Contenu vidéo

>[!VIDEO](https://video.tv.adobe.com/v/3426381?learn=on)

## Obtention des attributs de couleur à l’aide de cURL

Dans cet exemple, l’ensemble des attributs avec tous les attributs individuels est renvoyé pour le jeu d’attributs 10. Il peut être long, des centaines de lignes ne sont pas rares. Lors de la révision de la réponse, l’ID d’attribut de la couleur se trouve probablement au milieu. Accélérez la recherche de ces valeurs à l’aide de grep ou d’autres méthodes pour rechercher les résultats. Ma réponse se trouvait près de la ligne 665 et est incluse dans le fragment de code suivant de la réponse JSON.

```json
...
{
        "attribute_id": 93,
        "attribute_code": "color",
        "frontend_input": "select",
        "entity_type_id": "4",
        "is_required": false,
        "options": [
            {
                "label": " ",
                "value": ""
            },
            {
                "label": "Red",
                "value": "13"
            },
            {
                "label": "Blue",
                "value": "14"
            },
            {
                "label": "Green",
                "value": "15"
            }
        ],
...
```


Pour récupérer les identifiants d’attribut afin de configurer votre produit configurable, mettez à jour la partie `attribute-sets/10/attributes` de la requête cURL suivante pour remplacer `10` avec l’identifiant de jeu d’attributs dans votre environnement. Cette requête utilise la méthode GET.

```bash
curl --location '{{your.url.here}}rest/V1/products/attribute-sets/10/attributes' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Créer le premier produit simple à l’aide de cURL

### Ajuster les identifiants d’environnement et les détails du produit

Créez le premier produit simple en utilisant l’API pour envoyer la requête POST suivante à l’aide de cURL.

Avant d’envoyer la requête, mettez à jour l’exemple avec des valeurs pour votre environnement.

* Modifiez `"attribute-set": 10` pour remplacer `10` par l’ID de jeu d’attributs de votre environnement.
* Modifiez `"value": "13"` pour remplacer `13` par la valeur de votre environnement.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-red",
    "name": "Kids Hawaiian Ukulele Red",
    "attribute_set_id": 10,
    "price": 12.50,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "10",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "13"
        }
    ]
  }
}
'
```

## Créer le second produit simple à l’aide de cURL

Créez le deuxième produit simple en utilisant l’API pour envoyer la requête POST suivante à l’aide de cURL.

Avant d’envoyer la requête, mettez à jour l’exemple avec des valeurs pour votre environnement.

* Modifiez le `"attribute_set_id": 10,` et remplacez `10` par l’ID de jeu d’attributs à partir de dans votre environnement.
* Modifiez `"value": "14"` et remplacez `14` par la valeur de votre environnement.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Blue",
    "name": "Kids Hawaiian Ukulele Blue",
    "attribute_set_id": 10,
    "price": 15,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "20",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "14"
        }
    ]
  }
}
'
```

## Créer le troisième produit simple à l’aide de cURL

Créez le troisième produit simple en envoyant la requête POST suivante à l’aide de cURL.

Avant d’envoyer la requête, mettez à jour l’exemple avec des valeurs pour votre environnement.

* Modifiez `"attribute_set_id": 10,` pour remplacer `10` par l’ID de jeu d’attributs de votre environnement.
* Modifiez `"value": "15"` et remplacez `15` par la valeur de votre environnement.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele-Green",
    "name": "Kids Hawaiian Ukulele Green",
    "attribute_set_id": 10,
    "price": 25,
    "status": 1,
    "visibility": 1,
    "type_id": "simple",
    "weight": "0.5",
    "extension_attributes": {
        "stock_item": {
            "qty": "30",
            "is_in_stock": true
        }
    },
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "15"
        }
    ]
  }
}
'
```

## Créer un produit configurable vide à l’aide de cURL

Créez un produit configurable vide en envoyant la requête POST suivante à l’aide de cURL.

Avant d’envoyer la requête, mettez à jour l’exemple avec des valeurs pour votre environnement.

* Modifiez le `"attribute_set_id": 10,` et remplacez `10` par l’ID de jeu d’attributs de votre environnement.
* Modifiez `"value": "93"` et remplacez `93` par la valeur de votre environnement.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "product": {
    "sku": "Kids-Hawaiian-Ukulele",
    "name": "Kids Hawaiian Ukulele",
    "attribute_set_id": 10,
    "status": 1,
    "visibility": 4,
    "type_id": "configurable",
    "weight": "0.5",
    "custom_attributes": [
        {
            "attribute_code": "color",
            "value": "93"
        }
    ]
  }
}'
```

## Définir les options disponibles pour le produit configurable

Définissez les options disponibles pour le produit configurable en envoyant la requête POST suivante à l’aide de cURL.

Avant d’envoyer la requête, modifiez la `"attribute_id": 93,` pour remplacer `93` avec l’ID d’attribut de votre environnement.

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/options' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "option": {
    "attribute_id": "93",
    "label": "Color",
    "position": 0,
    "is_use_default": true,
    "values": [
      {
        "value_index": 9
      }
    ]
  }
}'
```

Si vous oubliez de définir les options du produit configurable (parent), une erreur se produit lorsque vous essayez d’associer un produit enfant au produit configurable. Le message d’erreur est similaire à l’exemple suivant :

`{"message":"The parent product doesn't have configurable product options.","trace":"#0 [internal function]: Magento\\ConfigurableProduct\\Model\\LinkManagement->addChild('Kids-Hawaiian-U...'}`

## Lier le produit enfant au configurable

Vous avez à présent créé trois produits simples :

* `"Kids Hawaiian Ukulele Red"`,
* `"Kids-Hawaiian-Ukulele-Blue"`
* `"Kids-Hawaiian-Ukulele-Green"`

Ajoutez ces produits simples en tant qu’enfants du produit configurable en envoyant la requête POST suivante. Envoyez une demande distincte pour chaque produit.

Pour chaque requête, mettez à jour la valeur du `childSKU` avec la valeur du produit enfant que vous ajoutez. L’exemple suivant attribue la `kids-Hawaiian-Ukulele-red` de produit simple au produit configurable avec la `Kids-Hawaiian-Ukulele-red` de SKU.


```bash
curl --location '{{your.url.here}}rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/child' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=aff63a1634f6f1a773e7a4894bf1a55c' \
--data '{
  "childSku": "Kids-Hawaiian-Ukulele-red"
}
'
```

## Obtention d’un produit configurable à l’aide de cURL

Maintenant que vous avez créé un produit configurable avec trois SKU enfants attribués. Vous pouvez afficher les identifiants liés pour les produits affectés en envoyant la requête GET suivante à l’aide de cURL. Cette requête renvoie des informations détaillées sur le produit configurable.

```json
...
        "configurable_product_links": [
            155,
            157,
            156
        ]
...
```

L’exemple suivant utilise la méthode GET

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/Kids-Hawaiian-Ukulele' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Obtenir le produit enfant associé à un produit configurable

Renvoyez uniquement les enfants associés au produit configurable en envoyant la requête GET suivante. La réponse inclut tous les attributs du produit enfant, y compris le SKU et le prix.

L’exemple suivant utilise la méthode GET

```bash
curl --location '{{your.url.here}}/rest/default/V1/configurable-products/kids-hawaiian-ukulele/children' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Supprimer ou supprimer un produit enfant du parent configurable

Vous pouvez supprimer un produit enfant d’un produit configurable sans supprimer le produit du catalogue en envoyant la requête DELETE suivante à l’aide de cURL.

```bash
curl --location --request DELETE '{{your.url.here}}/rest/default/V1/configurable-products/Kids-Hawaiian-Ukulele/children/Kids-Hawaiian-Ukulele-Blue' \
--header 'Authorization: Bearer {{Your Bearer Token}}'
```

## Ressources supplémentaires

* [Tutoriel sur la création d’un produit configurable](https://developer.adobe.com/commerce/webapi/rest/tutorials/configurable-product/){target="_blank"}
* [Produit configurable](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-configurable.html?lang=fr){target="_blank"}
* [Tutoriels REST Adobe Developer](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
* [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
