---
title: Création d’une règle de prix du panier
description: Découvrez comment créer des règles de prix de panier qui appliquent des remises dans le panier en fonction d’un ensemble de conditions.
doc-type: feature video
audience: all
activity: use
last-substantial-update: 2022-12-28T00:00:00Z
feature: Configuration, System, Customers, Shopping Cart
topic: Commerce, Administration
role: Admin, Leader, User
level: Beginner, Intermediate
exl-id: ae8cab73-8a8b-4266-8205-b7397633e9bf
source-git-commit: 273119420a7051b1833d9b796bdce36e17d893c7
workflow-type: tm+mt
source-wordcount: '687'
ht-degree: 0%

---

# Création d’une règle de prix du panier

Les règles de prix du panier s’appliquent aux articles du panier, selon un ensemble de conditions. La remise peut être appliquée automatiquement lorsque les conditions sont remplies ou lorsque le client saisit un code de bon valide. Une fois appliquée, la remise apparaît dans le panier sous le sous-total. Une règle de prix de panier peut être utilisée selon les besoins pour une saison ou une promotion en modifiant son statut et sa plage de dates.

## Pour qui est cette vidéo ?

- Marketing du commerce électronique
- Chargés de site web

## Contenu vidéo

>[!VIDEO](https://video.tv.adobe.com/v/343835?quality=12&learn=on)

## Problèmes d&#39;affichage des tarifs

Il existe des scénarios uniques qui exigent que chaque article affiche sa remise, mais les valeurs peuvent ne pas correspondre exactement. La raison est qu’une remise de règle de prix de panier est appliquée à plusieurs produits, mais que les valeurs ne se divisent pas uniformément en deux décimales.

>[!BEGINSHADEBOX]

Règle de prix du panier = 10 % de remise appliquée à 2 produits du panier
Condition de prise en compte de la règle de prix : le total des articles dans le panier est de 2
Les actions appliquent le pourcentage de remise sur le prix du produit et ce montant de remise est de 10

2 éléments sont ajoutés au panier, chacun étant de 19,95 $

Pour obtenir le montant de la remise, multipliez le prix du produit par 0,1

19,95 x 0,1 = 1,995

C&#39;est le problème, nous avons 3 décimales, au lieu de deux. Convertir ceci en dollars est maintenant un problème.

>[!ENDSHADEBOX]

### La solution

En pensant au propriétaire du site, qui est la seule personne concernée par ce problème, il a été déterminé que l&#39;affichage de chaque article commandé avec la remise en dollars était la plus appropriée. Pour que le montant de la commande soit correctement calculé, il a été décidé de arrondir le premier élément et les autres de supprimer la troisième décimale. Consultez ce scénario :

>[!BEGINSHADEBOX]

Même remise de 10 % que la règle de panier ci-dessus en vigueur
Ajoutez 2 produits au panier 19.95.

Chaque produit doit bénéficier de remises de 1,995 $
Produit 1 - 19,95 x 0,1 = 1,995
2 - 19,95 x 0,1 = 1,995

Un total total de 3,99 est fourni en remise au client

Lors de l’affichage des éléments de ligne au propriétaire du magasin dans l’administrateur,
nous devons ajuster le premier élément et l’arrondir à 2 000. Le deuxième élément est la troisième décimale.
Produit 1 = 2,00
Produit 2 = 1,99

La remise totale des deux produits cumulée correspond désormais à la remise réelle accordée à un client.
>[!ENDSHADEBOX]

Voici une capture d’écran telle qu’elle s’afficherait dans l’administrateur pour une commande qui présente le scénario suivant :

![Vue d’administration affichant les éléments triés avec des valeurs différentes](../assets/commerce-admin-cart-price-rule-values-different.png)

### Autres solutions potentielles et raisons pour lesquelles elles n’ont pas été utilisées

>[!BEGINSHADEBOX]

Même remise de 10 % que la règle de panier ci-dessus en vigueur
Ajoutez 2 produits au panier 19.95.

Chaque produit doit recevoir 1,995 $ en remises,
cependant, si nous les additionnons, cela donne trop de réduction.

Produit 1 - 19,95 x 0,1 = 1,995
Produit 2 - 19,95 x 0,1 = 1,995

Convertir pour arrondir tous les éléments
Product 1 Nouvelle valeur : 2,00
Product 2 Nouvelle valeur : 2,00

Un total de 3,99 a en fait été accordé en remise au client,
cependant, si nous additionnons, cela montrerait que 4,00 $ ont été donnés, ce qui est incorrect.

2,00 + 2,00 = 4,00 $

>[!ENDSHADEBOX]

Même problème si la troisième décimale était supprimée pour tous les articles, la remise fournie serait trop faible.

>[!BEGINSHADEBOX]

Même remise de 10 % que la règle de panier ci-dessus en vigueur
Ajoutez 2 produits au panier 19.95.

Chaque produit doit obtenir des remises de 1,995 $. Toutefois, si nous déposez simplement la troisième décimale, ceci se produit :
Produit 1 - 19,95 x 0,1 = 1,995
Produit 2 - 19,95 x 0,1 = 1,995

Convertir pour supprimer la troisième décimale de tous les éléments
Product 1 Nouvelle valeur : 1.99
Product 2 La nouvelle valeur est 1.99

Un total de 3,99 a en fait été accordé en remise au client,
cependant, si nous supprimons la troisième décimale, cela montrerait que 3,98 $ a été donné, ce qui est incorrect.

1,99 + 1,99 = 3,98 $

>[!ENDSHADEBOX]


## Ressources supplémentaires

- [Créer une règle de prix de panier - [!DNL Commerce] Guide de marchandisage et de promotions](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-create.html)
- [Codes de bon - [!DNL Commerce] Guide de marchandisage et de promotion](https://experienceleague.adobe.com/docs/commerce-admin/marketing/promotions/cart-rules/price-rules-cart-coupon.html)
