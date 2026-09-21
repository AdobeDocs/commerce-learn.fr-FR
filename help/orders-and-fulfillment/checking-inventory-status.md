---
title: 'Contrôles de l''état des stocks : développement et performance'
description: Découvrez comment évaluer si les vérifications d’inventaire en temps réel sont nécessaires dans Adobe Commerce et examiner les considérations relatives au développement et aux performances de votre boutique.
feature: Best Practices, Inventory
topic: Development, Performance
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 496
last-substantial-update: 2024-05-09
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
TQID: https://experienceleague.adobe.com/IfBm4JSpLXViUNTHo7amAL6GIYJsC4O-rdITtbqJV24
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
    internal-label: Commerce
feature_v2:
  - id: bd989d82-1e15-4534-88db-f1f51dd77ffa
    internal-label: Accounts
  - id: c1256247-af4b-46d8-9dca-0c654ecfa157
    internal-label: Order Management System
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
    internal-label: Configuration
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
    internal-label: Architecture
subfeature_v2:
  - id: b01a71b7-d17a-42b2-a9ac-af4b8d9d2ef5
    internal-label: 2FA
  - id: f56d26ed-050b-4fb7-b29b-8e6e994e80a2
    internal-label: B2B
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
    internal-label: Intermediate
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
    internal-label: Customer experience
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
    internal-label: Troubleshooting
source-git-commit: fcf0f5116ba75bd618f4ed80591ecc96132cdb45
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%
---
# Le statut de l’inventaire vérifie les considérations relatives au développement et aux performances.

L&#39;exactitude de l&#39;inventaire est une considération importante. Certaines fonctionnalités natives peuvent permettre de s’assurer que ce risque est aussi faible que possible, telles que les commandes en souffrance et la définition du seuil de rupture de stock. Ces deux sujets peuvent être lus sur [Adobe Experience League](https://experienceleague.adobe.com/en/docs/commerce-admin/inventory/configuration/backorders) pour plus d’explications.

Il existe des projets et des cas d’utilisation pour lesquels des vérifications de statut de l’inventaire en temps réel sont demandées pour une boutique Adobe Commerce. Ce tutoriel permet à insight de gérer cette conversation en tenant compte des considérations de développement et de performances.

## Valider si cette requête est nécessaire

Préparez-vous à discuter de la demande avec le plus d’informations possible. La chose la plus importante à faire est de vérifier que les fonctionnalités natives ne sont pas acceptables pour ce projet. Découvrez le raisonnement derrière cette demande pour vérifier que les fonctionnalités natives d’Adobe Commerce ne répondent pas à cette demande.

Le coût du développement, du test et de la maintenance de cette fonctionnalité constitue un autre aspect à prendre en compte. L&#39;opinion d&#39;un intervenant ne rend pas nécessairement quelque chose obligatoire. La validation des stocks en dehors des fonctionnalités de base d’Adobe Commerce entraîne des coûts. Ces coûts se présentent sous la forme d’une dette technique, de davantage de tests et de validations, ainsi que de documentation d’utilisation et de documents annexes pour son architecture.

## Déterminer ce qui constitue une cadence acceptable de mise à jour de l’inventaire

Essayez de tenir compte des vérifications d’inventaire et de la façon dont elles sont effectuées en 3 approches. Chacun présente des avantages et des limites. Ils deviennent également plus complexes et nécessitent davantage de tests et de réflexion pour la gestion des erreurs. N’oubliez pas que lorsque vous décidez de mettre en œuvre une solution personnalisée, des responsabilités et des considérations supplémentaires s’ajoutent. Par exemple, un processus de secours, la surveillance, les tests et le dépannage, qui incombent à l’équipe de développement. Parmi les bons éléments à inclure, citons la nouvelle documentation d’assistance, la formation et la surveillance pour s’assurer que l’équipe de développement peut prendre en charge l’ensemble de la fonctionnalité. Par conséquent, l’équipe de développement est propriétaire du processus et n’utilise plus les fonctionnalités natives fournies par l’application Adobe Commerce principale. La prise en charge d’Adobe ne peut pas vous aider à atteindre ce niveau de personnalisation.

