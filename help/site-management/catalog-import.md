---
title: Découvrez les options d’importation de catalogue natives d’Adobe Commerce
description: Découvrez quelques-unes des options natives pour importer votre catalogue dans votre boutique Adobe Commerce.
kt: 13634
doc-type: tutorial
duration: 211
audience: all
activity: use
last-substantial-update: 2023-8-15
feature: Backend Development, Data Import/Export, REST
topic: Commerce, Administration, Content Management
role: Admin, User
level: Beginner, Intermediate
exl-id: 18713a44-df39-4b94-91ce-c7efeb4ce2b3
TQID: 'https://experienceleague.adobe.com/-JG7blrxImSXjA2DP9soZfsicISW0hkP2zJeWdMpVBU'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
    internal-label: Accounts
  - id: c18ed297-2187-4aec-affb-9d9654eca6fc
    internal-label: Catalog management
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: 601e4abe-d9bf-58de-a779-32ed6794dcbe
    internal-label: Data Import/Export
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
subfeature_v2:
  - id: e0ca0e7a-9738-48d1-b98b-615468ab4aaf
    internal-label: REST API
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
    internal-label: Administration
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%
---
# Options d’import d’un catalogue

Il existe plusieurs méthodes natives pour importer un catalogue dans Adobe Commerce. Chaque méthode a son propre raisonnement pour l’utilisation, ainsi que les avantages et les inconvénients qui doivent être pris en compte.

Choisissez l’une des options ci-dessous pour en savoir plus.

>[!BEGINTABS]

>[!TAB Manuel]

## Création manuelle des produits {#manual-import}

