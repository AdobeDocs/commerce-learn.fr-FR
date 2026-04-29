---
title: 'PdC de paiement partagé : invite d’IA du module Commerce'
description: 'Découvrez comment utiliser cette invite pour générer Client_SplitPayment : REST, modules externes, passage en caisse de JavaScript, événements I/O et activer, compiler et déployer des commandes.'
feature: App Builder, Backend Development, Eventing, Extensibility, Paas, REST, Orders
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, Leader, User
level: Intermediate
doc-type: Tutorial
duration: 503
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: beb22335cec97141b46ddbbca97d21b216c55a80
workflow-type: tm+mt
source-wordcount: '1207'
ht-degree: 1%

---

# PdC de paiement partagé : invite d’IA du module Commerce

Utilisez cette page pour copier l&#39;invite complète qui génère le module en cours de `Client_SplitPayment` : REST, gestion de session, **[!UICONTROL Checkout]** et affichage **[!UICONTROL Admin]** pour la preuve de concept de paiement fractionné. Le workflow de l’opérateur reste dans App Builder.

## Utiliser cette invite

Copiez tout du **DÉBUT INVITE** au **Fin de l&#39;invite** dans Cursor (avec Claude) ou directement dans Claude. Exécutez-le à partir de la racine de votre projet Commerce ou d’un répertoire dans lequel l’IA peut créer des fichiers.

## Personnaliser avant d’exécuter

* Remplacez `Client` par le nom réel du fournisseur.
* Modifiez `SplitPayment` si vous souhaitez un autre nom de module.
* Si le site utilise un thème personnalisé, les chemins XML de disposition et RequireJS peuvent nécessiter des modifications.
* Si votre méthode de **[!UICONTROL Cash on delivery]** utilise un code différent de `cashondelivery`, mettez à jour `payment-method-helper.js`.


## L’invite

**DÉBUT DE L’INVITE**

Vous générez un module Adobe Commerce 2.4.5+ en cours de production complet pour une fonctionnalité de paiement partagé. Ce module est l&#39;adaptateur PHP fin qui expose la bonne surface REST et joint les bonnes données aux bons moments du cycle de vie Commerce. Toute la logique de workflow des opérateurs réside dans Adobe App Builder (pas dans ce module).

**Identité du module :**
* Fournisseur : `Client`
* Module : `SplitPayment`
* Nom complet : `Client_SplitPayment`
* Espace de noms : `Client\SplitPayment`
* Lieu : `app/code/Client/SplitPayment/`
* Dépendances : `Magento_Checkout`, `Magento_CustomerBalance`, `Magento_Sales`, `Magento_Quote`, `Magento_WebApi`, `Magento_AdobeCommerceEventsClient`

Générez tous les fichiers répertoriés dans la structure de fichiers ci-dessous. N’omettez aucun fichier. Utilisez `declare(strict_types=1)` dans tous les fichiers PHP.


### Structure de fichier à générer

```
app/code/Client/SplitPayment/
├── registration.php
├── composer.json
├── Api/
│   ├── Data/SplitPaymentInterface.php
│   └── SplitPaymentManagementInterface.php
├── Block/
│   └── Order/SplitPaymentInfo.php
├── Controller/
│   └── Checkout/StoreCreditBalance.php
├── etc/
│   ├── acl.xml
│   ├── config.xml
│   ├── db_schema.xml
│   ├── db_schema_whitelist.json
│   ├── di.xml
│   ├── events.xml
│   ├── extension_attributes.xml
│   ├── io_events.xml
│   ├── module.xml
│   ├── webapi.xml
│   └── frontend/
│       └── routes.xml
├── Model/
│   ├── SplitPaymentData.php
│   ├── SplitPaymentManagement.php
│   ├── Service/
│   │   └── SplitInvoiceService.php
│   └── Session/
│       └── SplitPaymentSession.php
├── Observer/
│   ├── AutoInvoiceStoreCreditOnOrderPlace.php
│   └── CopySplitPaymentToOrder.php
├── Plugin/
│   ├── CheckoutPlugin.php
│   ├── OrderRepositoryPlugin.php
│   ├── PlaceOrderPlugin.php
│   ├── Adminhtml/
│   │   └── OrderPaymentPlugin.php
│   ├── Checkout/
│   │   └── LayoutProcessorPlugin.php
│   ├── CustomerBalance/
│   │   └── CapCustomerBalanceCollectPlugin.php
│   ├── Payment/
│   │   ├── Block/
│   │   │   └── AdminSplitPaymentTitlePlugin.php
│   │   └── Checks/
│   │       └── SplitPaymentZeroTotalPlugin.php
│   ├── Quote/
│   │   └── FixSplitPaymentGrandTotalPlugin.php
│   └── Sales/
│       └── FixInvoiceCustomerBalanceAfterTotalsPlugin.php
├── Setup/
│   └── Patch/
│       └── Data/
│           └── AddSplitPaymentOrderAttribute.php
└── view/
    └── frontend/
        ├── requirejs-config.js
        ├── layout/
        │   └── sales_order_view.xml
        ├── templates/
        │   └── order/
        │       └── split-payment-info.phtml
        └── web/
            ├── js/
            │   ├── model/
            │   │   └── payment-method-helper.js
            │   └── view/
            │       └── payment/
            │           ├── cashondelivery-method.js
            │           └── split-payment.js
            └── template/
                └── payment/
                    ├── cashondelivery.html
                    └── split-payment.html
```


