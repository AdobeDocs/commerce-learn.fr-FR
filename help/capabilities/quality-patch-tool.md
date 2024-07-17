---
title: En savoir plus sur l’outil de correctif de qualité
description: Améliorez la stabilité et la sécurité de votre plateforme de commerce électronique en appliquant des correctifs de qualité. Restez à jour, résolvez les problèmes et optimisez les performances grâce à cet outil essentiel.
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Architect, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 771
last-substantial-update: 2024-07-17T00:00:00Z
jira: KT-15836
source-git-commit: 5a2a757bc97c5af25070e448abf119e2e657b6c8
workflow-type: tm+mt
source-wordcount: '518'
ht-degree: 0%

---

# A quoi vous attendre en regardant cette vidéo

Découvrez comment trier un problème, puis utiliser certaines techniques de base pour trouver un correctif de qualité pour appliquer un correctif.

## Pour qui est cette vidéo ?

* Les développeurs apprennent à trouver les problèmes et à utiliser cet outil pour appliquer des correctifs GIT aux problèmes connus

## Contenu vidéo

L’outil de correctifs de qualité est un utilitaire de ligne de commande pour Adobe Commerce et Magento Open Source. Voici ce qu’il vous permet de faire :

* Affichez des informations générales sur les derniers correctifs de qualité.
* Appliquez des correctifs de qualité à votre installation.
* Rétablir les correctifs appliqués si nécessaire

Ces correctifs sont développés par les développeurs Adobes de la communauté Magento Open Source pour améliorer la stabilité et les performances. Gardez à l’esprit qu’il n’est pas recommandé d’appliquer un grand nombre de correctifs, car cela peut compliquer les futures mises à niveau.

>[!VIDEO](https://video.tv.adobe.com/v/3431436?learn=on)

## Pourquoi utiliser l’outil de correctif de qualité ?

Vous pouvez utiliser l’outil de correctifs de qualité pour Adobe Commerce ou Magento Open Source si vous souhaitez :

Améliorez la stabilité et les performances : les correctifs de qualité résolvent les problèmes, améliorent la sécurité et optimisent votre installation.
Restez à jour : l’application de correctifs garantit que votre système est à jour et protégé.
Rétablir les modifications : si un correctif entraîne des problèmes inattendus, vous pouvez le rétablir à l’aide de l’outil. N’oubliez pas qu’il convient le mieux d’appliquer un nombre limité de correctifs afin d’éviter de compliquer les futures mises à niveau.  

## Limites ou préoccupations relatives à l’utilisation de l’outil de correctif de qualité

Bien que l’outil Correctifs de qualité offre des avantages, vous devez tenir compte de quelques points :

* Compatibilité : assurez-vous que les correctifs sont compatibles avec votre version spécifique d’Adobe Commerce ou de Magento Open Source.
* Tests : testez toujours les correctifs dans un environnement d’évaluation avant de les appliquer à la production. Des problèmes inattendus peuvent survenir.
* Dépendances des correctifs : certains correctifs peuvent dépendre d’autres. Tenez compte des conditions préalables.
* Personnalisations : si vous avez apporté des modifications au code personnalisé, des correctifs peuvent entrer en conflit. Examinez attentivement les modifications.
* Sauvegarde : sauvegardez votre installation avant d&#39;appliquer les correctifs afin d&#39;éviter toute perte de données.

Bien que l’outil Correctifs de qualité soit utile pour appliquer un nombre limité de correctifs, il n’est pas recommandé pour gérer un grand nombre de correctifs. L’application d’un trop grand nombre de correctifs peut compliquer les mises à niveau et la maintenance ultérieures. Si vous avez de nombreux correctifs à appliquer, envisagez d’autres approches ou consultez un spécialiste Magento. 

## Résumé

L’outil de correctifs de qualité permet aux plateformes de commerce électronique d’améliorer la stabilité et la sécurité en appliquant des correctifs. Ces correctifs résolvent les problèmes, améliorent les performances et optimisent le système. Le fait de conserver votre installation à jour garantit une protection contre les vulnérabilités.

Avant d’appliquer des correctifs, il est essentiel de les tester dans un environnement d’évaluation. Vérifiez la compatibilité avec votre version spécifique d’Adobe Commerce ou de Magento Open Source. Certains correctifs peuvent avoir des dépendances. Révisez donc soigneusement les conditions préalables.

 Sauvegardez votre installation avant d’appliquer des correctifs pour éviter toute perte de données. Si vous avez apporté des modifications au code personnalisé, sachez que les correctifs peuvent entrer en conflit. Suivez les bonnes pratiques et surveillez l’impact de chaque correctif.

## Articles et vidéos connexes

* [Recherche des outils de correctifs de qualité](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html)
* [Notes de mise à jour](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* [Github pour les correctifs](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [Utilisation de l’outil de correctif de qualité](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/quality-patches-tool/usage)
* [Vidéo technique sur QPT](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/tools/quality-patch-tool)