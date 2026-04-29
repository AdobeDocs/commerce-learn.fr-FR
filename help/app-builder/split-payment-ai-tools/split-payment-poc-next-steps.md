---
title: 'Split payment POC: next steps after the proof of concept'
description: 'Learn how to move the split payment POC toward production: Experience Cloud UI, ERP hooks, API Mesh, PHP scope, App Builder workflows, and deploy checklist.'
feature: App Builder, API Mesh, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 269
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 1e2c7e0e6d0f2d174b88406ce3fb7c787676ecee
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 0%

---

# Split payment POC: next steps after the proof of concept

The demo dashboard and simulation script you built in this tutorial are intentionally rough. They exist to prove the pattern. This page describes a practical path from proof of concept to production-style App Builder development.


## Step 1 — Replace the Demo Dashboard with an Experience Cloud UI Extension

The `demo-dashboard` web action serves HTML from a string inside a Node.js function. It works, but it is not the production pattern.

The proper replacement is the `commerce-backend-ui-1` extension in the `commerce-checkout-starter-kit` — a React application embedded in the Commerce Admin Shell via the Adobe Admin UI SDK. See [Split payment POC: Experience Cloud UI extension AI prompt](split-payment-poc-experience-cloud-ui-prompt.md) for the generation prompt.

**What changes:**
* Operators log in through Commerce Admin Shell (IMS authentication instead of a shared secret)
* The order list uses the IMS token context — no need for a demo secret
* Accept/decline actions are scoped to the operator&#39;s IMS identity
* The UI is embedded in Commerce Admin at the URL Commerce administrators already know

**What stays the same:**
* `payment-accept` and `payment-decline` App Builder actions are unchanged
* Commerce REST endpoints are unchanged
* The PHP module is unchanged


## Step 2 — Connect a Real ERP

The accept/decline flow in this PoC is driven by a human clicking a button. In production, this is driven by your ERP, CRM, or payment processor.

**The integration pattern:**
1. Your ERP system captures cash and calls `POST /payment-accept` (the App Builder web action URL) with `{ orderId: <entity_id> }`
2. App Builder valide l’appel (jeton du porteur ou clé API — ajout du middleware d’authentification au `payment-accept`)
3. App Builder appelle le `cash-received` REST Commerce
4. Commerce facture et expédie la commande

Aucune modification PHP n&#39;est requise. La surface REST est déjà là. L’action App Builder a simplement besoin d’un appelant sécurisé au lieu d’un bouton de tableau de bord.

**Options d&#39;authentification pour l&#39;appelant ERP :**
* Adobe I/O Runtime prend en charge la `require-adobe-auth: true` des jetons IMS (si votre ERP peut obtenir un jeton IMS)
* Pour les systèmes non Adobe : ajout d’une vérification simple de la clé API dans l’action `payment-accept` (vérification d’un en-tête par rapport à un secret stocké dans l’environnement de l’action)


## Étape 3 — Ajouter un maillage API en tant que couche de courtier

Actuellement, App Builder appelle Commerce REST directement avec les informations d’identification OAuth 1.0a. Pour les déploiements plus importants, le maillage API d’Adobe fournit une couche de passerelle gérée entre App Builder et Commerce.

**Avantages :**
* Gestion centralisée des informations d’identification : le maillage API contient les informations d’identification Commerce ; App Builder appelle le maillage API
* Transformation des requêtes : mappez les payloads App Builder aux formes REST Commerce sans modifier l’action App Builder.
* Limitation de débit et mise en cache : protection de Commerce contre le trafic d’événements à volume élevé

**Le modèle :**
* Remplacez les appels `createCommerceClient(params, logger)` par des appels à votre point d’entrée du maillage API
* Le maillage API gère la signature OAuth vers Commerce.


## Étape 4 — Réduire l&#39;empreinte PHP

Le module PHP actuel gère cinq éléments qui doivent rester en cours de traitement (voir [Split Payment POC : architecture and design decisions](split-payment-poc-architecture-and-decisions.md)). À mesure que la surface de l’API Adobe Commerce se développe, certaines d’entre elles peuvent devenir mobiles :

**Éventuellement déplaçable à l’avenir :**
* L’API REST de crédit de magasin est en évolution. Les versions futures pourront prendre en charge l’application de crédit après commande ou aux paniers inactifs.
* À mesure que davantage d’opérations Commerce deviennent asynchrones, le dispositif de protection du seuil peut être déplacé vers un résolveur de maillage API en précommande

