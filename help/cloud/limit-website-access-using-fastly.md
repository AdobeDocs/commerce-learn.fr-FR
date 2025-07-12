---
title: Utilisez Fastly pour refuser l’accès à un site web entier
description: Restreindre l’accès au site Adobe Commerce avec des listes de contrôle d’accès Fastly Edge et une liste de contrôle de contenu personnalisée
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 200
last-substantial-update: 2025-07-11T00:00:00Z
jira: KT-18494
source-git-commit: 810d1a17e9fe564e8450b091bbeb5574d7d76075
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 0%

---


# Utilisez Fastly pour refuser l’accès à un site web entier

Découvrez comment restreindre l’accès à votre site Adobe Commerce Cloud à l’aide des listes de contrôle d’accès Fastly Edge et de fragments de code VCL personnalisés. Ce guide détaillé vous aide à sécuriser votre environnement de prélancement en n’autorisant que des adresses IP spécifiques et en veillant à ce que votre site de développement reste privé.

## Ce que vous apprendrez

Restreindre l’accès au site Adobe Commerce avec les listes de contrôle d’accès Fastly Edge et le VCL personnalisé | Environnements de prélancement sécurisés

## À qui s&#39;adresse cette vidéo ?

* Ingénieur DevOps
* Développeur Adobe Commerce
* Ingénieur de fiabilité du site

>[!VIDEO](https://video.tv.adobe.com/v/3464781/?learn=on&enablevpops&captions=fre_fr)

## Exemple de code

Voici un exemple du VCL utilisé

```BASH
if ( !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
```

## Documentation connexe

* [Détection des adresses IP malveillantes](https://experienceleague.adobe.com/fr/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* [VCL personnalisé pour autoriser les requêtes](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [VCL personnalisé pour les requêtes de blocage](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)