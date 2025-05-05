---
title: Installation de l’interface de ligne de commande Adobe I/O Runtime et du module externe Maillage d’API
description: Découvrez comment installer l’interface de ligne de commande de Adobe I/O Runtime et le module externe Maillage API
landing-page-description: Découvrez comment utiliser Adobe App Builder et installer le module externe Adobe I/O Runtime with API Mesh .
short-description: Découvrez comment utiliser Adobe App Builder et installer le module externe Adobe I/O Runtime with API Mesh .
kt: 11801
doc-type: tutorial
audience: all
last-substantial-update: 2023-2-8
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: 404d2708a6d540d6fb19a33afb20726356cd8000
workflow-type: tm+mt
source-wordcount: '186'
ht-degree: 0%

---

# Installation du module Adobe I/O Runtime CLI et Mesh

Avant de commencer à utiliser le maillage d’API pour Adobe Developer App Builder, vous devez installer l’interface de ligne de commande `aio` et le module externe de maillage d’API.
Pour obtenir des instructions sur l’installation et connaître les conditions préalables requises, consultez la page de prise en main de l’API [&#128279;](https://developer.adobe.com/graphql-mesh-gateway/gateway/getting-started/){target="_blank"}.

## Pour qui est cette vidéo ?

* Les développeurs découvrent le maillage API ou [!DNL Adobe Commerce] avec une expérience limitée utilisant [Adobe I/O Runtime](https://developer.adobe.com/runtime/docs/guides/overview/){target="_blank"} et le maillage API.

## Contenu vidéo

* Présentation du maillage d’API
* Installation de l’interface de ligne de commande Adobe I/O Runtime
* Installation du module externe de messagerie d’API

>[!VIDEO](https://video.tv.adobe.com/v/3414122?quality=12&learn=on)

## Installation de l’interface de ligne de commande et du module externe Maillage API `aio`

Après avoir installé `node` et `npm`, exécutez la commande suivante pour installer l’interface de ligne de commande `aio` :

```bash
npm install -g @adobe/aio-cli
```

Une fois l’interface de ligne de commande de Adobe I/O Runtime installée, utilisez la commande suivante pour installer le module externe Maillage d’API :

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
