---
title: Create Cart Price Rules
description: Learn how to create cart price rules that apply discounts in the shopping cart when conditions you define are met.
doc-type: Tutorial
last-substantial-update: 2022-12-28T00:00:00.000Z
feature: Configuration, System, Customers, Shopping Cart
topic: Commerce, Administration
role: User
level: Beginner
duration: 353
jira: KT-17148
exl-id: ae8cab73-8a8b-4266-8205-b7397633e9bf
TQID: https://experienceleague.adobe.com/2gmoGQBVz2foQwnGJRlXzWF-OkNGZtiJkQWy0F-0utg
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: d1e21356-0064-4f48-9089-16e3f0dbd2a6
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 701
ht-degree: 0%

---

# Create Cart Price Rules

Cart price rules apply discounts to items in the shopping cart based on conditions you set. The discount can apply automatically when conditions are met, or when the customer enters a valid coupon code. The discount appears in the cart under the subtotal. You can turn a rule on or off for a season or promotion by changing its status and date range.

## À qui s&#39;adresse cette vidéo ?

* eCommerce marketers
* Gestionnaires de site web

## Contenu vidéo

* Create cart price rules and optional coupon codes.
* See how discounts appear in the cart and for promotions.

>[!VIDEO](https://video.tv.adobe.com/v/343835?learn=on)

## Pricing display issues

In some cases, each line item must show the discount applied, but displayed values may not match exactly. This happens when a cart price rule applies one discount across multiple products and the split does not divide evenly to two decimal places.

>[!BEGINSHADEBOX]

Cart Price Rule = 10% discount applied to 2 products in the cart
Condition for price rule to take effect: total items in cart is 2
Actions apply percent of product price discount and that discount amount is 10

2 items are added to the cart, each are $19.95

To get the discount amount multiply the product price times 0.1

19.95 x 0.1 = 1.995

This is the issue, we have 3 decimal places, instead of two. Converting this to dollars is now a problem

>[!ENDSHADEBOX]

### The solution

Pour le commerçant dans l&#39;Admin, l&#39;approche la plus claire est d&#39;afficher chaque ligne commandée avec sa remise en dollars. Pour que le total de la commande reste correct, arrondissez la première ligne vers le haut et déposez la troisième décimale sur les autres lignes. Examinez ce scénario :

>[!BEGINSHADEBOX]

Même remise de 10 % que la règle du panier ci-dessus en vigueur
Ajouter 2 produits au panier de 19,95

Chaque produit devrait bénéficier de remises de 1,995 $
Produit 1 - 19,95 x 0,1 = 1,995
2 - 19,95 x 0,1 = 1,995

Un total général de 3,99 est fourni à titre de remise au client

Lors de l’affichage des éléments de ligne au propriétaire du magasin dans l’administrateur,
nous devons ajuster le premier élément et l&#39;arrondir à 2 000. Pour le deuxième élément, supprimez la troisième décimale.
Produit 1 = 2,00
Produit 2 = 1,99

La remise totale des deux produits maintenant, lorsqu&#39;elle est additionnée, correspond à la remise réelle fournie à un client.
>[!ENDSHADEBOX]

Voici une capture d’écran qui s’afficherait dans l’administration pour une commande avec ce scénario :

![Vue Administration affichant les éléments triés avec différentes valeurs](../assets/commerce-admin-cart-price-rule-values-different.png)

### Autres solutions potentielles et raisons pour lesquelles elles n’ont pas été utilisées

>[!BEGINSHADEBOX]

Même remise de 10 % que la règle du panier ci-dessus en vigueur
Ajouter 2 produits au panier de 19,95

Chaque produit devrait obtenir 1,995 $ de rabais,
cependant, si nous les arrondissons, cela montre trop de réduction.

Produit 1 - 19,95 x 0,1 = 1,995
Produit 2 - 19,95 x 0,1 = 1,995

Convertir pour arrondir tous les éléments
Product 1 New value is 2.00
Product 2 New value is 2.00

A grand total of 3.99 was actually provided as a discount to the customer,
however if we round up, it would show that $4.00 was given, and that is incorrect.

2.00 + 2.00 = $4.00

>[!ENDSHADEBOX]

Similar issue if the third decimal was dropped for all items, it would show too little discount provided.

>[!BEGINSHADEBOX]

Même remise de 10 % que la règle du panier ci-dessus en vigueur
Ajouter 2 produits au panier de 19,95

Each product should get $1.995 in discounts, however if we just drop the third decimal, this happens:
Produit 1 - 19,95 x 0,1 = 1,995
Produit 2 - 19,95 x 0,1 = 1,995

Convert to drop the third decimal for all items
Product 1 New value is 1.99
Product 2 New value is 1.99

A grand total of 3.99 was actually provided as a discount to the customer,
however if we drop the third decimal, it would show that $3.98 was given, and that is incorrect.

1.99 + 1.99 = $3.98

>[!ENDSHADEBOX]

## Ressources supplémentaires

* [Create a Cart Price Rule - [!DNL Commerce] Merchandising and Promotions Guide](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-create.html){target="_blank"}
* [Coupon Codes - [!DNL Commerce] Merchandising and Promotions Guide](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-coupon.html){target="_blank"}
