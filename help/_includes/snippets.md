---
title: Bannières d’édition
description: Réutilisation des éléments visuels pour noter la fonctionnalité ou les pages s’appliquant à une édition spécifique
source-git-commit: 180bfd303df77d95c88fa518253ddff6a67c76d4
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 0%

---

# Bannières d’édition

## Fonctionnalité EE uniquement {#ee-feature}

<table style="border:1px solid red">
<tr><td><img alt="Fonctionnalité Adobe Commerce" src="../assets/adobe-logo.svg" width="20" height="20" /> Fonctionnalité exclusive uniquement dans Adobe Commerce (<a href="https://experienceleague.adobe.com/docs/commerce-admin/user-guides/home.html?lang=fr#product-editions">En savoir plus</a>)</td></tr>
</table>

## Fonctionnalité B2B uniquement {#b2b-feature}

<table style="border:1px solid green">
<tr><td><img alt="Fonctionnalité Adobe Commerce" src="../assets/b2b.svg" width="20" height="20" /> Fonction exclusive disponible uniquement avec <a href="https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html?lang=fr">B2B pour Adobe Commerce</a></td></tr>
</table>

## 400 événements {#avoid-400-error}

>[!CAUTION]
>
>Lors de l’exécution d’appels d’API, assurez-vous qu’un certain type de searchCriteria est utilisé. Vous pouvez également envisager d’utiliser la pagination. Si le résultat d’Adobe Commerce est trop volumineux, la capacité d’Adobe Developer App Builder peut être atteinte et entraîner une fin inattendue du fichier. Le résultat est un résultat de réponse incorrect résultant d’une erreur 400.\
> Supposons, par exemple, qu’il soit nécessaire de demander tous les produits actuels à Adobe Commerce. L’URL qui en résulte ressemble à `{{base_url}}rest/V1/products?searchCriteria=all`. Selon la taille du catalogue renvoyé par le client, le fichier json peut être trop volumineux pour être utilisé par App Builder. Utilisez plutôt la pagination et effectuez quelques requêtes pour éviter les `Response is not valid 'message/http'.`.

## PaaS et Commerce Cloud uniquement {#only-for-on-prem-commerce-cloud}


**Ces informations s’appliquent à :**

| On-Prem | Adobe Commerce Cloud | Adobe Commerce as a Cloud Service | Adobe Commerce Optimizer |
| --- | --- | --- | --- |
| Oui | Oui | Non | Non |
