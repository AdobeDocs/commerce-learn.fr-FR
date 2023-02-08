---
title: Installation de l’interface de ligne de commande d’Adobe Developer IO et du module externe Maillage d’API
description: Découvrez comment installer l’interface de ligne de commande d’Adobe Developer IO et le module externe de maillage d’API
landing-page-description: Découvrez comment utiliser Adobe App Builder et installer Adobe Developer IO avec le module externe Maillage d’API.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: b6d501c5c852e1cc2cf1f05f91b5a9d96ac7d036
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Installation du module externe Adobe Developer IO et Mesh

Avant de commencer, il y a quelques éléments à configurer. Tout d’abord, la configuration de l’interface de ligne de commande d’Adobe Developer IO. Ensuite, assurez-vous que le module externe de maillage d’API est configuré dans chaque environnement.
Pour obtenir des instructions sur la configuration de votre environnement local pour exécuter Node, nvm et l’installation d’Adobe Developer IO, veillez à consulter [Prise en main de GraphQL Mesh](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/).

## Pour qui est cette vidéo ?

* Développeurs nouveaux pour Adobe App Builder ou [!DNL Magento Open Source] avec une expérience limitée d’Adobe Developer IO et d’API Mesh.

## Contenu vidéo

* Présentation du maillage API
* Installation de l’interface de ligne de commande d’Adobe Developer IO
* Ajout du module externe Maillage API à la ligne de commande AIO

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## Exemples de commandes utilisant NPM et AIO

L’installation de l’interface de ligne de commande d’Adobe Developer est relativement simple. Après avoir installé Node, exécutez cette commande `npm install -g @adobe/aio-cli`
Une fois l’interface de ligne de commande Adobe Developer installée, il est possible d’installer le module externe mesh. Pour ce faire, exécutez cette commande `aio plugins:install @adobe/aio-cli-plugin-api-mesh`

{{$include /help/_includes/api-mesh-related-links.md}}
