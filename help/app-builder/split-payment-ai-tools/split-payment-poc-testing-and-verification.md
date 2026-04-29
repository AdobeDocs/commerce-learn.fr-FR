---
title: 'PdC de paiement partagé : guide de test et de vérification'
description: 'Découvrez comment vérifier la preuve de concept de paiement partagé : installation de Commerce, REST, passage en caisse, seuil, acceptation et refus de la simulation, tableau de bord de démonstration et journaux App Builder.'
feature: App Builder, Configuration, Extensibility, Paas, Payments, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 359
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: beb22335cec97141b46ddbbca97d21b216c55a80
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 0%

---

# PdC de paiement partagé : guide de test et de vérification

Ce guide vous guide tout au long de la vérification du bon fonctionnement de chaque composant, dans l’ordre dans lequel ils doivent être testés. Commencez par le bas (module Commerce) et remontez (App Builder).


## Étape 1 : vérification de l’installation du module Commerce

Après avoir exécuté `bin/magento setup:upgrade` et `bin/magento setup:di:compile` :

```bash
# Confirm module is enabled
bin/magento module:status Client_SplitPayment

# Confirm db_schema columns were added
bin/magento doctrine:schema:validate
# or check directly:
mysql -u <user> -p <dbname> -e "DESCRIBE sales_order;" | grep split
```

Sortie attendue : quatre colonnes commençant par `split_` visible dans `sales_order`.


## Étape 2 : vérification de la configuration de l’administrateur Commerce

Dans Commerce Admin :
1. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Sales]** > **[!UICONTROL Payment Methods]** — vérifiez **[!UICONTROL Cash On Delivery]** est activé avec le titre `Cash`
2. **[!UICONTROL Stores]** > **[!UICONTROL Configuration]** > **[!UICONTROL Customers]** > **[!UICONTROL Customer Configuration]** > **[!UICONTROL Store Credit Options]** — confirmer activée
3. Vérifiez que votre client test a un crédit de magasin : **[!UICONTROL Customers]** > **[!UICONTROL All Customers]** > *[client]* > **[!UICONTROL Store Credit]**


## Étape 3 : vérifier que les points d’entrée REST sont accessibles

Utilisez curl pour confirmer que les points d’entrée répondent (ils rejetteront les requêtes non autorisées, mais un 401 confirmera qu’elles sont correctement acheminées) :

```bash
# Should return 401 (not 404) — endpoint exists but requires auth
curl -X POST https://your-store.example.com/rest/V1/split-payment/orders/1/cash-received

# Should return 200 or session-based response — anonymous endpoint
curl -X POST https://your-store.example.com/rest/V1/split-payment/set \
  -H "Content-Type: application/json" \
  -d '{"storeCreditAmount": 0, "cashAmount": 0}'
```


## Étape 4 — Tester l’interface utilisateur de passage en caisse

1. Connectez-vous au storefront en tant que client de test (qui dispose d’un crédit de magasin)
2. Ajouter un produit au panier (total inférieur à 100 $ après expédition + taxes)
3. Passer en caisse
4. A l&#39;étape de paiement, sélectionnez **Encaissement** (ou Encaissement à la livraison)
5. Vérifiez que les champs de paiement fractionné s&#39;affichent :
   * Le solde créditeur de votre boutique s’affiche
   * Champ de montant de trésorerie prérempli avec le total de la commande
   * Champ « Crédit de magasin pour cette commande » affichant 0,00 $ (car argent comptant = total de la commande)
6. Réduisez le montant en espèces (p. ex., entrez 10 $ pour une commande de 50 $)
7. Vérifiez que la portion de crédit de magasin est mise à jour à 40 $.
8. Vérifiez que le message suivant s’affiche : `"The remaining $40.00 will automatically be applied from your store credit."`

**Validation du test :**
* Saisir un montant en espèces supérieur au message d&#39;erreur → du total de la commande
* Saisir un montant en espèces qui nécessite un crédit de magasin supérieur au montant disponible → message d&#39;erreur
* Entrer cash = 0 → erreur (ou le crédit de magasin couvre toute la commande)


## Étape 5 — Protection du seuil d&#39;essai

