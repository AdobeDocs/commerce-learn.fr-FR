---
title: Installation de l’interface de ligne de commande Adobe I/O Runtime et du module externe Maillage d’API
description: Découvrez comment installer l’interface de ligne de commande de Adobe I/O Runtime et le module externe Maillage d’API
landing-page-description: Découvrez comment utiliser Adobe App Builder et installer le module externe Adobe I/O Runtime with API Mesh .
short-description: Discover how to use Adobe App Builder and install the Adobe I/O Runtime with API Mesh plugin.
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
source-git-commit: d85426bcf3ae0412a433414d70c874964905dda0
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---


# Installation de l’interface de ligne de commande et du module externe Mesh de Adobe I/O Runtime

Avant de commencer à utiliser le Maillage d’API pour Adobe Developer App Builder, vous devez installer le `aio` Interface de ligne de commande et module externe Mesh de l’API.
Pour obtenir les instructions d’installation et connaître les conditions préalables requises, consultez le message de l’API [Prise en main](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"} page.

## Pour qui est cette vidéo ?

* Les développeurs qui découvrent le maillage d’API ou [!DNL Adobe Commerce] avec une expérience limitée de l’utilisation [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} et le maillage API.

## Contenu vidéo

* Présentation du maillage API
* Installation de l’interface de ligne de commande Adobe I/O Runtime
* Installation du module externe de messagerie d’API

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

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
