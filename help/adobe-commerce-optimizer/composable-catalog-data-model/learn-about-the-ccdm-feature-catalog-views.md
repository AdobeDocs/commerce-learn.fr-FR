---
title: En savoir plus sur les vues de catalogue dans le modèle de données de catalogue composite
description: Découvrez comment les vues de catalogue dans le modèle de données de catalogue composable (CCDM) d’Adobe mappent un catalogue de base à chaque storefront avec des produits, des prix et des règles distincts.
feature: Saas, Storefront
topic: Commerce
role: Developer
level: Beginner
doc-type: Tutorial
duration: 297
last-substantial-update: 2026-05-15T00:00:00Z
jira: KT-21132
source-git-commit: e3257f9713b26b0ab8ca2e827aeaac4532ff9dff
workflow-type: tm+mt
source-wordcount: '543'
ht-degree: 0%

---

# Vues de catalogue dans le modèle de données de catalogue composable Adobe

Les vues catalogue vous permettent de diffuser chaque audience différemment d’un seul catalogue composable. Ce tutoriel explique ce qu’est une vue de catalogue, comment Adobe Commerce Optimizer l’applique à un acheteur et pourquoi un identifiant de vue unique est l’ancrage des produits, des prix et des règles, à l’aide du scénario de démonstration **Carvelo Automobiles**.

## À qui s&#39;adresse cette vidéo ?

* Architectes et développeurs de solutions Commerce qui modélisent des catalogues multi-marques ou multi-revendeurs sur Adobe Commerce Optimizer

## Contenu vidéo

* Le scénario Carvelo Automobiles (marques, concessionnaires et contrats)
* Qu’est-ce qu’une vue de catalogue et la métaphore de l’« objectif » d’un catalogue de base unifié ?
* Comment un storefront utilise une vue de catalogue pour filtrer les produits et les prix (par exemple, Celport)
* Identifiants uniques d’affichage du catalogue et valeur commerciale d’une source unique de vérité

>[!VIDEO](https://video.tv.adobe.com/v/3491261?learn=on)

## Scénario : Carvelo Automobiles

**Carvelo Automobiles** est une société fictive de pièces automobiles souvent utilisée dans les démonstrations, la documentation et les tutoriels d’Adobe Commerce. Carvelo vend des pièces sur trois marques — **Aurora**, **Bolt** et **Cruz** — par l&#39;intermédiaire de trois concessionnaires :

* **Arkbridge** appartient à West Coast Inc.
* **Kingsbluff** et **Celport** appartiennent à East Coast Inc.

Chaque concessionnaire a son propre accord sur les produits qu&#39;il est autorisé à vendre. Cela crée une configuration véritablement complexe, qui peut toujours fonctionner à partir d’**un seul catalogue de base** d’environ six millions de SKU. La fonctionnalité qui rend cela possible est une **vue de catalogue**.

## Qu’est-ce qu’une vue de catalogue ?

Une **vue catalogue** est une vue configurée de votre catalogue pour un contexte métier spécifique. Considérez-le comme une **lentille**. Votre catalogue de base unifié se trouve au milieu, contenant chaque SKU pour chaque marque. Chaque concessionnaire obtient sa propre lentille, sa propre vue du catalogue.

Dans l&#39;exemple :

* L&#39;objectif **Arkbridge** peut afficher les trois marques : pièces Aurora, Bolt et Cruz.
* L&#39;objectif **Celport** ne montre qu&#39;un sous-ensemble de pièces Bolt et Cruz.

Chaque vue de catalogue peut également contrôler **les prix**. Les **données de catalogue sous-jacentes ne changent pas** seuls les aspects visibles (assortiment, prix et règles) changent pour ce contexte.

## Application d’une vue de catalogue par Commerce Optimizer

Lorsqu’un acheteur visite la vitrine **Celport**, Adobe Commerce Optimizer utilise la vue de catalogue **Celport** pour déterminer exactement quels produits, prix et règles s’appliquent. L&#39;acheteur ne voit que ce que permet cette lentille.

D’autres produits peuvent encore exister dans le catalogue (par exemple, les pneus Aurora, les moteurs Bolt ou les batteries Cruz), mais **le storefront de Celport ne les expose jamais** si la vue du catalogue ne le permet pas.

## ID de vue de catalogue et valeur commerciale

Chaque vue de catalogue possède un **ID unique**. Cet identifiant connecte votre storefront à sa configuration de catalogue. Vous la définissez une fois dans la configuration du storefront, et le comportement en aval suit : les bons produits, les bons prix et les bonnes règles.

Au lieu de conserver des catalogues distincts pour chaque concessionnaire ou marque, et de les garder synchronisés, vous conservez **un** catalogue composable. Les vues de catalogue sont la manière dont vous façonnez **de nombreuses expériences de storefront différentes** à partir de cette **source unique de vérité**.

## Contenu connexe

* [Guide [!DNL Adobe Commerce Optimizer]](https://experienceleague.adobe.com/en/docs/commerce/optimizer/overview){target="_blank"}
* [Prise en main de l’API de marchandisage](https://developer.adobe.com/commerce/services/optimizer/merchandising-services/using-the-api/#make-your-first-request){target="_blank"}
