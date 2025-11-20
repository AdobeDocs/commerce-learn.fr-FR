---
title: Présentation de GraphQL
description: Découvrez comment utiliser GraphQL sur Adobe Commerce et  [!DNL Magento Open Source]. Utilisez le GET GraphQL et les appels POST pour Adobe Commerce et  [!DNL Magento Open Source].
short-description: Découvrez comment utiliser GraphQL GET et les appels POST pour Adobe Commerce et  [!DNL Magento Open Source].
kt: 11524
doc-type: video
audience: all
last-substantial-update: 2023-10-12T00:00:00Z
feature: GraphQL
topic: Commerce, Architecture, Headless
role: Architect, Developer
level: Beginner, Intermediate
exl-id: 8ea823da-24a3-4627-885c-4b3279b9142c
source-git-commit: b8b1e40a2f4d38954f0d21bc6f1a91b7ec0bd8c9
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# Présentation de GraphQL

Il s’agit de la première partie de la série pour GraphQL et Adobe Commerce. GraphQL est rapidement devenu la norme du secteur en ce qui concerne la manière dont les applications côté client puissantes communiquent avec un serveur principal. Il s’agit d’un sujet de plus en plus pertinent pour les développeurs Adobe Commerce, car la plateforme continue à étendre ses fonctionnalités dans le domaine des implémentations découplées.

Si GraphQL est nouveau pour vous, cette section vous guide vers les concepts de base et son utilisation.

>[!VIDEO](https://video.tv.adobe.com/v/3443942?captions=fre_fr&learn=on)

## Vidéos et tutoriels connexes sur GraphQL dans cette série

* [Partie 2 - GraphQL - Requêtes](../graphql-rest/graphql-queries.md)
* [Partie 3 - GraphQL - Mutations](../graphql-rest/graphql-mutations.md)
* [Partie 4 - GraphQL - Schéma](../graphql-rest/graphql-schema.md)

## Qu’est-ce que GraphQL ?

GraphQL est une spécification pour un langage de requête d’API unique et le runtime qui fournit des données en réponse à ce langage de requête.

Les API web traditionnelles telles que REST ont bien servi les systèmes disparates qui transmettent des données entre eux, mais n’ont pas fourni de performances de pointe pour les expériences de lien d’application modernes telles que les applications web progressives. Dans des applications comme celle-ci, les calques front-end et back-end d’une _même_ application communiquent via l’API web. L’approche structurée de schémas tels que REST n’offre souvent pas la flexibilité appropriée dans ce contexte, où de nombreux types de données doivent être récupérées rapidement.

GraphQL permet à un client de décrire de manière expresse _exactement_ les données dont il a besoin. Au lieu d’avoir besoin de plusieurs requêtes réseau pour récupérer plusieurs types de données, une seule requête peut rechercher plusieurs types. De plus, les réponses sont optimisées en incluant (dans un format reflétant intuitivement la requête) uniquement les types et champs demandés.

Le runtime qui implémente la spécification GraphQL peut être créé dans n’importe quelle langue. Adobe Commerce et [!DNL Magento Open Source] utilisent
Implémentation PHP [graphql-php](https://webonyx.github.io/graphql-php/){target="_blank"} et construit ses propres calques dessus.

[Consultez la documentation complète de GraphQL](https://graphql.org/learn){target="_blank"}

## Utilisation d’un client GraphQL

Vous avez besoin d’un client GraphQL avec interface utilisateur graphique pour tester des exemples de code et des tutoriels. Il existe plusieurs options :

* [Altair](https://altairgraphql.dev/){target="_blank"} est un excellent client entièrement équipé spécialement conçu pour GraphQL. Adobe utilise Altair dans les vidéos de présentation.
* Si vous ne souhaitez pas installer l’application de bureau, il existe également des extensions Altair qui s’exécutent directement dans votre ordinateur
  Navigateur [Chrome](https://chromewebstore.google.com/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja){target="_blank"}, Firefox ou [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa){target="_blank"}.
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql){target="_blank"} est une implémentation de l’IDE GraphQL de GraphQL Foundation. Il ne s’agit pas d’un outil installable, mais plutôt d’un package que vous pouvez utiliser pour créer l’interface vous-même.
* Si vous connaissez déjà [Postman](https://www.postman.com/){target="_blank"}, il offre une prise en charge correcte des requêtes GraphQL, bien qu’il ne soit pas aussi complet qu’un client GraphQL dédié.

Dans votre client GraphQL, vous devez envoyer vos requêtes au chemin d’URL `/graphql` sur votre instance Adobe Commerce ou [!DNL Magento Open Source]. Si vous préférez utiliser une instance existante pour vos tests, vous pouvez utiliser la démo du thème Venia (l’exemple d’implémentation de PWA Studio) : `https://venia.magento.com/graphql`

{{$include /help/_includes/graphql-rest-related-links.md}}