La première approche consiste à utiliser la fonctionnalité native. L’utilisation des fonctionnalités natives est la moins risquée et présente de nombreux avantages. En adoptant cette approche, vous pouvez vous fier à toute la documentation et aux tutoriels existants fournis par Adobe Commerce pour l’utilisation de la fonctionnalité. La gestion des stocks comporte de nombreux aspects, alors utilisez d&#39;abord ce qui est fourni avec l&#39;application. Cependant, il existe des cas d’utilisation où les données trouvées dans Commerce au moment de la commande ne sont pas exactes. Un exemple de la manière dont les données sont désynchronisées est que les ventes sont autorisées en dehors de l’application Adobe Commerce directement dans le système Order Management. En effet, pour s’assurer que les niveaux d’inventaire exacts sont représentés dans Adobe Commerce, une intégration est nécessaire pour que les informations d’Adobe Commerce soient aussi précises que possible. Si la vente excessive n’est pas acceptable, l’ajout d’un seuil de rupture de stock est une bonne méthode pour arrêter la vente d’articles avant d’atteindre zéro. La fonctionnalité de synchronisation native d’Adobe Commerce est limitée à 1 fois par jour au maximum. Cette fréquence est suffisante pour certains cas d’utilisation, mais pas suffisamment pour d’autres. Veuillez lire [Importation et exportation planifiées](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export) pour plus d’informations.

La deuxième approche est `near real-time`. En temps quasi réel, utilise toujours la fonctionnalité native. Toutefois, cela inclut du travail supplémentaire pour fournir une intégration qui alimente fréquemment Commerce afin de mettre à jour son inventaire selon un calendrier précis. Par exemple, toutes les heures. Cette option nécessite de réfléchir au fonctionnement d’une intégration. Toutefois, l’utilisation de l’« api en masse » et de logiciels intermédiaires pour effectuer la transformation des données et les pousser vers Commerce est une excellente approche. Envisagez d’utiliser Adobe App Builder ou des plateformes similaires pour effectuer l’essentiel du travail et transmettre les informations à Adobe Commerce plus fréquemment.

La troisième approche, la plus complexe et celle qui implique le plus de risques et de responsabilités, consiste à effectuer des contrôles d’inventaire en temps réel sur une API ou une source de données externe. Effectuer une vérification d&#39;inventaire en temps réel sur un système externe est risqué et comporte plusieurs autres éléments à prendre en compte. Voici un petit ensemble d’autres éléments qui doivent être évalués :

* Le système externe peut-il accepter des requêtes REST ou GraphQL ?
* Existe-t-il des limites au point d’entrée, telles que X nombre de requêtes par minute, qui ne coïncident pas avec le trafic du site web ?
* Qu’advient-il du temps de réponse sous charge ?
* Que se passe-t-il lorsque les temps de réponse sont longs ? Y mettez-vous fin automatiquement et utilisez une option de secours telle que l’inventaire natif ?
* Quel type de surveillance est disponible pour s’assurer que les requêtes API se trouvent dans les limites de tolérance ?

## Considérations relatives à la gestion des stocks non natifs

Veillez à ce que les personnalisations restent aussi simples que possible.
Dans quelle mesure l’organisation de l’inventaire peut-elle être plate ? S’agit-il de 1 SKU et de la quantité totale de stock disponible ? OU d’autres attributs doivent-ils être pris en compte ?

Si les informations d’inventaire sont assez uniformes (par exemple, un SKU et la quantité totale disponible), les options relatives au temps quasi réel sont étendues. Le concept de temps quasi réel signifie qu’il existe une opération en arrière-plan qui collecte l’inventaire à partir de la source, puis renseigne un moteur de stockage pour être utilisé afin de répondre à la requête. Pour cela, vous pouvez utiliser des éléments tels que Redis, Mongo ou d’autres bases de données non relationnelles. Ces options sont rapides et fonctionnent parfaitement pour les paires clé/valeur. Si les données sont un peu plus complexes, l’utilisation d’une base de données de relations, à l’intérieur ou à l’extérieur de l’application commerciale, est requise. En le déchargeant de la base de données commerciale, vous isolez l’application commerciale principale de ces transactions. Un autre ensemble d’avantages permet d’économiser l’E/S de l’application de commerce, CPU, la RAM et d’autres de l’utilisation. Pour économiser des ressources sur les serveurs d’applications Adobe Commerce, tirez parti des nouvelles API pour extraire les données du stockage hors site. Ce processus nécessite un middleware pour aider à transformer toutes les données. Assurez-vous ensuite que l’application appelante peut obtenir le résultat attendu. En utilisant Adobe App Builder avec un maillage API, les données peuvent être transformées et renvoyées correctement formatées.

L’utilisation d’Adobe App Builder avec un maillage API est également une excellente option lorsqu’il existe plusieurs sources d’inventaire.


## Retirer la logique d&#39;exécution du processus

Adobe Developer App Builder fournit un framework d’extensibilité tiers unifié permettant d’intégrer et de créer des expériences personnalisées pour étendre les solutions Adobe. Adobe Commerce peut utiliser Adobe Developer App Builder. Cette approche est un excellent cas d’utilisation pour étendre certaines fonctionnalités qui se produisent normalement dans l’application principale et la déplacer hors site. En supprimant certaines fonctionnalités de l’application Commerce, vous réduisez le nombre de modules et la complexité de l’application Commerce. En retour, un nombre réduit de personnalisations en cours de processus réduit la complexité de la mise à niveau et de la maintenance.

