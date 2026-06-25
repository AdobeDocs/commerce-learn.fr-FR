---
title: En savoir plus sur les politiques CCDM dans le modèle de données de catalogue composite
description: Découvrez comment les politiques STATIC et TRIGGER dans le modèle de données de catalogue composable Adobe contrôlent la visibilité du produit sur toutes les vues du catalogue sans recréer le catalogue.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 349
last-substantial-update: 2026-05-21T00:00:00Z
jira: KT-21258
source-git-commit: 456f3cae8c45d137a195456692c2d11204126bb7
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Politiques dans le modèle de données de catalogue composable Adobe

Si une **vue catalogue** est l’objectif qui façonne ce que les acheteurs voient à partir d’un catalogue de base unifié, les **politiques** sont ce dont cet objectif est composé. Ce tutoriel explique ce qu’est une politique, comment les politiques **STATIC** et **TRIGGER** fonctionnent ensemble dans le scénario de démonstration **Carvelo Automobiles** et pourquoi la mise à jour d’une politique prend effet immédiatement, sans devoir reconstruire le catalogue.

## À qui s&#39;adresse cette vidéo ?

* Architectes et développeurs de solutions Commerce qui configurent des vues de catalogue et des règles de marchandisage sur Adobe Commerce Optimizer

## Contenu vidéo

* Politiques en tant que filtres d’accès aux données sur les attributs de produit
* Combinaison de plusieurs politiques dans la vue Catalogue Celport (marques et catégories de pièces)
* Politiques STATIQUES pour les règles métier permanentes
* Politiques TRIGGER activées par les en-têtes de requête API (par exemple, `AC-Policy-Brand`)
* Mise à jour des politiques dans les opérations quotidiennes sans reconstruction de catalogue

>[!VIDEO](https://video.tv.adobe.com/v/3491426?captions=fre_fr&learn=on)

Une **politique** est un **filtre d’accès aux données**. Il inspecte les attributs de produit et applique des règles qui déterminent les produits qu’une vue de catalogue peut exposer. Les politiques se trouvent au-dessus du catalogue composable partagé et ne dupliquent pas les données du catalogue.

## Politiques STATIQUES

Une politique **STATIQUE** est **toujours activée**. Il applique des règles métier permanentes, quel que soit le comportement de l’acheteur ou l’état de session.

Dans le scénario Celport, les règles STATIC sont les suivantes :

* Celport ne vendra que les marques **Bolt** et **Cruz**.
* Celport affichera uniquement les pièces **freins** et **suspension**.

Ces règles ne s&#39;éteignent jamais. Les politiques STATIC sont l’endroit où **accords de licence**, **restrictions territoriales** et **autorisations de marque** s’appliquent. Définissez-les une fois et Adobe Commerce Optimizer les applique automatiquement à chaque requête.

## POLITIQUES DE DÉCLENCHEMENT

Les politiques avec une source de valeur **TRIGGER** sont parfois appelées **politiques exclusives**. La vue Catalogue exécute cette politique **uniquement lorsque le déclencheur est spécifié** dans l’en-tête de l’appel API.

Par exemple, un storefront Experience Delivery Services (EDS) peut proposer une liste déroulante avec **Toutes les marques**, **Aurora**, **Bolt** et **Cruz** :

* Sur la page initiale vue, rien n’est sélectionné, donc aucun en-tête de marque supplémentaire n’est envoyé.
* Lorsque l’acheteur sélectionne **Bolt**, l’API sous-jacente définit l’en-tête `AC-Policy-Brand` sur `Bolt`.
* Lorsque l’acheteur sélectionne **Cruz**, le même en-tête est défini sur `Cruz`.

La vue Catalogue applique la politique TRIGGER uniquement lorsque cet en-tête est présent, ce qui prend en charge le filtrage interactif sans modifier vos règles STATIQUES permanentes.

## STATIC et TRIGGER ensemble

Les politiques **STATIQUES** gèrent les règles permanentes. Les politiques **TRIGGER** gèrent les politiques interactives. Utilisés conjointement sur une seule vue de catalogue, ils offrent à la fois **conformité** et **flexibilité** : des limites d’assortiment fixes ainsi qu’un raffinement axé sur l’acheteur.

## Mettre à jour les politiques sans recréer le catalogue

Pour les opérations quotidiennes, les changements de politique sont immédiats. Supposons que le contrat de licence de Celport autorise désormais **pneus** ainsi que les freins et la suspension :

1. Ouvrez la politique appropriée.
1. Ajoutez **pneus** à la liste de valeurs.
1. Enregistrer.

Ce changement est **en ligne immédiatement**. Il n’y a pas de temps de reconstruction, de redéploiement ou d’attente du catalogue. La requête suivante au storefront Celport reflète la mise à jour.

Les politiques sont des filtres légers sur un **catalogue partagé**, et non des règles intégrées dans des copies de catalogue distinctes. **Modifiez la règle, pas les données.**

## Contenu connexe

* [Explication du modèle de données de catalogue composable](./why-ccdm-exists.md)
* [En savoir plus sur les vues de catalogue](./learn-about-the-ccdm-feature-catalog-views.md)
* [Vues de catalogue pour les services de marchandisage](https://experienceleague.adobe.com/fr/docs/commerce/optimizer/setup/catalog-view){target="_blank"}
* [Guide [!DNL Adobe Commerce Optimizer]](https://experienceleague.adobe.com/fr/docs/commerce/optimizer/overview){target="_blank"}
* [Prise en main de l’API de marchandisage](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api#make-your-first-request){target="_blank"}

