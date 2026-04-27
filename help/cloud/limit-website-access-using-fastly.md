---
title: Utilisez Fastly pour refuser l’accès à un site web entier
description: Restreindre l’accès au site Adobe Commerce avec des listes de contrôle d’accès Fastly Edge et une liste de contrôle de contenu personnalisée
feature: Cloud, Configuration, Site Management, System
topic: Administration,Commerce,Development, Security
role: Admin, Developer, User
level: Intermediate, Experienced
doc-type: Technical Video
duration: 231
last-substantial-update: 2025-07-11T00:00:00.000Z
jira: KT-18494
exl-id: 121e7a2f-f9fd-4cd1-b2be-48a12b538008
TQID: https://experienceleague.adobe.com/OQlPdH-rAoSoPK1-zemp5OODX6pKWV-dW1C88KVRE6I
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: b5f00040-57a0-4a6d-a39e-383b1936c2c9id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080bid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 175
ht-degree: 0%

---

# Utilisez Fastly pour refuser l’accès à un site web entier

Découvrez comment restreindre l’accès à votre site Adobe Commerce Cloud à l’aide des listes de contrôle d’accès Fastly Edge et de fragments de code VCL personnalisés. Ce guide détaillé vous aide à sécuriser votre environnement de prélancement en n’autorisant que des adresses IP spécifiques et en veillant à ce que votre site de développement reste privé.

## Ce que vous apprendrez

Restreindre l’accès au site Adobe Commerce avec les listes de contrôle d’accès Fastly Edge et les listes de contrôle de contenu virtuelles personnalisées | Environnements de prélancement sécurisés

## À qui s&#39;adresse cette vidéo ?

* Ingénieur DevOps
* Développeur Adobe Commerce
* Ingénieur de fiabilité du site

>[!VIDEO](https://video.tv.adobe.com/v/3464779?learn=on)

## Exemple de code

Voici un exemple du VCL utilisé

```BASH
if ( !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
```

## Documentation connexe

* [Détection des adresses IP malveillantes](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/tools/new-relic/malicious-ip)
* [VCL personnalisé pour autoriser les requêtes](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist)
* [VCL personnalisé pour les requêtes de blocage](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/cdn/custom-vcl-snippets/fastly-vcl-blocking)
