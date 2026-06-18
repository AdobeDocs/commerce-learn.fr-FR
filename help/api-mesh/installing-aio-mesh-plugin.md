---
title: Installation de l’interface de ligne de commande Adobe I/O Runtime et du plug-in API Mesh
description: Découvrez comment installer l’interface de ligne de commande Adobe I/O Runtime et le plug-in API Mesh pour commencer à utiliser le maillage API pour Adobe Developer App Builder.
jira: KT-11801
doc-type: Tutorial
duration: 410
last-substantial-update: 2023-02-08T00:00:00Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Beginner
exl-id: 898a0918-0362-4fa4-9204-d770ff1a7e6f
source-git-commit: c73744d503de5023e5c001d0534200522db55b04
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 0%

---

# Installation de l’interface de ligne de commande Adobe I/O Runtime et du plug-in Mesh

Avant de commencer à utiliser le maillage API pour Adobe Developer App Builder, vous devez installer l’interface de ligne de commande `aio` et le plug-in de maillage API.
Pour obtenir des instructions relatives à l’installation et connaître les conditions préalables requises, consultez la page Maillage API [Prise en main](https://developer.adobe.com/graphql-mesh-gateway/mesh/basic/){target="_blank"}.

## À qui s&#39;adresse cette vidéo ?

* Développeurs peu familiers avec le maillage API ou [!DNL Adobe Commerce] avec une expérience limitée de l’utilisation de [Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"} et du maillage API.

## Contenu vidéo

* Présentation du maillage API
* Installation de l’interface de ligne de commande Adobe I/O Runtime
* Installation du plug-in Maillage API

>[!VIDEO](https://video.tv.adobe.com/v/3419793?captions=fre_fr&learn=on)

## Installation de l’interface de ligne de commande `aio` et du plug-in API Mesh

Pour installer l’interface de ligne de commande `aio`, exécutez la commande suivante après avoir installé `node` et `npm` :

```bash
npm install -g @adobe/aio-cli
```

Une fois l’interface de ligne de commande Adobe I/O Runtime installée, utilisez la commande suivante pour installer le plug-in de maillage API :

```bash
aio plugins:install @adobe/aio-cli-plugin-api-mesh
```

{{$include /help/_includes/api-mesh-related-links.md}}
