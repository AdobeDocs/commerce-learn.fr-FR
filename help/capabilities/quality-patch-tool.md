---
title: Outil de correctifs de qualité
description: Découvrez comment utiliser l’outil de correctifs de qualité lors du diagnostic d’un problème, de la recherche d’une solution et de l’application d’un correctif trouvé dans la liste des correctifs disponibles.
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 771
last-substantial-update: 2024-07-17T00:00:00Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---

# Outil de correction de la qualité

Découvrez comment utiliser l’outil de correctifs de qualité lors du diagnostic d’un problème, de la recherche d’une solution et de l’application d’un correctif trouvé dans la liste des correctifs disponibles.

## Ce que vous apprendrez

Découvrez comment trier un problème, puis utiliser certaines techniques de base pour trouver un correctif de qualité afin d’appliquer un correctif.

## Audience

* Les développeurs apprennent à résoudre les problèmes et à utiliser cet outil pour appliquer des correctifs GIT en cas de problèmes connus

## Contenu vidéo

L’outil de correctifs de qualité est un utilitaire de ligne de commande pour Adobe Commerce et Magento Open Source. Voici ce qu’il vous permet de faire :

* Affichez des informations générales sur les derniers correctifs de qualité.
* Appliquez des correctifs de qualité à votre installation.
* Rétablissement des correctifs appliqués si nécessaire

Ces correctifs sont développés à partir de la communauté Adobe Developers et Magento Open Source pour améliorer la stabilité et les performances. Gardez à l&#39;esprit qu&#39;il n&#39;est pas recommandé d&#39;appliquer un grand nombre de correctifs, car cela peut compliquer les futures mises à niveau.

>[!VIDEO](https://video.tv.adobe.com/v/3431436?learn=on)

## Pourquoi utiliser l’outil de correctif de qualité ?

Vous pouvez utiliser l’outil de correctifs de la qualité pour Adobe Commerce ou Magento Open Source si vous souhaitez :

Améliorez la stabilité et les performances : les correctifs de qualité résolvent les problèmes, améliorent la sécurité et optimisent votre installation.
Restez à jour : l’application de correctifs garantit que votre système est à jour et protégé.
Annuler les modifications : si un correctif entraîne des problèmes inattendus, vous pouvez l’annuler à l’aide de l’outil. N’oubliez pas qu’il est préférable d’appliquer un nombre limité de correctifs pour éviter de compliquer les mises à niveau ultérieures.  

## Restrictions ou problèmes liés à l’utilisation de l’outil de correctif de qualité

Bien que l’outil de correctifs de qualité offre des avantages, il existe quelques points à garder à l’esprit :

* Compatibilité : assurez-vous que les correctifs sont compatibles avec votre version spécifique d’Adobe Commerce ou de Magento Open Source.
* Tests : testez toujours les correctifs dans un environnement d’évaluation avant de les appliquer à la production. Des problèmes inattendus peuvent survenir.
* Dépendances de correctifs : certains correctifs peuvent dépendre d’autres. Tenez compte de toutes les conditions préalables.
* Personnalisations : si vous avez apporté des modifications de code personnalisé, les correctifs peuvent entrer en conflit. Examinez attentivement les modifications.
* Sauvegarde : sauvegardez votre installation avant d&#39;appliquer des correctifs pour éviter la perte de données.

Bien que l&#39;outil Correctifs de la qualité soit utile pour appliquer un nombre limité de correctifs, il n&#39;est pas recommandé pour gérer un grand volume de correctifs. L’application d’un trop grand nombre de correctifs peut compliquer les futures mises à niveau et maintenance. Si vous devez appliquer de nombreux correctifs, envisagez d’autres approches ou consultez un spécialiste Magento. 

## Résumé

L’outil de correctifs de qualité permet aux plateformes d’e-commerce d’améliorer la stabilité et la sécurité en appliquant des correctifs. Ces correctifs résolvent les problèmes, améliorent les performances et optimisent le système. La mise à jour de votre installation garantit une protection contre les vulnérabilités.

Avant d’appliquer des correctifs, il est essentiel de les tester dans un environnement d’évaluation. Vérifiez la compatibilité avec votre version spécifique d’Adobe Commerce ou de Magento Open Source. Certains correctifs peuvent avoir des dépendances. Examinez donc attentivement les conditions préalables.

 Sauvegardez votre installation avant d&#39;appliquer des correctifs pour éviter la perte de données. Si vous avez apporté des modifications de code personnalisé, sachez que les correctifs peuvent entrer en conflit. Appliquez les bonnes pratiques et surveillez l’impact de chaque correctif.

## Articles et vidéos connexes

* [Recherche des outils de correctifs de qualité](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html)
* [Notes de mise à jour](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* [Github pour les correctifs](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [Utilisation de l’outil de correctifs de qualité](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/usage)
* [Vidéo technique sur QPT](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/tools/quality-patch-tool)
