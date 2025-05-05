---
title: Bannières d’édition
description: Éléments visuels réutilisés pour noter une fonctionnalité ou des pages s’appliquant à une édition spécifique
source-git-commit: 066e031bd98458c8692f1cb3234ff1ecd1b99e6e
workflow-type: tm+mt
source-wordcount: '156'
ht-degree: 0%

---

# Bannières d’édition

## Fonctionnalité EE uniquement {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Fonctionnalité Adobe Commerce" src="../assets/adobe-logo.svg" width="20" height="20" /> Fonctionnalité exclusive uniquement dans Adobe Commerce (<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html?lang=fr#product-editions">En savoir plus</a>)</td></tr>
</table>

## Fonctionnalité B2B uniquement {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Fonctionnalité Adobe Commerce" src="../assets/b2b.svg" width="20" height="20" /> Fonctionnalité exclusive disponible uniquement avec <a href="https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html?lang=fr">B2B pour Adobe Commerce</a></td></tr>
</table>

## 400 problèmes {#avoid-400-error}

>[!CAUTION]
>
>Lors de l’exécution d’appels API, assurez-vous qu’un certain type de critères de recherche est utilisé. Vous pouvez également envisager d’utiliser la pagination. Si le résultat d’Adobe Commerce est trop important, la capacité d’Adobe Developer App Builder peut être atteinte et provoquer une fin inattendue du fichier. Le résultat est un résultat de réponse incorrect sous la forme d’une erreur 400.\
> Par exemple, supposons qu’il soit nécessaire de demander tous les produits actuels à Adobe Commerce. L’URL résultante ressemblerait à `{{base_url}}rest/V1/products?searchCriteria=all`. Selon la taille du catalogue renvoyé, le fichier json peut être trop volumineux pour qu’App Builder puisse le consommer. Utilisez plutôt la pagination et effectuez quelques requêtes pour éviter `Response is not valid 'message/http'.`