Pour vous inspirer de la manière dont cette tâche est accomplie, l’équipe d’Adobe a créé une documentation qui est une excellente source d’inspiration et qui fournit des exemples de code de travail. Lorsqu’un acheteur ajoute un produit au panier, un système de gestion des stocks tiers vérifie si l’article est en stock. Si c’est le cas, autorisez l’ajout du produit. Sinon, affichez un message d’erreur. Pour obtenir des exemples de code et des informations supplémentaires, accédez à [Cas d’utilisation de Webhook](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart).

## Quand effectuer des vérifications d’inventaire

C’est à l’entité commerciale qu’il revient de vérifier si l’inventaire est toujours disponible, à l’architecte de logiciel, avec la contribution d’autres parties prenantes clés. Lors de l’ajout d’un article au panier et de la saisie du workflow de passage en caisse, vous pouvez notamment choisir quelques moments appropriés. Tous les autres événements ajoutent de la charge aux systèmes principaux lorsque cela n’est pas nécessaire. Gardez à l’esprit que l’objectif est de détecter un problème d’inventaire uniquement lorsqu’il est primordial. Examinez attentivement les autres vérifications qui ont une incidence sur l’objectif global des vérifications de l’état des stocks et n’autorisez-les que si les parties prenantes sont conscientes du risque potentiel de charge supplémentaire.

## Recherche de la source de votre inventaire

Il faut mener une enquête approfondie sur la source de l&#39;inventaire externe. Les éléments à évaluer sont les options d’API disponibles, la prise en charge de GraphQL et les temps de réponse attendus. Si la source d’inventaire a une bande passante de connexion limitée ou n’a jamais été conçue pour être utilisée dans une requête en temps réel, la possibilité d’utilisation est exclue et l’architecte doit plutôt envisager une utilisation en temps quasi réel. Si les délais de requête de l’API dépassent les paramètres définis, cela exclut que cette option soit viable. Un exemple de ce comportement est que les réponses de l’api sont de 200 ms pour les requêtes uniques, mais qu’elles atteignent 500 à 900 ms sous une charge modérée. Cette situation s’aggrave avec une charge plus importante et exclut la disponibilité des appels d’inventaire actifs.

Veillez à tester les temps de réponse de l’api avec des requêtes simples et avec un volume élevé similaire au trafic attendu sur le site web en ligne. N’oubliez pas de tester simultanément toutes les zones de Commerce pour simuler des scénarios réels. Si des appels d’inventaire actif se produisent sur les pages de produits, dans le panier et lors du passage en caisse, le test de chargement doit simuler tous ces éléments simultanément afin d’imiter le comportement réel du client.

## Options de secours

Si la source d’inventaire est en panne et que la surveillance est disponible, il est recommandé d’utiliser la fonctionnalité native d’Adobe Commerce. Cependant, avec une surveillance appropriée, l’expérience client peut changer de manière dynamique pour refléter la perte des contrôles d’inventaire en temps réel. Cela signifie qu&#39;une vente ou un événement est annulé de manière anticipée ou retiré de l&#39;affichage pour éviter les ventes excessives. Discutez du plan de secours avec le propriétaire du magasin afin que tout le monde comprenne le processus automatique qui prend le relais si la source de l&#39;inventaire tombe en panne.

## Conclusion

La décision d&#39;effectuer des vérifications d&#39;inventaire en temps réel est importante. Assurez-vous que le propriétaire du site web, l’équipe de développement et d’autres personnes sont parfaitement informés et conscients de tous les gains et pièges potentiels incombe au développeur ou à l’architecte. En fournissant un plan réfléchi qui couvre les raisons et un processus de secours est la clé du succès.

Les contrôles d’inventaire en direct peuvent être effectués, mais nécessitent des recherches et une réflexion sur les tests et la validation pendant le cycle d’assurance qualité. Les tests de chargement et les tests automatisés de bout en bout permettent de s’assurer que tous les problèmes potentiels sont détectés et triés.

Si la surveillance détecte des appels ayant échoué ou des temps de réponse lents, effectuez les actions nécessaires pour garder le site en ligne et minimiser l’irritation des clients. Les options de secours vont de l’utilisation de fonctionnalités natives à la désactivation de promotions, la notification de l’équipe de développement ou le routage des requêtes vers un système principal secondaire. La mise en œuvre du mécanisme de secours doit être planifiée avec autant de soin que l’intégration réelle, car chaque système rencontre des problèmes à un moment donné. Tout élément automatisé ou nécessitant une action manuelle doit être clairement documenté.