1. Ajouter des produits totalisant plus de 100 $ (sous-total + expédition + taxe > 100 $)
2. Passer en caisse, sélectionner **Espèces**
3. Tentative de passer la commande
4. Vérifiez que le message d’erreur s’affiche : `"Payment could not be processed. Please try again or contact support."`
5. Vérifiez que le panier est conservé (le client peut toujours ajuster le panier et réessayer).


## Etape 6 — Effectuer un test d&#39;ordre de paiement fractionné

1. Créer un panier inférieur à 100 $ (client connecté avec un crédit de magasin)
2. Sélectionner espèces au moment du passage en caisse
3. Saisissez un montant en espèces inférieur au total de la commande (par exemple, 10 $ sur une commande de 45 $)
4. Confirmer que le message de crédit de magasin s’affiche
5. Cliquez sur **Passer une commande**

Une fois la commande passée, vérifiez dans Commerce Admin :
* La commande a le statut `pending_payment`
* Order comporte deux commentaires d&#39;historique :
   1. `"Cash payment of $X.XX pending. Awaiting admin confirmation."` (à partir de l’`payment-orchestrator` App Builder)
   2. `"Split payment orchestration completed. Order awaiting cash confirmation."` (à partir d’App Builder)
* Les montants de paiement fractionnés sont visibles dans le bloc de paiement des commandes

> **Si aucun commentaire App Builder n’apparaît :** vérifiez les journaux d’actions App Builder avec `aio app logs`. L’événement ne s’est peut-être pas déclenché ou l’action a peut-être une erreur.


## Étape 7 — Tester l’acceptation via le script de simulation

Le script de simulation est le moyen le plus rapide de tester le flux d’acceptation/refus sans l’interface utilisateur complète de l’opérateur.

```bash
cd commerce-checkout-starter-kit
cp commerce-backend-ui-1/.env.simulation.example commerce-backend-ui-1/.env.simulation
# Edit .env.simulation with your credentials

# List recent orders (find your test order entity_id)
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs list

# Show split payment fields for a specific order
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs show 42

# Accept the cash payment
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs accept 42
```

