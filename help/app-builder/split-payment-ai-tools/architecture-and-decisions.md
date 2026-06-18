---
title: Paiement fractionné POC — décisions d'architecture et de conception
description: Découvrez comment la PDC de paiement partagé mappe le passage en caisse de synchronisation à Commerce et les étapes pilotées par les E/S à App Builder, avec des attributs d’extension, REST et cinq cas de périphérie de plug-in.
feature: App Builder, Eventing, Extensibility, Paas, Payments, REST
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 293
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '863'
ht-degree: 1%

---

# Paiement fractionné POC : décisions d&#39;architecture et de conception

Cette page explique les choix architecturaux qui sous-tendent la preuve de concept du paiement partagé. Lisez-le avant d’utiliser les invites de build de cette série afin de comprendre la structure de chaque composant et d’adapter les modèles dans votre propre projet.

## Le principe de base

La preuve de concept ne concerne pas la mise en œuvre la plus élégante du paiement fractionné. Il s’agit de montrer **comment commencer à déplacer la logique de Commerce vers App Builder sans une réécriture en masse**.

La règle appliquée dans est la suivante :

> **Si quelque chose doit s&#39;exécuter de manière synchrone dans le cycle de requête de Commerce, ou doit appeler des API internes à Commerce qui n&#39;ont pas de surface externe propre, il reste en PHP. Toutes les autres tâches sont transférées vers App Builder.**

## Qu’est-ce qui vit dans Commerce (PHP) et pourquoi ?

### &#x200B;1. Demande de crédit de magasin : `PlaceOrderPlugin`

Le crédit de la boutique est appliqué au panier à l’aide de `Magento\CustomerBalance\Api\BalanceManagementInterface::apply()`. Cette méthode fonctionne uniquement sur un panier **actif**. Le panier devient inactif au moment où la commande est passée. App Builder reçoit l’événement d’E/S *une fois la commande passée* il n’est donc pas possible d’appliquer le crédit de magasin d’App Builder.

**La leçon :** tout ce qui doit changer d’état du panier avant le placement de la commande doit s’exécuter dans Commerce. Il n’y a pas de solution.

### &#x200B;2. Garde de seuil synchrone : `CheckoutPlugin`

Le contrôle du seuil de commande de 100 $ doit bloquer le client à l’étape de paiement, avant qu’il ne sélectionne **[!UICONTROL Place Order]**. La réponse doit être synchrone dans le cycle de requête Commerce. App Builder étant piloté par les événements et asynchrone, il ne peut pas renvoyer d’erreur immédiate à ce moment-là.

App Builder *valide également* le seuil (en tant qu’audit), mais l’expérience client dépend de la vérification Commerce exécutée en premier.

### &#x200B;3. Points d’entrée REST personnalisés : `webapi.xml` et `SplitPaymentManagement`

Les points d’entrée suivants doivent :

* Appeler `SplitInvoiceService` (factures qui utilisent le service de facture interne Commerce)
* Appeler `ShipOrder::execute()` (service d’expédition interne de Commerce)
* Mise à jour de l’état et du statut des commandes avec la machine d’état des commandes Commerce

`/V1/split-payment/orders/:id/cash-received` et `/V1/split-payment/orders/:id/cash-decline`

Il n’existe pas de couche REST publique propre pour ce comportement, de sorte que Commerce expose les points d’entrée . App Builder les appelle.

### &#x200B;4. Fractionner les montants sur devis et commande : observateurs et plugins

Commerce a besoin des montants fractionnés (`split_store_credit_amount`, `split_cash_amount`, `split_cash_status`) sur la commande, à la fois pour les réponses REST lues par App Builder et pour la vue de commande **[!UICONTROL Admin]**. Les montants sont joints avec des attributs d&#39;extension et sont copiés du devis vers la commande dans un observateur sur `sales_model_service_quote_submit_before`.

## Qu’est-ce qui vit dans App Builder et pourquoi ?

### &#x200B;1. Traitement des commandes piloté par les événements : `payment-orchestrator`

Après `sales_order_place_before` déclenchement, App Builder reçoit l’événement. Il revalide le seuil (en tant qu’audit), enregistre un *cash pending* un commentaire sur la commande et met à jour le statut de la commande. Rien de tout cela ne nécessite un nouveau PHP, seulement REST de retour dans Commerce.

### &#x200B;2. Acceptation d&#39;espèces : `payment-accept`

Lorsqu&#39;un ERP (ou un opérateur dans le tableau de bord) confirme que de l&#39;argent a été reçu, `payment-accept` appelle `POST /V1/split-payment/orders/:id/cash-received`. La facture, l’expédition et le statut de la commande sont gérés dans Commerce. App Builder est le déclencheur.

### &#x200B;3. Baisse de trésorerie : `payment-decline`

`payment-decline` appelle `POST /V1/split-payment/orders/:id/cash-decline` et Commerce annule la commande. Même modèle que l&#39;acceptation en espèces.

### &#x200B;4. Tableau de bord de l’opérateur : `demo-dashboard`

