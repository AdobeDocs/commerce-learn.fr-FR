---
title: Référence des variables d’environnement de paiement fractionné PDC
description: Découvrez comment mapper Commerce OAuth, l’URL de base, le seuil de paiement et les paramètres de démonstration facultatifs à l’orchestrateur, à l’extension d’interface utilisateur et aux fichiers d’environnement de simulation.
feature: App Builder, Configuration, Extensibility, Paas, REST, Security
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 115
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '243'
ht-degree: 0%

---

# PDC de paiement fractionné : référence des variables d’environnement

Les quatre mêmes informations d’identification OAuth Commerce sont utilisées dans chaque composant. Dans **[!UICONTROL Commerce Admin]**, créez une **[!UICONTROL Integration]**, puis réutilisez les quatre valeurs dans chaque fichier `.env` ci-dessous. (Pour connaître les étapes d’activation, voir [Validation de principe du paiement partagé : conditions préalables et configuration de l’environnement](./prerequisites-and-setup.md).)

## Les quatre informations d’identification OAuth (utilisées partout)

| Variable | Où l&#39;obtenir |
| --- | --- |
| `COMMERCE_CONSUMER_KEY` | **[!UICONTROL Commerce Admin]** > **[!UICONTROL System]** > **[!UICONTROL Integrations]** > *[votre intégration]* |
| `COMMERCE_CONSUMER_SECRET` | Comme ci-dessus : les valeurs ne sont affichées qu’au moment de l’activation |
| `COMMERCE_ACCESS_TOKEN` | Comme ci-dessus |
| `COMMERCE_ACCESS_TOKEN_SECRET` | Comme ci-dessus |


## Orchestrateur App Builder

### `split-payment-orchestrator/.env`

Copiez depuis `.env.example` dans le répertoire de l’orchestrateur. Ne pas valider ce fichier.

```dotenv
# Commerce REST base URL — no trailing slash
COMMERCE_BASE_URL=https://your-store.example.com

# OAuth 1.0a integration credentials
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=

# Must match split_payment/general/threshold in Commerce config (default: 100)
# Both Commerce and App Builder fall back to 100 if this is missing, non-numeric, or ≤ 0
PAYMENT_THRESHOLD=100

LOG_LEVEL=info

# Demo dashboard: if set, requires ?secret=<value> in URL or x-demo-secret header
# Leave empty for private staging only (anyone with the URL can list/accept orders)
DEMO_UI_SECRET=

# Optional: override the base URL used in dashboard action links (useful behind proxies)
DEMO_UI_BASE_URL=
```


## Extension de l’interface utilisateur d’Experience Cloud (commerce-checkout-starter-kit)

### `commerce-checkout-starter-kit/.env`

Ce composant utilise deux ensembles d’informations d’identification : IMS pour la liste de commandes avec le SDK de l’interface utilisateur **[!UICONTROL Admin]** et OAuth 1.0a pour les actions d’acceptation et de refus.

```dotenv
# IMS — used by CustomMenu/commerce-rest-api to list orders
# The Admin UI SDK provides the IMS token context; these set the Commerce base URL
COMMERCE_BASE_URL=https://your-store.example.com
OAUTH_CLIENT_ID=
OAUTH_CLIENT_SECRETS=
OAUTH_TECHNICAL_ACCOUNT_ID=
OAUTH_TECHNICAL_ACCOUNT_EMAIL=
OAUTH_SCOPES=
OAUTH_IMS_ORG_ID=
AIO_CLI_ENV=stage

# OAuth 1.0a — same four credentials, COMMERCE_INTEGRATION_ prefix
COMMERCE_INTEGRATION_BASE_URL=https://your-store.example.com
COMMERCE_INTEGRATION_CONSUMER_KEY=
COMMERCE_INTEGRATION_CONSUMER_SECRET=
COMMERCE_INTEGRATION_ACCESS_TOKEN=
COMMERCE_INTEGRATION_ACCESS_TOKEN_SECRET=
```


## Script de simulation

### `commerce-backend-ui-1/.env.simulation`

Copiez depuis `.env.simulation.example` dans le même répertoire.

```dotenv
COMMERCE_BASE_URL=https://your-store.example.com
COMMERCE_CONSUMER_KEY=
COMMERCE_CONSUMER_SECRET=
COMMERCE_ACCESS_TOKEN=
COMMERCE_ACCESS_TOKEN_SECRET=
```


## Remarques

**`PAYMENT_THRESHOLD`** — Doit correspondre à `split_payment/general/threshold` dans **[!UICONTROL Commerce]** configuration du système. Les deux côtés prennent par défaut la valeur `100` si la valeur est manquante, n’est pas numérique ou est inférieure ou égale à `0`. Si vous modifiez le seuil dans **[!UICONTROL Commerce]**, mettez à jour le `.env` App Builder pour qu’il corresponde.

**`DEMO_UI_SECRET`** — Facultatif mais recommandé pour tout déploiement qui n&#39;est pas localhost. Toute personne disposant de l’URL du tableau de bord peut répertorier les commandes et exécuter les commandes accepter et refuser si ce champ est vide. Pour un véritable environnement d’évaluation, définissez un secret partagé.

**`COMMERCE_BASE_URL`** — N&#39;incluez jamais de barre oblique. Le client REST Commerce ajoute automatiquement des `/rest/V1/`.

**`AIO_CLI_ENV`** — Définissez sur `stage` pour l&#39;espace de travail **[!UICONTROL Stage]**. Remplacez par `prod` lors du déploiement sur **[!UICONTROL Production]**.


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
