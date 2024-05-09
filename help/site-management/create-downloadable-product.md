---
title: Créer un produit téléchargeable
description: Découvrez comment créer un produit téléchargeable à l’aide de l’API REST et de l’administrateur Adobe Commerce.
kt: 14464
doc-type: video
audience: all
activity: use
last-substantial-update: 2023-11-16T00:00:00Z
feature: Catalog Management, Admin Workspace, Backend Development, Integration, REST
topic: Commerce, Integrations, Content Management
role: Developer, User
level: Beginner
exl-id: 90753b8d-eca0-4868-b40e-9563d2b0e1e8
source-git-commit: 8ef4b0e0a0e4dfffdef8759e4ac7659ed854fae2
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# Créer un produit téléchargeable

Découvrez comment créer un produit téléchargeable à l’aide de l’API REST et de l’administrateur Adobe Commerce.

## Pour qui est cette vidéo ?

- Chargés de site web
- Marchandisers e en eCommerce
- Nouveaux développeurs Adobe Commerce qui souhaitent apprendre à créer des produits dans Adobe Commerce à l’aide de l’API REST

## Contenu vidéo

>[!VIDEO](https://video.tv.adobe.com/v/3425753?learn=on)

## Domaines téléchargeables autorisés

Vous devez indiquer les domaines autorisés pour autoriser les téléchargements. Les domaines sont ajoutés au `env.php` via la ligne de commande. La variable `env.php` détaille les domaines autorisés à contenir du contenu téléchargeable. Une erreur se produit si un produit téléchargeable est créé à l’aide de l’API REST. _before_  la valeur `php.env` est mis à jour :

```bash
{
    "message": "Link URL's domain is not in list of downloadable_domains in env.php."
}
```

Pour définir le domaine, connectez-vous au serveur : `bin/magento downloadable:domains:add www.example.com`

Une fois cette opération terminée, la variable `env.php` est modifié dans la fonction _downloadable_domains_ tableau.

```php
    'downloadable_domains' => [
        'www.example.com'
    ],
```

Maintenant que le domaine est ajouté à la variable `env.php`, vous pouvez créer un produit téléchargeable dans l’administrateur Adobe Commerce ou à l’aide de l’API REST.

Voir [Référence de configuration](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html#downloadable_domains) pour en savoir plus.

>[!IMPORTANT]
>Dans certaines versions d’Adobe Commerce, l’erreur suivante peut s’afficher lorsqu’un produit est modifié dans l’administrateur Adobe Commerce. Le produit est créé à l’aide de l’API REST, mais le téléchargement lié comporte une `null` prix.

`Deprecated Functionality: number_format(): Passing null to parameter #1 ($num) of type float is deprecated in /app/vendor/magento/module-downloadable/Ui/DataProvider/Product/Form/Modifier/Data/Links.php on line 228`.

Pour corriger cette erreur, utilisez l’API du lien de mise à jour : `POST V1/products/{sku}/downloadable-links.`

Voir [Mettre à jour un lien de téléchargement de produit à l’aide de cURL](#update-downloadable-links) pour plus d’informations.

## Créer un produit téléchargeable à l’aide de cURL (téléchargement depuis un serveur distant)

Cet exemple montre comment créer un produit téléchargeable à l’aide de cURL lorsque le fichier à télécharger ne se trouve pas sur le même serveur. Ce cas pratique est typique si le fichier est stocké dans un compartiment S3 ou un autre gestionnaire de ressources numériques.

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "product": {
    "sku": "POSTMAN-download-product-1",
    "name": "POSTMAN download product-1",
    "attribute_set_id": 4,
    "price": 9.9,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        
        "downloadable_product_links": [
            {
                "title": "My url link",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "url",
                "link_url": "{{location.url.where.file.exists}}/some-file.zip",
                "sample_type": "url",
                "sample_url": "{{location.url.where.file.exists}}/sample-file.zip"
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}
'
```

## Créer un produit téléchargeable à l’aide de cURL (téléchargement depuis le serveur d’applications Commerce)

Cet exemple montre comment utiliser cURL pour créer un produit téléchargeable depuis l’administrateur Adobe Commerce lorsque le fichier est stocké sur le même serveur que l’application Adobe Commerce.

Dans ce cas pratique, lorsque l’administrateur qui gère le catalogue choisit `upload file`, le fichier est transféré à la fonction `pub/media/downloadable/files/links/` répertoire .  L’automatisation crée les fichiers et les déplace vers leurs emplacements respectifs en fonction du modèle suivant :

- Chaque fichier téléchargé est stocké dans un dossier en fonction des deux premiers caractères du nom du fichier.
- Lorsque le chargement est lancé, l’application Commerce crée ou utilise des dossiers existants pour transférer le fichier.
- Lors du téléchargement du fichier, la variable `link_file` de chemin utilise la partie du chemin annexée à la propriété `pub/media/downloadable/files/links/` répertoire .

Par exemple, si le fichier chargé est nommé `download-example.zip`:

- Le fichier est chargé dans le chemin d’accès. `pub/media/downloadable/files/links/d/o/`.
Les sous-répertoires `/d` et `/d/o` sont créés s’ils n’existent pas.

- Le chemin d’accès final au fichier est : `/pub/media/downloadable/files/links/d/o/download-example.zip`.

- La variable `link_url` la valeur de cet exemple est `d/o/download-example.zip`

```bash
curl --location '{{your.url.here}}/rest/default/V1/products' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=571f5ebe48857569cd56bde5d65f83a2; private_content_version=9f3286b427812be6aec0b04cae33ba35' \
--data '{
  "product": {
    "sku": "sample-local-download-file",
    "name": "A downloadable product with locally hosted download file",
    "attribute_set_id": 4,
    "price": 33,
    "status": 1,
    "visibility": 4,
    "type_id": "downloadable",
    "extension_attributes": {
        "website_ids": [
            1
        ],
        "downloadable_product_links": [
            {
                "title": "an api version of already upload file",
                "sort_order": 1,
                "is_shareable": 1,
                "price": 0,
                "number_of_downloads": 0,
                "link_type": "file",
                "link_file": "/d/o/downloadable-file-demo-file-upload.zip",
                "sample_type": null
            }
        ],
        "downloadable_product_samples": []
    },
    "product_links": [],
    "options": [],
    "media_gallery_entries": [],
    "tier_prices": []
  }
}'
```

## Obtention d’un produit à l’aide de curl

```bash
curl --location '{{your.url.here}}/rest/default/V1/products/POSTMAN-download-product-1' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=b78cae2338f12d846d233d4e9486607e; private_content_version=564dde2976849891583a9a649073f01e'
```

## Mettre à jour le produit à l’aide de Postman {#update-downloadable-links}

Utilisation du point de terminaison `rest/all/V1/products/{sku}/downloadable-links`
La variable `SKU` est l’identifiant du produit qui a été généré lors de la création du produit. Par exemple, dans l’exemple de code ci-dessous, il s’agit du numéro 39, mais assurez-vous qu’il est mis à jour pour utiliser l’identifiant de votre site web. Ceci met à jour les liens pour les produits téléchargeables.

```json
{
  "link": {
    "id": 39,
    "title": "My swagger update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}
```

## Mise à jour d’un lien de téléchargement de produit à l’aide de CURL

Lorsque vous mettez à jour un lien de téléchargement de produit à l’aide de cURL, l’URL inclut le SKU du produit mis à jour.  Dans l’exemple de code suivant, le SKU est `abcd12345`. Lorsque vous envoyez la commande, modifiez la valeur pour qu’elle corresponde au SKU du produit que vous souhaitez mettre à jour.

```bash
curl --location '{{your.url.here}}/rest/all/V1/products/abcd12345/downloadable-links' \
--header 'Content-Type: application/json' \
--header 'Authorization: Bearer {{Your Bearer Token}}' \
--header 'Cookie: PHPSESSID=fa5d76f4568982adf369f758e8cb9544; private_content_version=564dde2976849891583a9a649073f01e' \
--data '{
  "link": {
    "id": {{The ID of the download link for example 39}},
    "title": "My update",
    "sort_order": 0,
    "is_shareable": 0,
    "price": 0,
    "number_of_downloads": 0,
    "link_type": "url",
    "link_file": "{{your.url.here}}/some-file.zip",
    "link_url": "{{your.url.here}}/some-file.zip",
    "link_file_content": {
      "file_data": "{{your.url.here}}/some-file.zip",
      "extension_attributes": {}
    }
  },
  "isGlobalScopeContent": true
}'
```

## Ressources supplémentaires

- [Type de produit téléchargeable](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/types/product-create-downloadable.html){target="_blank"}
- [Guide de configuration des domaines téléchargeables](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html#downloadable_domains){target="_blank"}
- [Tutoriels Adobe Developer REST](https://developer.adobe.com/commerce/webapi/rest/tutorials/prerequisite-tasks/){target="_blank"}
- [Adobe Commerce REST ReDoc](https://adobe-commerce.redoc.ly/2.4.6-admin/tag/products#operation/PostV1Products){target="_blank"}
