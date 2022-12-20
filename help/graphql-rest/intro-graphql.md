---
title: Présentation de GraphQL
description: Découvrez comment utiliser GraphQL sur Adobe Commerce et [!DNL Magento Open Source]. Utilisation des appels GraphQL GET et POST pour Adobe Commerce et [!DNL Magento Open Source].
landing-page-description: Découvrez comment utiliser GraphQL sur Adobe Commerce et [!DNL Magento Open Source]. Utilisation des appels GraphQL GET et POST pour Adobe Commerce et [!DNL Magento Open Source].
kt: 11524
doc-type: tutorial
audience: all
last-substantial-update: 2022-12-13T00:00:00Z
source-git-commit: 9dc530107470617f88992d8eb2ed9feb017a6530
workflow-type: tm+mt
source-wordcount: '477'
ht-degree: 0%

---

# Présentation de GraphQL

GraphQL est rapidement devenu la norme du secteur dans la manière dont les applications côté client puissantes communiquent avec un serveur principal. Il s’agit d’un sujet de plus en plus pertinent pour les développeurs Adobe Commerce, car la plateforme continue d’étendre ses fonctionnalités dans le domaine des implémentations sans interface.

Si vous découvrez GraphQL, cette section vous oriente vers les concepts et l’utilisation de base.

## Qu’est-ce que GraphQL ?

GraphQL est une spécification pour un langage de requête API unique et le runtime qui fournit des données en réponse à ce langage de requête.

Les API web traditionnelles telles que REST ont été très utiles pour les systèmes disparates qui transmettent des données entre-temps, mais ont fourni des performances moins élevées pour les expériences de liaison d’applications modernes comme les Progressives Web Application. Dans des applications de ce type, les calques front-end et back-end de la variable _same_ l’application communique via l’API web. L&#39;approche régimentée des systèmes comme REST ne fournit souvent pas la flexibilité appropriée dans ce contexte, où de nombreux types de données doivent être récupérés rapidement.

GraphQL permet à un client de décrire de manière explicite _what_ les données dont il a besoin. Au lieu de nécessiter plusieurs requêtes réseau pour récupérer plusieurs types de données, une seule requête peut interroger de nombreux types. De plus, les réponses sont allégées en incluant (dans un format reflétant intuitivement la requête) uniquement les types et les champs requis.

Le runtime qui met en oeuvre la spécification GraphQL peut être créé dans n’importe quelle langue. Adobe Commerce et [!DNL Magento Open Source] utilisez la méthode
[graphql-php](https://webonyx.github.io/graphql-php/) implémentation PHP et crée ses propres couches.

[Afficher la documentation complète de GraphQL](https://graphql.org/learn)

## Utilisation d’un client GraphQL

Vous avez besoin d’un client GraphQL avec interface utilisateur graphique pour tester des exemples de code et des tutoriels. Il existe plusieurs options :

* [Altair](https://altairgraphql.dev/) est un client excellent et complet, conçu spécifiquement pour GraphQL. Adobe utilise Altair dans les vidéos de présentation.
* Si vous ne souhaitez pas installer l’application de bureau, il existe également des extensions Altair qui s’exécutent correctement dans votre
   [Chrome](https://chrome.google.com/webstore/detail/altair-graphql-client/flnheeellpciglgpaodhkhmapeljopja), Firefox ou [Edge](https://microsoftedge.microsoft.com/addons/detail/altair-graphql-client/kpggioiimijgcalmnfnalgglgooonopa) navigateur.
* [GraphiQL](https://github.com/graphql/graphiql/tree/main/packages/graphiql) est une implémentation de l’IDE GraphQL de GraphQL Foundation. Il ne s’agit pas d’un outil installable, mais plutôt d’un module que vous pouvez utiliser pour créer vous-même l’interface.
* Si vous connaissez déjà [Postman](https://www.postman.com/), il prend correctement en charge les requêtes GraphQL, bien qu’il ne soit pas aussi complet qu’un client GraphQL dédié.

Dans votre client GraphQL, vous devez envoyer vos requêtes au chemin d’URL. `/graphql` sur votre Adobe Commerce ou [!DNL Magento Open Source] instance. Si vous préférez utiliser une instance existante pour vos tests, vous pouvez utiliser la démonstration du thème Venia (exemple d’implémentation de PWA Studio) : `https://venia.magento.com/graphql`