### Spécifications comportementales

#### &#x200B;1. Schéma De Base De Données (`etc/db_schema.xml`)

Ajoutez ces colonnes à `sales_order` (ressource : `sales`) :

| Colonne | Type | Nul | Commentaire |
|---|---|---|---|
| `split_store_credit_amount` | decimal(12,4) | oui | Partie du crédit de magasin |
| `split_cash_amount` | decimal(12,4) | oui | Tranche d&#39;encaissement due |
| `split_cash_status` | varchar(32) | oui | `pending` / `received` / `declined` |
| `split_sc_invoice_id` | int sans signe | oui | ID entité de facture de crédit de magasin |
| `split_cash_invoice_id` | int sans signe | oui | ID entité de facture d&#39;espèces |

Générez également le `db_schema_whitelist.json` de ces colonnes.

#### &#x200B;2. Attributs d’extension (`etc/extension_attributes.xml`)

Ajoutez `split_store_credit_amount` (flottant), `split_cash_amount` (flottant), `split_cash_status` (chaîne) à :
* `Magento\Quote\Api\Data\CartInterface`
* `Magento\Sales\Api\Data\OrderInterface`
* `Magento\Sales\Api\Data\OrderPaymentInterface`

#### &#x200B;3. Points d’entrée REST (`etc/webapi.xml`)

```xml
POST /V1/split-payment/set              → anonymous (session-scoped)
POST /V1/split-payment/orders/:orderId/cash-received  → Magento_Sales::actions
POST /V1/split-payment/orders/:orderId/cash-decline   → Magento_Sales::cancel
```

Tous les trois vont à `Client\SplitPayment\Api\SplitPaymentManagementInterface`.

#### &#x200B;4. Événements I/O (`etc/io_events.xml`)

Abonnez-vous aux champs `observer.sales_order_place_before` et inclure :
`entity_id`, `quote_id`, `increment_id`, `subtotal`, `split_store_credit_amount`, `split_cash_amount`, `split_cash_status`

#### &#x200B;5. `SplitPaymentSession` — Wrapper de session

Stocke les montants fractionnés déclarés dans la session de paiement. Doit exposer :
* `setAmounts(float $storeCredit, float $cash): void`
* `getAmounts(): array` — renvoie `['store_credit' => float, 'cash' => float]`
* `clear(): void`
* `beginBalanceApply(): void` — définit un indicateur supprimant le plug-in de correction du total global lors de l&#39;application du crédit de magasin
* `endBalanceApply(): void`
* `isBalanceApplyInProgress(): bool`

#### &#x200B;6. `SplitPaymentManagement` — Contrôleur REST

**`setSplitPayment(float $storeCreditAmount, float $cashAmount, ?string $cartId = null): bool`**
* Valide le panier appartenant à la session en cours (en comparant les identifiants numériques et de devis masqués)
* Stocke les montants en `SplitPaymentSession`
* Renvoie un `true` en cas de succès ; renvoie un `LocalizedException` avec un message générique en cas d’échec

