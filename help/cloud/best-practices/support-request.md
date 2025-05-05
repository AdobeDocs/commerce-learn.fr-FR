---
title: Créer une demande d’assistance efficace
description: Découvrez comment créer un ticket d’assistance afin d’optimiser l’efficacité de la requête.
feature: Best Practices, Customer Service, Support
topic: Commerce
role: Admin, Developer, User
level: Beginner, Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-08-23T00:00:00Z
jira: KT-15165
source-git-commit: b3ec1230e5efa198d71e1a5c0756a47666065117
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 0%

---


# Demandes d’assistance effectives

Lors de la création d’un ticket d’assistance, il est important de l’envoyer par les canaux appropriés, de fournir des informations précises et détaillées sur le problème, de sélectionner la bonne organisation et la raison de contact, de choisir le produit et la version appropriés, de consulter les articles suggérés pour les solutions potentielles, de vérifier toutes les informations avant l’envoi, de suivre la progression du ticket et d’engager une conversation avec l’équipe d’assistance, de marquer le ticket comme résolu lorsque le problème est résolu et d’ouvrir un ticket de relance si une assistance supplémentaire si nécessaire. &#x200B; N’oubliez pas d’envoyer le ticket par les canaux appropriés, de fournir des informations précises et détaillées, de sélectionner l’organisation et la raison de contact appropriées, de choisir le produit et la version appropriés, de revoir les articles suggérés, de vérifier toutes les informations avant l’envoi, de suivre l’avancement du ticket, d’engager la conversation avec l’équipe d’assistance, de marquer le ticket comme résolu lorsque le problème est résolu et d’ouvrir un ticket de relance si nécessaire. &#x200B;

## Inclure les journaux ou les captures d’écran

Plusieurs journaux sont utiles en fonction du problème en question. Si possible, recherchez une section du journal et incluez-la dans la demande de prise en charge. Cet extrait de journal permet aux ingénieurs de comprendre les erreurs affichées et accélère le processus de triage. N’oubliez pas que chaque serveur d’applications comporte des journaux individuels, qui sont alimentés en New Relic sous la forme d’un agrégat.  Cela signifie que vous pouvez rechercher toutes les erreurs dans un emplacement au lieu de lire les fichiers journaux individuels de chaque serveur d’applications. Comme dernière option, si une entrée est trouvée dans New Relic, une capture d’écran peut également être utilisée comme informations supplémentaires et contribuer à accélérer le processus, mais il est préférable d’avoir une instruction NRQL.

## Assurez-vous que le fuseau horaire est référencé.

Veiller à ce que, lorsque des personnes contribuent à un problème d’assistance, leur rappelle de clarifier leur fuseau horaire lorsqu’elles font référence à un incident. Le référencement d’un fuseau horaire garantit que lorsque l’équipe d’ingénierie de l’assistance effectue des recherches sur les journaux et les événements, elle peut se concentrer sur les périodes réellement pertinentes. N’oubliez pas que quelqu’un peut avoir plusieurs heures d’avance ou de retard sur les temps de journalisation du serveur.

## Décrire le triage initial et les résultats

En discutant et en documentant toutes les étapes de triage entreprises jusqu&#39;à présent, les ingénieurs du support valident les suppositions initiales. Si les étapes de triage et les résultats sont fournis, cela peut accélérer le processus global. Cela permettra également de réduire les doublons d’efforts et, à terme, de fournir un moyen de documenter les résultats avec un niveau de validation.

## Liens vers les rapports New Relic ou instructions NRQL

La plupart des problèmes d’Adobe Commerce sont traçables via New Relic. En examinant les tableaux de bord New Relic ou les instructions NRQL personnalisées, vous obtenez des informations sur l’origine de certains problèmes. Ces mêmes tableaux de bord et requêtes New Relic personnalisées peuvent être partagés. En fournissant ces liens dans le dossier d&#39;assistance, les ingénieurs sont en mesure de voir exactement ce qu&#39;est le reporter.

>[!MORELIKETHIS]
> 
> - [Guide de l’utilisateur de l’aide Adobe Commerce](https://experienceleague.adobe.com/fr/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide){target="_blank"}
