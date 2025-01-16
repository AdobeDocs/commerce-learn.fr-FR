---
title: Créer une règle de prix de panier
description: Découvrez comment créer des règles de prix de panier qui appliquent des remises dans le panier en fonction d’un ensemble de conditions.
doc-type: feature video
audience: all
activity: use
last-substantial-update: 2022-12-28T00:00:00Z
feature: Configuration, System, Customers, Shopping Cart
topic: Commerce, Administration
role: Admin, Leader, User
level: Beginner
duration: 171
jira: KT-17148calc
exl-id: ae8cab73-8a8b-4266-8205-b7397633e9bf
source-git-commit: d5820fd053cef00f40036bcd51f7f3556c5fcd61
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 0%

---

# Créer une règle de prix de panier

Les règles de prix de panier appliquent des remises aux articles du panier, en fonction d’un ensemble de conditions. La remise peut être appliquée automatiquement lorsque les conditions sont remplies ou lorsque le client saisit un code de coupon valide. Lorsqu’elle est appliquée, la remise apparaît dans le panier sous le sous-total . Une règle de prix de panier peut être utilisée selon les besoins pour une saison ou une promotion en modifiant son statut et sa période.

## À qui s&#39;adresse cette vidéo ?

- Spécialistes du marketing eCommerce
- Gestionnaires de site web

## Contenu vidéo

>[!VIDEO](https://video.tv.adobe.com/v/343835?quality=12&learn=on)

## Problèmes d’affichage des prix

Certains scénarios uniques nécessitent que chaque ligne affiche sa remise fournie, mais les valeurs peuvent ne pas correspondre exactement. Cela s’explique par le fait qu’une remise de règle de prix de panier est appliquée à plusieurs produits, mais que les valeurs ne sont pas divisées de manière égale à deux décimales.

>[!BEGINSHADEBOX]

Règle de prix du panier = remise de 10 % appliquée à 2 produits du panier
Condition pour que la règle de prix prenne effet : le total des articles du panier est de 2
Les actions appliquent un pourcentage de remise sur le prix du produit et ce montant de remise est de 10

2 articles sont ajoutés au panier, chacun est de 19,95 $

Pour obtenir le montant de la remise, multipliez le prix du produit par 0,1

19,95 x 0,1 = 1,995

C&#39;est le problème, nous avons 3 décimales, au lieu de deux. Convertir cela en dollars est maintenant un problème

>[!ENDSHADEBOX]

### La solution

En pensant au propriétaire du site Web, qui est la seule personne touchée par ce problème, il a été déterminé que montrer chaque article commandé avec la remise fournie en dollars était la plus appropriée. Pour s&#39;assurer que le montant total de la commande est correctement calculé, il a été décidé d&#39;arrondir le premier article et les autres de supprimer la troisième décimale. Examinez ce scénario :

>[!BEGINSHADEBOX]

Même remise de 10 % que la règle du panier ci-dessus en vigueur
Ajouter 2 produits au panier de 19,95

Chaque produit devrait bénéficier de remises de 1,995 $
Produit 1 - 19,95 x 0,1 = 1,995
2 - 19,95 x 0,1 = 1,995

Un total général de 3,99 est fourni à titre de remise au client

Lors de l’affichage des éléments de ligne au propriétaire du magasin dans l’administrateur,
nous devons ajuster le premier élément et l’arrondir à 2 000. Le deuxième élément est supprimé à la troisième décimale
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
Produit 1 La nouvelle valeur est 2.00
Produit 2 La nouvelle valeur est 2.00

Un total général de 3,99 a été effectivement fourni comme une remise au client,
toutefois, si nous arrondissons les chiffres, nous constatons que 4 $ ont été versés, ce qui est inexact.

2 $ + 2 $ = 4 $

>[!ENDSHADEBOX]

Problème similaire si la troisième décimale était supprimée pour tous les éléments, la remise fournie serait trop faible.

>[!BEGINSHADEBOX]

Même remise de 10 % que la règle du panier ci-dessus en vigueur
Ajouter 2 produits au panier de 19,95

Chaque produit devrait obtenir 1,995 $ de rabais, mais si nous laissons simplement tomber la troisième décimale, cela se produit :
Produit 1 - 19,95 x 0,1 = 1,995
Produit 2 - 19,95 x 0,1 = 1,995

Convertir pour supprimer la troisième décimale pour tous les éléments
Produit 1 Nouvelle valeur : 1,99.
Produit 2 La nouvelle valeur est 1,99

Un total général de 3,99 a été effectivement fourni comme une remise au client,
toutefois, si nous laissons tomber la troisième décimale, cela indiquera que 3,98 $ ont été versés, ce qui est inexact.

1,99 + 1,99 = 3,98 $

>[!ENDSHADEBOX]


## Ressources supplémentaires

- [Créer une règle de prix de panier - [!DNL Commerce] Guide de marchandisage et de promotion](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-create.html)
- [Codes promotionnels - [!DNL Commerce] Guide de merchandising et de promotion](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-coupon.html)
