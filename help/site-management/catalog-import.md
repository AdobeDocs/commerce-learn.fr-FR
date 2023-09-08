---
title: Découvrez les options d’importation de catalogue natives avec Adobe Commerce
description: Découvrez comment quelques-unes des options natives pour importer votre catalogue dans votre boutique Adobe Commerce.
kt: 13634
doc-type: tutorial
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
source-git-commit: 46855a18faf94c0d7347663ba6f83f423ef79c3c
workflow-type: tm+mt
source-wordcount: '825'
ht-degree: 0%

---

# Options d’importation d’un catalogue

Il existe quelques méthodes natives pour importer un catalogue dans Adobe Commerce. Chaque méthode a son propre raisonnement pour l’utilisation ainsi que les avantages et inconvénients qui doivent être pris en compte.

Pour en savoir plus, sélectionnez l’une des options ci-dessous.

>[!BEGINTABS]

>[!TAB Manuel]

## Création manuelle des produits {#manual-import}

Si vous disposez d’un catalogue limité et que les mises à jour sont peu fréquentes, la création manuelle de ces derniers peut être la meilleure option. Il faut du temps pour entrer dans chaque produit et une formation limitée sur l’utilisation de l’administrateur Commerce. La gestion manuelle du catalogue n’est pas la bonne option pour la plupart des magasins, mais dans certaines situations, elle peut s’avérer logique. Pour consulter la documentation supplémentaire de ce processus, voir [Création d’un produit](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html){target="_blank"}. Do not forget, you can use more than one method to manage your catalog, however once automation is used, manual edits must be limited. Automated updates have the opportunity to overwrite any changes performed manually, and therefore cause confusion. Once the integration with Adobe Commerce to manage the catalog is using automation and APIs, it is advised to restrict management of the catalog from the admin through [user roles and permissions](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html){target="_blank"}.



### Quand envisager cette approche ?

- Un très petit catalogue, par exemple moins de 50 produits
- Les mises à jour sont peu fréquentes
- Vous disposez de tous les détails, images et vidéos du produit, et vous ne souhaitez pas prendre le temps d’apprendre à convertir les données au format CSV.
- Vous souhaitez inclure l’ajout d’images et de vidéos lors de la création des produits.
- Votre équipe est `not` connaissance des API et du fonctionnement d’OAUTH



>[!TAB Fichier CSV d’administration]

## Outil d’importation CSV d’administration {#admin-csv}

Cet outil permet à un propriétaire de magasin d’importer un catalogue à l’aide d’un droit CSV provenant de l’administrateur de commerce.
[Importation de données depuis l’administrateur Commerce](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}

Avantages : le transfert d’un fichier CSV depuis l’administrateur est une méthode simple de gestion des catalogues. Il permet des mises à jour plus rapides des produits du catalogue pour un catalogue de taille modérée.

Inconvénients

- Lent
- Taille maximale du fichier de téléchargement définie sur le serveur et qui ne peut pas être facilement ajustée par le propriétaire d’un magasin.
- Nécessite un accès administrateur et une personne pour effectuer l’action ; l’automatisation est limitée.
- Les importations de planification sont limitées à 1 fois par jour.
- Les images et vidéos associées doivent être téléchargées séparément.



### Quand envisager cette approche ?

- La taille du catalogue est modérée
- Les mises à jour ne sont pas plus d’une fois par jour
- vous disposez d’un accès aux configurations de serveur au cas où vous deviez augmenter la taille de chargement de fichier maximale.
- Votre équipe est `not` connaissance des API et du fonctionnement d’OAUTH



>[!TAB API REST en bloc]

## API REST en bloc {#bulk-rest-api}

L’API REST en bloc permet l’automatisation et des mises à jour plus fréquentes. Cette API est plus rapide que l’utilisation du transfert d’administrateur du fichier CSV.
[Documentation sur les points de fin en bloc](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

Avantages : la possibilité d’importer des jeux de données volumineux qui ne sont pas au format CSV.

Inconvénients

- Les images et vidéos associées doivent être téléchargées séparément.
- Peut être limité par des contraintes de bande passante sur le fournisseur d’hébergement
- Vous devez utiliser des identifiants d’attribut d’option et non les étiquettes.



### Quand envisager cette approche ?

- Le catalogue est de n’importe quelle taille
- Les mises à jour sont fréquentes, plus de 1 fois par jour est acceptable.
- Le temps d&#39;importation est important mais pas
- Les données ne sont pas structurées au format CSV et vous ne pouvez pas les transformer à l’aide de l’automatisation.



>[!TAB API REST ASYNC]

## API REST ASYNC {#async-rest-api}

Un point de terminaison web asynchrone intercepte les messages vers une API web et les écrit dans la file d’attente des messages. Chaque fois que le système accepte une telle demande d’API, il génère un identifiant UUID. Adobe Commerce inclut cet UUID lorsqu’il ajoute le message à la file d’attente. Ensuite, un consommateur lit les messages de la file d&#39;attente et les exécute un par un.
[Documentation sur les points de terminaison web asynchrones](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

Avantages :

- Importation rapide des données
- La portée du magasin est prise en charge ou vous pouvez spécifier `all` pour effectuer une opération sur tous les magasins existants

Inconvénients

- La demande de GET n’est pas prise en charge
- Vous devez utiliser les identifiants d’attribut d’option au lieu des libellés.


### Quand envisager cette approche ?

- Les imports sont fréquents
- Aucun problème lié à un léger délai entre l’envoi via l’API et le traitement depuis la file d’attente des messages.



>[!TAB API REST CSV]

## API REST CSV {#csv-rest-api}

Cette option d’API permet des importations extrêmement rapides par rapport à toutes les autres options natives.

[Importation des données de l’api CSV REST](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
Avantages :

- Méthode la plus rapide pour traiter les données entrantes
- Peuvent être exécutées plusieurs fois par jour
- Les données peuvent être compressées à l’aide de gzip pour les requêtes volumineuses afin d’éviter les limites de taille de requête HTTP.

Inconvénients

- Les images et vidéos associées doivent être téléchargées séparément.
- Vous devez utiliser les identifiants d’attribut d’option et non les étiquettes.
- Les données doivent être au format CSV

### Quand envisager cette approche ?

- Le catalogue est de n’importe quelle taille
- Les mises à jour sont fréquentes, plus de 1 fois par jour est acceptable.
- Le temps global d’importation est important
- Les données sont déjà au format CSV ou peuvent facilement être transformées à l’aide de l’automatisation.



>[!ENDTABS]

## Ressources supplémentaires

- [Importation de données à l’aide d’un nouveau fichier CSV REST](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
- [Importation de la documentation principale de données](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html){target="_blank"}
- [Notes de mise à jour d’Adobe Commerce version 2.4.6](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html){target="_blank"}
- [Utilisateurs, rôles et autorisations](../site-management/users-roles-permissions.md){target="_blank"}