Si vous disposez d’un catalogue limité et que les mises à jour sont peu fréquentes, leur création manuelle peut être la meilleure option. Il faut du temps pour entrer dans chaque produit et une formation limitée sur l’utilisation de Commerce Admin. La gestion manuelle des catalogues n’est pas la bonne option pour la plupart des magasins, mais elle peut s’avérer utile dans certains cas. Pour consulter la documentation supplémentaire relative à ce processus, consultez [Création d’un produit](https://experienceleague.adobe.com/docs/commerce-admin/catalog/products/product-create.html?lang=fr){target="_blank"}. N’oubliez pas que vous pouvez utiliser plusieurs méthodes pour gérer votre catalogue. Toutefois, une fois l’automatisation utilisée, les modifications manuelles doivent être limitées. Les mises à jour automatisées ont la possibilité de remplacer toutes les modifications effectuées manuellement et de prêter à confusion. Une fois que l’intégration à Adobe Commerce pour gérer le catalogue utilise l’automatisation et les API, il est conseillé de limiter la gestion du catalogue de l’administrateur par le biais de [rôles utilisateur et autorisations](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions-user-roles.html?lang=fr){target="_blank"}.

### Quand envisager cette approche ?

* Très petit catalogue, par exemple moins de 50 produits
* Les mises à jour sont peu fréquentes
* Vous disposez de tous les détails du produit, les images, les vidéos et vous ne souhaitez pas prendre le temps d’apprendre à convertir les données au format CSV
* Vous souhaitez inclure l’ajout d’images et de vidéos lors de la création des produits
* Votre équipe connaît `not` les API et le fonctionnement d’OAUTH

>[!TAB CSV administrateur]

## Outil d’importation CSV d’administration {#admin-csv}

Cet outil permet à un propriétaire de magasin d’importer un catalogue à l’aide d’un fichier CSV directement depuis l’administrateur de Commerce.
[Importer des données depuis Commerce Admin](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html?lang=fr){target="_blank"}

Avantages :
Le chargement d’un fichier CSV à partir de l’administration est une approche simple de la gestion des catalogues. Il permet des mises à jour de produits de catalogue plus rapides pour un catalogue de taille moyenne.

Inconvénients :

* Lent
* Taille maximale du fichier de chargement définie sur le serveur et qui peut ne pas être facilement ajustée par le propriétaire du magasin.
* Nécessite un accès administrateur et une personne pour effectuer l’action. L’automatisation est limitée.
* Les imports planifiés sont limités à 1 x par jour max
* Les images et vidéos associées doivent être téléchargées séparément

### Quand envisager cette approche ?

* La taille du catalogue est modérée
* Les mises à jour ne sont pas effectuées plus d’une fois par jour
* vous avez accès aux configurations de serveur au cas où vous devriez augmenter la taille maximale de chargement de fichier
* Votre équipe connaît `not` les API et le fonctionnement d’OAUTH

>[!TAB API REST en bloc]

## API REST en bloc {#bulk-rest-api}

L’API REST en masse permet une automatisation et des mises à jour plus fréquentes. Cette API est plus rapide que le téléchargement administrateur d’un fichier CSV.
[Documentation des points d’entrée en bloc](https://developer.adobe.com/commerce/webapi/rest/use-rest/bulk-endpoints/){target="_blank"}

Avantages :
Possibilité d’importer des jeux de données volumineux qui ne sont pas au format CSV.

Inconvénients :

* Les images et vidéos associées doivent être téléchargées séparément
* Peut être limité par les contraintes de bande passante du fournisseur d’hébergement

### Quand envisager cette approche ?

* N’importe quelle taille de catalogue
* Les mises à jour sont fréquentes, plus d’une fois par jour est acceptable
* Le temps d’importation est important, mais pas critique, et un bref délai de traitement des données d’importation est acceptable
* Les données ne sont pas structurées au format CSV et vous ne pouvez pas les transformer à l’aide de l’automatisation

>[!TAB API REST ASYNC]

## API REST ASYNCHRONE {#async-rest-api}

Un point d’entrée web asynchrone intercepte les messages vers une API web et les écrit dans la file d’attente des messages. Chaque fois que le système accepte une telle requête d’API, il génère un identifiant UUID. Adobe Commerce inclut cet UUID lorsqu’il ajoute le message à la file d’attente. Ensuite, un client lit les messages de la file d’attente et les exécute un par un.
[Documentation sur les points d’entrée web asynchrones](https://developer.adobe.com/commerce/webapi/rest/use-rest/asynchronous-web-endpoints/){target="_blank"}

Avantages :

* Rapide pour importer les données
* La portée du magasin est prise en charge ou vous pouvez spécifier `all` effectuer une opération sur tous les magasins existants

Inconvénients :

* Les requêtes GET ne sont pas prises en charge

### Quand envisager cette approche ?

* Les importations sont fréquentes
* Aucun problème avec un léger retard à partir du moment où ils sont envoyés via l&#39;API puis traités à partir de la file d&#39;attente des messages.


>[!TAB API REST CSV]

## API REST CSV {#csv-rest-api}

Cette option d’API permet des importations extrêmement rapides par rapport à toutes les autres options natives.

[Api REST CSV d’import de données](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
Avantages :

* Méthode la plus rapide pour traiter les données entrantes
* Peut être effectué plusieurs fois par jour
* Les données peuvent être compressées à l’aide de gzip pour les requêtes volumineuses afin d’éviter les limites de taille des requêtes HTTP.

Inconvénients :

* Les images et vidéos associées doivent être téléchargées séparément
* Les données doivent être au format CSV

### Quand envisager cette approche ?

* N’importe quelle taille de catalogue
* Les mises à jour sont fréquentes, plus d’une fois par jour est acceptable
* Le temps global d’importation est important
* Les données sont déjà au format CSV ou peuvent être facilement transformées à l’aide de l’automatisation

>[!ENDTABS]

## Ressources supplémentaires

* [Importer des données à l’aide du nouveau fichier CSV REST](https://developer.adobe.com/commerce/webapi/rest/modules/import/){target="_blank"}
* [Documentation principale sur l’importation de données](https://experienceleague.adobe.com/docs/commerce-admin/systems/data-transfer/import/data-import.html?lang=fr){target="_blank"}
* [Notes de mise à jour de la version 2.4.6 d’Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/release/notes/adobe-commerce/2-4-6.html?lang=fr){target="_blank"}
* [Utilisateurs, rôles et autorisations](../site-management/users-roles-permissions.md){target="_blank"}
