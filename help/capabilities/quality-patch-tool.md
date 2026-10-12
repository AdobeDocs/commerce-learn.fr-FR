---
title: Outil de correctifs de qualité
description: Découvrez comment utiliser l’outil de correctifs de qualité lors du diagnostic d’un problème, de la recherche d’une solution et de l’application d’un correctif trouvé dans la liste des correctifs disponibles.
feature: Cloud, Configuration, Logs, System, Tools and External Services
topic: Architecture, Commerce, Development
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Technical Video
duration: 903
last-substantial-update: 2024-07-17T00:00:00.000Z
jira: KT-15836
exl-id: 16710f27-1232-4c6a-aac3-9838308d1267
TQID: 'https://experienceleague.adobe.com/GpcJqSCn3XqLZtm-QdQ-ka9c-RdkG-C6Hd3FpXrh8-I'
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
    internal-label: Security
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
  - id: f42e0a1a-0d79-488d-a83f-f2c30672b137
    internal-label: Reporting
  - id: 5a951749-fac9-5bc7-9a98-ebe4ff066437
    internal-label: Cloud
  - id: 8d0b446f-5b16-5a10-b272-01143504a11c
    internal-label: System
  - id: 125c1f49-aefd-5f34-a252-288937f95f6b
    internal-label: Marketing Tools
subfeature_v2:
  - id: 3c398179-d35a-51ba-b317-6c5b95feef5e
    internal-label: Logs
  - id: 618ab558-d6ad-5352-99d6-d5702c6fdf80
    internal-label: Tools and External Services
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
    internal-label: Reporting
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
    internal-label: Security
source-git-commit: 71835f0240311d4b26ec8e0d9bbdf005f9887579
workflow-type: tm+mt
source-wordcount: '592'
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

Bien que l&#39;outil Correctifs de la qualité soit utile pour appliquer un nombre limité de correctifs, il n&#39;est pas recommandé pour gérer un grand volume de correctifs. L’application d’un trop grand nombre de correctifs peut compliquer les futures mises à niveau et maintenance. Si vous devez appliquer de nombreux correctifs, envisagez d’autres approches ou consultez un spécialiste de Magento. 

## Résumé

L’outil de correctifs de qualité permet aux plateformes d’e-commerce d’améliorer la stabilité et la sécurité en appliquant des correctifs. Ces correctifs résolvent les problèmes, améliorent les performances et optimisent le système. La mise à jour de votre installation garantit une protection contre les vulnérabilités.

Avant d’appliquer des correctifs, il est essentiel de les tester dans un environnement d’évaluation. Vérifiez la compatibilité avec votre version spécifique d’Adobe Commerce ou de Magento Open Source. Certains correctifs peuvent avoir des dépendances. Examinez donc attentivement les conditions préalables.

 Sauvegardez votre installation avant d&#39;appliquer des correctifs pour éviter la perte de données. Si vous avez apporté des modifications de code personnalisé, sachez que les correctifs peuvent entrer en conflit. Appliquez les bonnes pratiques et surveillez l’impact de chaque correctif.

## Articles et vidéos connexes

* [Recherche dans les outils de correctifs de qualité](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html?lang=fr)
* [Notes de mise à jour](https://experienceleague.adobe.com/fr/docs/commerce-operations/tools/quality-patches-tool/release-notes)
* [Github pour les correctifs](https://github.com/magento/quality-patches/blob/master/patches/os/)
* [Utilisation de l’outil de correctifs de la qualité](https://experienceleague.adobe.com/fr/docs/commerce-operations/tools/quality-patches-tool/usage)
* [Vidéo technique sur QPT](https://experienceleague.adobe.com/fr/docs/commerce-learn/tutorials/tools/quality-patch-tool)
