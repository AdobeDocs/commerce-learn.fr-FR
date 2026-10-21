---
title: Prise en main du maillage API
description: Découvrez comment utiliser le maillage API sur Adobe Commerce et Adobe App Builder, notamment comment installer App Builder, travailler avec des projets et créer un proxy inverse.
jira: KT-11802
doc-type: Tutorial
duration: 422
last-substantial-update: 2023-08-27T00:00:00.000Z
feature: API Mesh, App Builder, Extensibility, Tools and External Services, Backend Development
topic: App Builder, I/O Events, Developer Console, Commerce, Development, Integrations
role: Developer
level: Intermediate
exl-id: baae6dab-48a4-49a0-b6f6-61cbebe63d0f
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: 72863f3c-9d27-5dda-afe1-d9f934b1fba0
    internal-label: Extensibility
  - id: b48dbafb-4193-5648-b9d9-bf96e9c9a411
    internal-label: Backend Development
  - id: c4f010fa-1478-4300-a88d-706fbc036a7a
    internal-label: APIs and SDKs
  - id: cc250cf1-34eb-4863-80d0-d170d45ea067
    internal-label: Developer tools
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: ce84ce08-883f-4337-ae83-6bb1855ca732
    internal-label: API Mesh
  - id: a743e5dc-8f37-4b5d-a848-03c32ca30598
    internal-label: App Builder
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
source-git-commit: 7516911fc14bc80f327310b99a00f9cbff7c65c7
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%
---
# Prise en main du maillage API

Si vous découvrez le maillage API pour Adobe Developer App Builder, Adobe vous recommande de commencer par ce tutoriel d’introduction, avant de passer aux autres vidéos et tutoriels.

## En quoi consiste le maillage API ?

Le maillage API combine plusieurs sources de données afin d’obtenir une réponse unique que votre application peut utiliser.

[Consultez la documentation complète sur le maillage API .](https://developer.adobe.com/graphql-mesh-gateway/mesh/){target="_blank"}

## À qui s&#39;adresse cette vidéo ?

* Tout développeur débutant dans l’utilisation du maillage API ou [!DNL Adobe Commerce] avec une expérience limitée de l’utilisation de [Adobe I/O Runtime](https://developer.adobe.com/app-builder/docs/intro_and_overview/what-is-app-builder){target="_blank"} et du maillage API.

## Contenu vidéo

* Présentation du maillage API
* Liens vers la documentation supplémentaire
* Cas pratique de vérification de l’inventaire en temps réel lors du passage en caisse
* Éloigner les efforts de développement et l’utilisation des ressources de votre application commerciale

>[!VIDEO](https://video.tv.adobe.com/v/3417534?learn=on)

## Exemples de cas d’utilisation

Votre application Commerce comporte une API REST et un point d’entrée GraphQL. Par exemple, utilisez l’API REST pour appliquer un prix spécial ou le point d’entrée GraphQL pour gérer le statut de l’inventaire. À l’aide du maillage API, vous pouvez définir les deux points d’entrée, récupérer les informations et les renvoyer à l’application qui les demande en tant que réponse unique.

## Qu’est-ce qu’un proxy inverse ?

En tant que développeur ou développeuse utilisant Adobe App Builder et le maillage API, il n’est pas nécessaire de comprendre la définition d’un proxy inverse. Cependant, si vous êtes intéressé par la fonctionnalité globale associée à Adobe App Builder, utilisez les ressources suivantes :

* [Qu’est-ce qu’un proxy inverse ?](https://www.imperva.com/learn/performance/reverse-proxy/){target="_blank"}


{{$include /help/_includes/api-mesh-related-links.md}}
