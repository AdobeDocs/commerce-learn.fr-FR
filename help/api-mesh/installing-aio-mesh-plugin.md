---
title: Installation de l’interface de ligne de commande Adobe I/O Runtime et du module externe Maillage d’API
description: Découvrez comment installer l’interface de ligne de commande de Adobe I/O Runtime et le module externe Maillage d’API
landing-page-description: Découvrez comment utiliser Adobe App Builder et installer le module externe Adobe I/O Runtime with API Mesh .
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: a6fb3810f34246df73ae5557240eaaa0f4407eb1
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 0%

---


# Installation de l’interface de ligne de commande et du module externe Mesh de Adobe I/O Runtime

Avant de commencer à utiliser le Maillage d’API pour Adobe Developer App Builder, vous devez installer le `aio` Interface de ligne de commande et module externe Mesh de l’API.
Pour obtenir les instructions d’installation et connaître les conditions préalables requises, consultez le message de l’API [Prise en main](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/) page.

## Pour qui est cette vidéo ?

* Les développeurs qui découvrent le maillage d’API ou [!DNL Adobe Commerce] avec une expérience limitée de l’utilisation [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/) et le maillage API.

## Contenu vidéo

* Présentation du maillage API
* Installation de l’interface de ligne de commande Adobe I/O Runtime
* Installation du module externe de messagerie d’API

>[!VIDEO](https://video.tv.adobe.com/v/3414122/)

## Installation de la variable `aio` Module externe CLI et Mesh de l’API

Après installation `node` et `npm`, exécutez la commande suivante pour installer le `aio` Interface de ligne de commande :

```bash
npm install -g @adobe/aio-cli
```

Une fois l’interface de ligne de commande de Adobe I/O Runtime installée, utilisez la commande suivante pour installer le module externe Maillage d’API :

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}