Tableau de bord HTML autonome diffusé à partir d’une action web App Builder. Il récupère les commandes qui attendent de l’argent liquide du REST Commerce et fournit des actions **[!UICONTROL Accept]**/**[!UICONTROL Decline]** qui appellent les actions App Builder ci-dessus. Commerce **[!UICONTROL Admin]** n’est pas obligatoire.

## Le seuil : appliqué deux fois intentionnellement

```text
Customer at checkout
        |
        v
[Commerce: CheckoutPlugin]     <- Synchronous, blocks immediately, user sees error
        |
        |  (if somehow bypassed: direct API call, and so on)
        v
[Order placed] -> I/O Event -> [App Builder: payment-orchestrator]
                                        |
                                        v
                              [evaluateThreshold()]  <- Async audit, records failure comment
```

**Commerce possède le garde orienté utilisateur ; App Builder possède l’audit post-emplacement.** C&#39;est intentionnel.

## Le crédit de la boutique : pourquoi il reste en PHP

```text
What you might think would work (it does not):
  Order placed -> I/O Event -> App Builder -> PUT /V1/carts/:id/store-credit
  (Fails: cart is inactive after place order)

What actually works:
  AroundPlaceOrder plugin
  -> BalanceManagementInterface::apply($cartId, $amount)  <- cart is still active
  -> place order
  -> order placed
  -> I/O event: App Builder (store credit is already applied)
```

Le fichier `store-credit.js` de l’orchestrateur documente cette situation. Il s’agit d’un stub sans opération avec des commentaires qui expliquent pourquoi il n’est pas utilisé.

## Attributs d’extension : la colle

Les montants fractionnés se déplacent dans le système sur les attributs d’extension :

```text
Checkout JavaScript (Knockout)
    |  POST /V1/split-payment/set
    v
SplitPaymentSession (PHP session)
    |  AroundPlaceOrder reads the session
    v
CartInterface extension attributes
    |  `sales_model_service_quote_submit_before` observer
    v
OrderInterface extension attributes -> `sales_order` flat columns
    |  I/O event payload includes these fields
    v
App Builder `payment-orchestrator` reads the split amounts
```

## Modèle de données

**`sales_order`des colonnes plates ajoutées par ce module**

| Colonne | Type | Objectif |
| --- | --- | --- |
| `split_store_credit_amount` | flotteur | Crédit de magasin appliqué |
| `split_cash_amount` | flotteur | Montant en espèces dû |
| `split_cash_status` | varchar | `pending`, `received` ou `declined` |
| `split_sc_invoice_id` | int | ID entité de la facture de crédit de magasin |
| `split_cash_invoice_id` | int | ID entité de la facture d&#39;espèces |

**Attributs d’extension** (sur `CartInterface`, `OrderInterface` et `OrderPaymentInterface`)

* `split_store_credit_amount` (flottant)
* `split_cash_amount` (flottant)
* `split_cash_status` (chaîne)

## Champs de payload des événements I/O

`observer.sales_order_place_before` est configuré dans `io_events.xml` pour inclure les éléments suivants dans l’événement :

```xml
entity_id, quote_id, increment_id, subtotal,
split_store_credit_amount, split_cash_amount, split_cash_status
```

App Builder utilise `entity_id` comme ID de commande, `split_store_credit_amount` et `split_cash_amount` pour la validation du seuil.

## Les cinq cas de figure couverts par la preuve de concept

### 1. `CapCustomerBalanceCollectPlugin`

**[!UICONTROL Customer balance]** Le collecteur de totaux natif de Commerce peut sur-appliquer (il peut voir le solde disponible complet, et non le montant de partage déclaré par la session). Ce plug-in limite le montant à la valeur déclarée dans la session.

### 2. `FixSplitPaymentGrandTotalPlugin`

Une fois le crédit du magasin appliqué, le **[!UICONTROL Grand Total]** du devis peut être réduit au montant en espèces uniquement. Le JavaScript de passage en caisse doit calculer le total de la commande pour la validation de partage *avant* cette modification. Le plug-in s’exécute après la collecte des totaux et corrige l’affichage, tandis que le JavaScript ne fait pas confiance à `grand_total` seul et reconstruit la valeur à partir des segments de sous-total.

### 3. `FixInvoiceCustomerBalanceAfterTotalsPlugin`

Lorsque les totaux de facture sont récupérés, le crédit de magasin peut être lettré deux fois. Ce plug-in corrige les `customer_balance_amount` sur les factures.

### 4. `SplitPaymentZeroTotalPlugin`

Une fois le crédit de magasin appliqué, le **[!UICONTROL Grand Total]** de panier peut être de 0 $ (commande de crédit de magasin complet). Dans ce cas, le contrôle **[!UICONTROL Zero subtotal checkout]** de Commerce peut bloquer le COD. Ce plug-in autorise le contre remboursement lorsque le montant de la session est supérieur à 0.

### &#x200B;5. Rappel de devis avant `BalanceManagementInterface::apply()`

`apply()` vérifie le montant par rapport au **[!UICONTROL Grand Total]** actuel. Si le total correspond déjà à la partie espèces uniquement, `apply()` pouvez échouer ou limiter. `PlaceOrderPlugin` suspend temporairement la correction du total général pendant l’application de l’équilibre, à l’aide d’un indicateur de session (`beginBalanceApply`/`endBalanceApply`).


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