Après avoir accepté, vérifiez dans la vue de commande de l’administrateur Commerce :
* Le statut de la commande est `processing`
* Commentaire sur l&#39;historique : `"Cash payment of $X.XX received."`
* Facture de caisse créée (visible dans l&#39;onglet Factures)
* Expédition créée (visible dans l&#39;onglet Expéditions, le cas échéant)
* Commentaire sur l&#39;historique : `"Split payment: cash portion invoiced #XXXXXXXX."`
* Commentaire sur l&#39;historique : `"Split payment: shipment created after cash was accepted (App Builder / API)."`


## Etape 8 — Test de refus via le script de simulation

Placez un autre ordre de test (même configuration que l’étape 6), puis :

```bash
node commerce-backend-ui-1/scripts/simulate-split-payment.mjs decline <orderId>
```

Après le refus, vérifiez dans Commerce Admin :
* Le statut de la commande est `canceled`
* Commentaire sur l&#39;historique : `"Cash payment declined (simulated fraud check)."`
* `split_cash_status` = `declined`


## Étape 9 — Tester le tableau de bord de démonstration

Après le déploiement de l’`split-payment-orchestrator`, `aio app deploy` imprime les URL d’action.

Ouvrez l’URL `demo-dashboard` dans un navigateur :

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard
```

Si `DEMO_UI_SECRET` est défini :

```
https://[runtime-host]/api/v1/web/split_payment_orchestrator/demo-dashboard?secret=<your-secret>
```

Avec une commande en attente :
1. Le tableau de bord doit afficher l’ordre dans la liste en attente
2. Cliquez sur **Accepter** → l’ordre doit passer à `processing` dans Commerce.
3. Passer une autre commande ; cliquez sur **Refuser** → la commande doit être `canceled` dans Commerce


## Étape 10 - Tester les journaux d’actions App Builder

```bash
# Follow logs in real-time
aio app logs --tail

# Or view last invocations
aio runtime activation list --limit 10
aio runtime activation logs <activation-id>
```

Pour le `payment-orchestrator`, recherchez :

```
[INFO] Split payment orchestration finished { orderId: '42' }
```

Par `payment-accept` ou `payment-decline` :

```
[INFO] Cash payment accepted on Commerce via REST { orderId: '42' }
```


## Problèmes courants et correctifs

### « La signature n’est pas valide » de Commerce OAuth

**Cause :** décalage d’horloge entre l’exécution d’App Builder et Commerce, ou bug de signature OAuth.

**Correctif:**
* Confirmer `COMMERCE_BASE_URL` n’a pas de barre oblique
* Vérifiez que les quatre informations d’identification OAuth sont pour une intégration _activée_
* Testez d’abord le script de simulation. Si le script fonctionne mais pas App Builder, il s’agit probablement d’une variable d’environnement non chargée (vérifiez si les variables d’environnement sont manquantes à la sortie `aio app deploy`).

### Les champs de paiement fractionnés n’apparaissent pas dans le passage en caisse

**Cause :** chemins d&#39;injection LayoutProcessorPlugin ne correspondent pas à votre disposition de passage en caisse.

**Correctif:**
* Recherchez les erreurs RequireJS lors du chargement dans la console du navigateur `Client_SplitPayment/js/view/payment/split-payment`
* La vérification de `bin/magento setup:static-content:deploy` a été effectuée
* Vider le cache : `bin/magento cache:flush`
* Si votre thème comporte une extraction personnalisée, le chemin d’accès `LayoutProcessorPlugin` pour injecter le composant peut nécessiter un ajustement

### Le crédit de magasin ne s’applique pas / « Le paiement n’a pas pu être traité » à la commande

**Cause :** l’un des modules externes de la casse Edge ne fonctionne généralement pas correctement.

**Vérifier :**
1. Retourne-t-`SplitPaymentSession` les bons montants ? Ajout de la journalisation de débogage temporaire dans `PlaceOrderPlugin`
2. Le `FixSplitPaymentGrandTotalPlugin` est-il en cours d&#39;exécution et affecte-t-il le total du devis avant l&#39;appel de `BalanceManagementInterface::apply()` ? Le drapeau `beginBalanceApply()` devrait le supprimer.
3. Le module Solde client est-il activé dans Commerce ?

### L’action App Builder reçoit l’événement mais orderId est manquant

**Cause :** la liste des champs `io_events.xml` n’inclut pas les `entity_id` ou la forme de la payload de l’événement a changé.

**Correctif:**
* Confirmer `io_events.xml` inclut `entity_id` dans la liste de champs
* Dans l&#39;action, connectez-vous temporairement pour `JSON.stringify(params)` la forme de payload complète
* Vérifiez que la fonction `extractValue()` trouve le niveau d’imbrication approprié

### Les commandes ne s’affichent pas dans le tableau de bord de démonstration

**Cause :** Commerce REST `orders` les critères de recherche ne renvoient pas les commandes ou `split_cash_status` champ ne se trouve pas dans la réponse REST.

**Correctif:**
* Vérifiez `OrderRepositoryPlugin` charge correctement les attributs d’extension
* Tester directement : `GET /rest/V1/orders?searchCriteria[pageSize]=5` et vérifiez si `extension_attributes.split_cash_status` apparaît dans la réponse.
* Vérifiez que `extension_attributes.xml` déclare correctement l’attribut `split_cash_status` sur `OrderInterface`


## Liste de contrôle de vérification

* [ ] `split_*` colonnes visibles dans `sales_order` tableau
* [ ] points d’entrée REST renvoient la valeur 401 (et non 404) lorsqu’ils sont appelés sans authentification
* [ ] L’interface utilisateur de paiement fractionné s’affiche lors du passage en caisse lorsque l’option Espèces est sélectionnée
* [ ] Fonctionnement des messages de validation (trop-perçu, crédit insuffisant)
* [ ] Seuil de protection bloque les commandes > 100 $
* [ ] commande passée a `pending_payment` statut et des commentaires App Builder
* [ ] `simulate-split-payment.mjs list` affiche l&#39;ordre de test avec des montants fractionnés
* [ ] `simulate-split-payment.mjs accept <id>` déplace l&#39;ordre à `processing` avec la facture et l&#39;expédition
* [ ] `simulate-split-payment.mjs decline <id>` annule la commande
* [ ] tableau de bord de démonstration répertorie les commandes en attente et accepte/refuse le travail à partir de l’interface utilisateur


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