**`markCashReceived(int $orderId): bool`**
* Charge l&#39;ordre des `entity_id`
* Valide les `split_cash_status === 'pending'`
* Définit le statut sur `received`, le statut sur `processing`
* Ajoute un commentaire d&#39;historique : `"Cash payment of $X.XX received."`
* Appels `SplitInvoiceService::createCashPortionInvoice($orderId)`
* Ajoute un commentaire avec l&#39;ID incrément de facture d&#39;espèces
* Appelle `createShipmentIfPossible($orderId)` — crée une expédition à l&#39;aide de `ShipOrder::execute()` s&#39;il y a des articles livrables
* Renvoie `true` ; renvoie une `LocalizedException` générique en cas d’erreur

**`markCashDeclined(int $orderId): bool`**
* Charge l’ordre
* Valide les `split_cash_status === 'pending'`
* Valide les `$order->canCancel()`
* Définit le statut sur `declined` et ajoute un commentaire : `"Cash payment declined (simulated fraud check)."`
* Enregistre la commande
* Appels `OrderManagement::cancel($orderId)`
* Renvoie `true` ; renvoie une `LocalizedException` générique en cas d’échec.

#### &#x200B;7. `PlaceOrderPlugin` — aboutPlaceOrder sur `QuoteManagement`

Il s’agit du plug-in le plus important. S’exécute `aroundPlaceOrder` :

1. Charger le devis ; s&#39;il n&#39;est pas actif, appeler immédiatement `$proceed()`
2. Lecture `SplitPaymentSession::getAmounts()`
3. Si `customer_balance_amount_used > 0` sur le devis, appelez `BalanceManagementInterface::remove($cartId)` (handle `LocalizedException` — journal et renvoi comme message générique)
4. `recollectQuoteForBalanceOperation()` d&#39;appel — charge les guillemets, appelle les `collectTotals()`, enregistre (dans `beginBalanceApply()` / `endBalanceApply()` pour supprimer le plug-in de correction du grand total)
5. Si `$storeCredit > 0`, appelez `BalanceManagementInterface::apply($cartId, $storeCredit)` (échec de gestion)
6. Recharger la citation ; définir les attributs d’extension : `split_store_credit_amount`, `split_cash_amount`, `split_cash_status = 'pending'`
7. Enregistrer le `payment->additional_information['client_split_payment']` en tant que `['store_credit' => x, 'cash' => y]`
8. Enregistrer le devis
9. `$proceed()` d’appel, ID de commande de capture
10. `SplitPaymentSession::clear()` d’appel
11. ID de l&#39;ordre de retour

#### &#x200B;8. `CopySplitPaymentToOrder` — Observateur sur `sales_model_service_quote_submit_before`

Lit les montants fractionnés des attributs d&#39;extension de session → de paiement additional_information → de devis (dans cet ordre de priorité). Écrit des `split_store_credit_amount`, des `split_cash_amount` et des `split_cash_status = 'pending'` dans l’objet order.

#### &#x200B;9. `AutoInvoiceStoreCreditOnOrderPlace` — Observateur sur `sales_order_place_after`

Après le placement de la commande, si la commande comporte un montant de crédit de magasin (`split_store_credit_amount > 0`), appelez le `SplitInvoiceService::createStoreCreditPortionInvoice($orderId)` pour facturer immédiatement la partie crédit de magasin.

#### 10. `SplitInvoiceService`

**`createStoreCreditPortionInvoice(int $orderId): ?int`**
Crée une facture pour la partie crédit de magasin uniquement. Définit le `customer_balance_amount` de la facture sur le montant du crédit de magasin. Enregistre et enregistre la facture. Retourne l&#39;identifiant de l&#39;entité facture ou la valeur null en cas d&#39;échec (log ; ne pas renvoyer).

**`createCashPortionInvoice(int $orderId): ?int`**
Crée une facture pour la partie espèces (les articles/montants de commande restants non couverts par la facture SC). S’enregistre et enregistre. Renvoie l’ID de l’entité ou la valeur null en cas d’échec.

#### &#x200B;11. `CheckoutPlugin` — le `PaymentInformationManagement`

Plug-in sur `Magento\Checkout\Model\PaymentInformationManagement::savePaymentInformationAndPlaceOrder`. Avant de poursuivre :
* Charger le devis de la session de passage en caisse
* Obtenez le seuil configuré : `Magento\Framework\App\Config\ScopeConfigInterface::getValue('split_payment/general/threshold')`, `100` par défaut
* Si `$quote->getGrandTotal() > $threshold`, lancez `LocalizedException('Payment could not be processed. Please try again or contact support.')`