**Non déplaçable (contraintes architecturales) :**
* La manipulation des guillemets avant `placeOrder()` nécessite toujours du code en cours de traitement, sauf si Commerce expose un hook épuré via un point d’extension API-first
* Les points d’entrée REST (`/V1/split-payment/*`) sont spécifiques à cette fonctionnalité ; ils se trouvent dans Commerce car ils appellent les services internes Commerce


## Étape 5 : ajouter d’autres workflows à App Builder

La preuve de concept actuelle effectue une opération : écoute le placement de la commande, puis active accepter/refuser. Extensions naturelles :

**Score de fraude avant acceptation :**
En `payment-orchestrator`, après avoir enregistré les espèces en attente, appelez une API de notation de fraude avant que le résultat de l’orchestration ne soit considéré comme final. Si le score est supérieur à un seuil, décline automatiquement au lieu d’attendre l’action de l’opérateur.

**E-mails de notification :**
Lorsque `payment-accept` réussit, déclenchez un e-mail (via Adobe Campaign, SendGrid ou toute API HTTPS) informant le client que son paiement en espèces a été confirmé.

**Récompenses de points de fidélité :**
Une fois l’argent confirmé, appelez une API de fidélité pour attribuer des points. C&#39;est de l&#39;App Builder pure — aucun PHP requis.

**Gestion des délais d’expiration :**
Ajoutez une action App Builder planifiée (à l’aide de `cron` dans `app.config.yaml`) pour rechercher les commandes de `split_cash_status = 'pending'` de plus de X jours et les refuser automatiquement.


## Étape 6 - Déploiement en production

La preuve de concept est configurée pour l’espace de travail `Stage`. Passage en exploitation :

```bash
# Switch to production workspace
aio app use
# Select Production workspace

# Update .env for production
# (same credentials, but production Commerce base URL)

# Deploy
aio app deploy
```

**Liste de contrôle pour la préparation à la production :**
* [ ] `DEMO_UI_SECRET` défini (ou tableau de bord de démonstration remplacé par l’interface utilisateur Experience Cloud)
* [ ] `LOG_LEVEL=warn` ou `error` en production (non `debug`)
* [ ] `PAYMENT_THRESHOLD` correspond à la configuration de production de Commerce
* [ ] informations d’identification de l’intégration Commerce dans `.env` concernent une intégration de production dédiée (et non l’évaluation)
* [ ] Fastly IP, place sur la liste autorisée mise à jour avec les adresses IP de sortie de production App Builder (Commerce Cloud)
* [ Enregistrement des événements d’E/S ] confirmé dans l’espace de travail de production


## Diagramme d’évolution de l’architecture

```
PoC (this tutorial)
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  └─────────────┘         │ demo-dashboard   │   │
└─────────────────────────────────────────────────┘

Phase 2: Experience Cloud UI
┌─────────────────────────────────────────────────┐
│  Commerce                 App Builder            │
│  ┌─────────────┐         ┌──────────────────┐   │
│  │  PHP Module │◄────────│ payment-orchestr │   │
│  │  REST API   │────────►│ payment-accept   │   │
│  │  Checkout UI│         │ payment-decline  │   │
│  │             │         │ Admin UI ext.    │   │
│  └─────────────┘         └──────────────────┘   │
└─────────────────────────────────────────────────┘

Phase 3: Real ERP + API Mesh
┌──────────────────────────────────────────────────────────┐
│  ERP  ──►  App Builder  ──►  API Mesh  ──►  Commerce     │
│            (orchestrator)   (gateway)    (PHP module      │
│            (accept)                      REST surface)    │
└──────────────────────────────────────────────────────────┘
```


## Références clés

* [Documentation Adobe App Builder](https://developer.adobe.com/app-builder/docs/overview/){target="_blank"}
* [Adobe I/O Events pour Commerce](https://developer.adobe.com/commerce/extensibility/events/){target="_blank"}
* [Kit de démarrage pour le passage en caisse Commerce](https://github.com/adobe/commerce-checkout-starter-kit){target="_blank"}
* [SDK de l’interface utilisateur d’administration d’Adobe](https://developer.adobe.com/commerce/extensibility/admin-ui-sdk/){target="_blank"}
* [Maillage API Adobe](https://developer.adobe.com/graphql-mesh-gateway/){target="_blank"}
* [Adobe I/O Runtime (OpenWhisk)](https://developer.adobe.com/runtime/docs/){target="_blank"}



{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
