---
title: Existence du modèle de données de catalogue composable (CCDM)
description: Découvrez comment le CCDM conserve un catalogue unifié tandis que les storefronts reçoivent des produits, des prix et des règles filtrés à l’aide des vues de catalogue et des services de marchandisage.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 259
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-18624
source-git-commit: e3257f9713b26b0ab8ca2e827aeaac4532ff9dff
workflow-type: tm+mt
source-wordcount: '597'
ht-degree: 0%

---

# Explication du modèle de données de catalogue composable

Les équipes commerciales modernes vendent souvent entre **marques**, **régions**, **concessionnaires** et **canaux numériques**. Lorsque chaque canal conserve sa propre copie de catalogue, les équipes passent plus de temps à réconcilier les SKU, les prix et la disponibilité qu’à améliorer l’expérience de l’acheteur. Le **modèle de données de catalogue composable Adobe (CCDM)** derrière **Adobe Commerce Optimizer** est conçu pour inverser ce modèle : **un catalogue unifié** dans une couche SaaS, avec **vues de catalogue** et **politiques** qui façonne ce que chaque storefront ou intégration est autorisé à voir.

## À qui s&#39;adresse cette vidéo ?

* Architectes et développeurs de solutions Commerce débutants dans Adobe Commerce Optimizer et qui ont besoin du contexte métier avant de configurer des sources de catalogue, des vues et des politiques

## Contenu vidéo

* Pourquoi les catalogues dupliqués spécifiques aux canaux créent des risques opérationnels et ralentissent l’innovation
* Comment le CCDM conserve les données de produit unifiées tout en prenant en charge les scénarios multi-marques et multi-régions
* comment les vues de catalogue agissent comme la « lentille » entre un catalogue de base partagé et un storefront ou une audience spécifique ;
* la manière dont les API Merchandising Services utilisent ces vues afin que les expériences découplées restent alignées sur le catalogue configuré ;

>[!VIDEO](https://video.tv.adobe.com/v/3491285?learn=on)

## Le défi des catalogues compartimentés

Lorsque chaque site de revendeur, vitrine régionale ou propriété de marque conserve **sa propre base de données de catalogue**, plusieurs problèmes se posent :

* **Duplication** — Le même SKU, la même description et le même média sont saisis plusieurs fois.
* **Dérive** — les mises à jour de prix, les nouveaux attributs ou les articles abandonnés arrivent sur un canal, mais pas sur d&#39;autres.
* **Lancements plus lents** — chaque nouveau point de contact répète un travail de données important au lieu de réutiliser un seul enregistrement de produit.

Le CCDM permet aux informations sur les produits de se trouver dans **un seul catalogue composable** enrichi par d&#39;autres systèmes, tandis que les vitrines reçoivent toujours des assortiments et des prix **appropriés**.

## Ce que le modèle de données de catalogue composable modifie

Dans Adobe Commerce Optimizer, les données de produit sont **ingérées dans un catalogue de base unifié** à partir d’une ou de plusieurs **sources de catalogue** (par exemple, des paramètres régionaux comme `en-US` ou des systèmes en amont comme un PIM ou un ERP). Cette source fournit les attributs et les valeurs bruts.

**Vues catalogue** définissez ensuite comment ces données unifiées sont **organisées et exposées** pour un contexte commercial : quels produits transmettent vos **politiques**, quels **livres de prix** s’appliquent et quelle **source du catalogue** soutient la vue. Les mêmes enregistrements sous-jacents peuvent donc prendre en charge **de nombreuses projections** par exemple des vues distinctes par concessionnaire, région ou marque, sans cloner l’intégralité du catalogue pour chaque site.

Cette séparation (**d’où proviennent les données** (source du catalogue) par rapport à **leur présentation** (vue du catalogue)) est la raison principale pour laquelle les équipes adoptent le CCDM au lieu de gérer des catalogues parallèles par canal.

## Vues catalogue comme objectif storefront

Comme décrit dans [Vues de catalogue pour les services de marchandisage](https://experienceleague.adobe.com/fr/docs/commerce/optimizer/setup/catalog-view){target="_blank"}, une vue de catalogue se comporte comme une **lentille** : les acheteurs ne voient que les produits, les prix et les règles que la vue autorise, tandis que le **catalogue de base** reste le système d’enregistrement partagé. Ce modèle s’associe directement aux **services de marchandisage** afin que les clients API transmettent la vue correcte (et les en-têtes associés) et reçoivent une réponse cohérente et axée sur les politiques pour chaque expérience.

Pour une présentation plus approfondie de la manière dont ces éléments s’adaptent à un flux de bout en bout, consultez la présentation destinée aux développeurs [Créer un catalogue composable pour votre storefront](https://developer.adobe.com/commerce/services/optimizer/ccdm-use-case/){target="_blank"}.

## Contenu connexe

* [En savoir plus sur les vues de catalogue](./learn-about-the-ccdm-feature-catalog-views.md)
* [Vues de catalogue pour les services de marchandisage](https://experienceleague.adobe.com/fr/docs/commerce/optimizer/setup/catalog-view){target="_blank"}
* [Créer un catalogue composable pour votre storefront](https://developer.adobe.com/commerce/services/optimizer/ccdm-use-case/){target="_blank"}
* [Guide [!DNL Adobe Commerce Optimizer]](https://experienceleague.adobe.com/fr/docs/commerce/optimizer/overview){target="_blank"}
* [Prise en main de l’API de marchandisage](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