#### &#x200B;12. `CapCustomerBalanceCollectPlugin` — sur `Customerbalance` total

Après les exécutions de collecte du total du solde client natif, limitez la `customer_balance_amount_used` à `SplitPaymentSession::getAmounts()['store_credit']`. Cela empêche Commerce de surappliquer le solde client complet lorsque le client a déclaré une partie de crédit de magasin plus petite.

#### &#x200B;13. `FixSplitPaymentGrandTotalPlugin` — le `Quote\Address\Total\Grand`

Après le recouvrement du total général : si une session de paiement fractionné existe et que `isBalanceApplyInProgress()` est faux, définissez le total général du devis sur le montant en espèces de la session. Ainsi, l’interface utilisateur de passage en caisse affiche uniquement ce qui est dû en espèces.

#### &#x200B;14. `FixInvoiceCustomerBalanceAfterTotalsPlugin` — le `Sales\Model\Order\Invoice`

Une fois les totaux de facture collectés, si la commande associée à la facture comporte un `split_sc_invoice_id`, corrigez les `customer_balance_amount` de la facture pour éviter une double imputation du crédit de magasin.

#### &#x200B;15. `SplitPaymentZeroTotalPlugin` — le `Payment\Model\Checks\ZeroTotal`

Autoriser le paiement de la contre remboursement lors de la `SplitPaymentSession::getAmounts()['cash'] > 0`, même si le total général du devis est de 0 $ (car le crédit du magasin couvre la commande entière).

#### &#x200B;16. `LayoutProcessorPlugin` — le `Checkout\Block\Checkout\LayoutProcessor`

Une fois la mise en page traitée :
* Insérez le composant `Client_SplitPayment/js/view/payment/split-payment` dans les `additional` enfants du composant `cashondelivery` de mode de paiement à l’adresse `components.checkout.children.steps.children.billing-step.children.payment.children.renders.children.offline-payments.children.cashondelivery.children.additional`
* Supprimez le composant d’interface utilisateur de crédit de magasin natif (`customerBalance`, `useStoreCredit`) de l’étape de paiement : le composant de paiement partagé possède l’affichage/l’application du crédit de magasin

#### &#x200B;17. `OrderRepositoryPlugin` — le `OrderRepositoryInterface`

Après `get()` et `getList()`, décompressez les attributs d’extension de l’ordre des colonnes `sales_order` aplaties (`split_store_credit_amount`, `split_cash_amount`, `split_cash_status`).

#### &#x200B;18. `AdminSplitPaymentTitlePlugin` — le `Payment\Block\Info`

Après `getTitle()` retours, si le mode de paiement est `cashondelivery` et que la commande comporte un paiement fractionné, ajoutez `" (Split: Cash $X.XX + Store Credit $Y.YY)"` au titre.

#### &#x200B;19. `OrderPaymentPlugin` — le `Sales\Block\Adminhtml\Order\Payment`

Dans `_beforeToHtml` ou via afterToHtml, ajoutez les détails du paiement fractionné (montant en espèces, montant du crédit de la boutique, statut) au bloc de paiement HTML dans la vue de commande de l’administrateur Commerce.

#### &#x200B;20. Contrôleur de solde créditeur de magasin Storefront

`Controller/Checkout/StoreCreditBalance.php` — itinéraire : `GET /splitpayment/checkout/storecreditbalance`

Renvoie JSON : `{"balance": float, "logged_in": bool}`. Si le client n’est pas connecté, renvoie `{"balance": 0, "logged_in": false}`. Lit le solde de `Magento\CustomerBalance\Model\Balance`.

Enregistrez l’itinéraire front-end en `etc/frontend/routes.xml` avec `frontName="splitpayment"`.

#### &#x200B;21. Composant KnockoutJS d’extraction — `split-payment.js`

Un `uiComponent` qui :
* Détecte quand le mode de paiement `cashondelivery` est sélectionné (`quote.paymentMethod.subscribe`)
* Charge le solde créditeur de la boutique du client via `GET /splitpayment/checkout/storecreditbalance`
* Pré-remplit le champ du montant en espèces avec le total de la commande complet (calculé à partir de `total_segments` hors `grand_total` et `customerbalance` — n&#39;utilise jamais `grand_total` directement, car il peut être défini sur le reste en espèces)
* Lorsque le client modifie la saisie du montant en espèces : calcule le crédit de magasin nécessaire = montant total de la commande − espèces ; valide ; valide ; `POST /V1/split-payment/set`
* Affiche les messages de validation pour : espèces > total de la commande, crédit de magasin insuffisant, non connecté
* Affiche un message de réussite lorsqu’un partage valide est saisi : `"The remaining $X.XX will automatically be applied from your store credit."`
* Se réinitialise lorsqu&#39;un autre mode de paiement est sélectionné (publie `{storeCreditAmount: 0, cashAmount: 0}` pour effacer la session)

#### 22. `cashondelivery-method.js`

Il étend `Magento_OfflinePayments/js/view/payment/offline-payments`. Utilise des `payment-method-helper.js` pour détecter le code de méthode de paiement. Enregistre `split-payment` composant dans sa région `additional`.

#### 23. `payment-method-helper.js`

Utilitaire renvoyant des `getCashMethodCode()` — vérifie `window.checkoutConfig.paymentMethods` les `cashondelivery` ; retourne à `checkmo` si nécessaire.

#### &#x200B;24. `cashondelivery.html` Modèle

Modèle COD standard mais inclut `<!-- ko foreach: getRegion('additional') -->` région afin que le composant enfant de paiement fractionné puisse être rendu.

#### &#x200B;25. `split-payment.html` Modèle

Modèle KnockoutJS pour les champs de paiement fractionné :
* Affichage du solde créditeur du magasin disponible
* Saisie du montant en espèces (nombre, étape 0.01)
* Affichage de la partie crédit du magasin (lecture seule)
* Appliquer automatiquement le message de crédit de magasin (affiché lorsque le fractionnement est valide et que le crédit de magasin est > 0)
* Message d’erreur de validation

#### 26. `requirejs-config.js`

Cartes :
* `Client_SplitPayment/js/view/payment/split-payment` → composant
* `Client_SplitPayment/js/view/payment/cashondelivery-method` → remplacement COD
* `Client_SplitPayment/js/model/payment-method-helper` → la fonction helper

#### 27. `etc/config.xml`

Valeurs de configuration système par défaut :

```xml
<split_payment>
  <general>
    <threshold>100</threshold>
    <enabled>1</enabled>
  </general>
</split_payment>
```


### Notes critiques d’implémentation

**La demande de crédit de magasin doit utiliser des `BalanceManagementInterface`, et non une manipulation directe du modèle.** `BalanceManagementInterface::apply()` gère de manière atomique le recalcul des sessions, des validations et des paniers.

**`PlaceOrderPlugin`devez utiliser `aroundPlaceOrder` (non `beforePlaceOrder`).** Le crédit de magasin doit être appliqué pendant que le panier est toujours actif, ce qui doit être garanti avant l’appel de `$proceed()`.

**Le modèle d’indicateur de session pour `beginBalanceApply`/`endBalanceApply` est critique.** Sans cela, le `FixSplitPaymentGrandTotalPlugin` s&#39;exécute au cours de la `collectTotals()` dans l&#39;opération de solde et fixe le total général au solde de trésorerie, ce qui entraîne la faillite de l&#39;`BalanceManagementInterface::apply()` ou le plafonnement du crédit.

**Ne jamais exposer les détails de l’erreur interne au client.** Tous les blocs de `catch` qui font surface pour les réponses REST doivent générer des `LocalizedException('Payment could not be processed. Please try again or contact support.')`.

**`entity_id`est l’identifiant numérique de base de données.** Les appels REST d’App Builder utilisent toujours `entity_id`, et non `increment_id`.

**`SplitInvoiceService`devez capturer et consigner les erreurs plutôt que de les propager.** L’échec de création de facture ne doit pas annuler une commande déjà passée. Consignez l’échec et laissez l’administrateur le gérer manuellement.


### Après la génération des fichiers

Exécutez les commandes suivantes à la racine du projet Commerce :

```bash
bin/magento module:enable Client_SplitPayment
bin/magento setup:upgrade
bin/magento setup:di:compile
bin/magento setup:static-content:deploy -f
bin/magento cache:flush
```

### Fin de l&#39;invite


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
