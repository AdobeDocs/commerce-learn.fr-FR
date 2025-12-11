---
title: Le statut de l’inventaire vérifie les considérations relatives au développement et aux performances.
description: Découvrez quelques idées et points à prendre en compte pour effectuer des vérifications de statut de l’inventaire pour Adobe Commerce.
feature: Best Practices, Inventory
topic: Development, Performance
old-role: Architect, Developer
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-05-09T00:00:00Z
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1932'
ht-degree: 0%

---

# Le statut de l’inventaire vérifie les considérations relatives au développement et aux performances.

L&#39;exactitude de l&#39;inventaire est un facteur très important. Certaines fonctionnalités natives peuvent permettre de s’assurer que ce risque est aussi faible que possible, telles que les commandes en souffrance et la définition du seuil de rupture de stock. Vous pouvez lire ces deux rubriques sur [Experience League](https://experienceleague.adobe.com/fr/docs/commerce-admin/inventory/configuration/backorders) pour plus d’explications.

Il existe des projets et des cas d’utilisation pour lesquels des vérifications de statut de l’inventaire en temps réel sont demandées pour une boutique Adobe Commerce. Ce tutoriel permet à insight de gérer cette conversation en tenant compte des considérations de développement et de performances.

## Validez si cette requête est absolument nécessaire.

Préparez-vous à engager la conversation avec le plus d’informations possible. La chose la plus importante à faire est de vérifier que les fonctionnalités natives ne sont pas acceptables pour ce projet. Découvrez le raisonnement derrière cette demande pour vérifier que les fonctionnalités natives d’Adobe Commerce ne répondent pas à cette demande.

Une autre considération est le coût de développement, de test et de maintenance de cette fonctionnalité. Ce n&#39;est pas parce qu&#39;un intervenant pense que c&#39;est nécessaire que c&#39;est une exigence. La validation de l’inventaire en dehors des fonctionnalités de base d’Adobe Commerce entraîne des coûts. Ces coûts représentent une dette technique, davantage de tests et de validations, ainsi que de la documentation d’utilisation et des documents annexes pour son architecture.

## Déterminer ce qui constitue une cadence acceptable de mise à jour de l’inventaire

Essayez de tenir compte des vérifications d’inventaire et de la façon dont elles sont effectuées en 3 approches. Chacun présente des avantages et des limites. Ils augmentent également en complexité et nécessitent davantage de tests et de réflexion pour la gestion des erreurs. N’oubliez pas que lorsque vous décidez d’emprunter un itinéraire personnalisé, il y a des responsabilités et des considérations supplémentaires. Par exemple, un processus de secours, la surveillance, les tests et le dépannage sont du ressort de l’équipe de développement. Parmi les bons éléments à inclure, citons la nouvelle documentation d’assistance, la formation et la surveillance pour s’assurer que l’équipe de développement peut prendre en charge l’ensemble de la fonctionnalité. Un autre effet secondaire est que l’équipe de développement possède complètement le processus et n’utilise plus les fonctionnalités natives fournies par l’application Adobe Commerce principale. La prise en charge d’Adobe ne peut pas vous aider à atteindre ce niveau de personnalisation.

La première approche consiste à utiliser la fonctionnalité native. L’utilisation des fonctionnalités natives est la moins risquée et présente de nombreux avantages. En adoptant cette approche, vous pouvez vous fier à toute la documentation et aux tutoriels existants fournis par Adobe Commerce pour l’utilisation de la fonctionnalité. La gestion des stocks comporte de nombreux aspects. Il faut donc d&#39;abord tenir compte de ce qui accompagne l&#39;application. Cependant, il existe des cas d’utilisation où les données trouvées dans Commerce au moment de la commande peuvent ne pas être entièrement exactes. Un exemple de la manière dont les choses peuvent se désynchroniser est que les ventes sont autorisées en dehors de l’application Adobe Commerce directement dans le système de gestion des commandes. En effet, pour s’assurer que les niveaux d’inventaire exacts sont représentés dans Adobe Commerce, il faudrait une sorte d’intégration pour que les informations d’Adobe Commerce soient aussi précises que possible. Si la vente excessive n’est pas acceptable, l’ajout d’un seuil de rupture de stock est une bonne méthode pour arrêter la vente d’articles avant d’atteindre zéro. La fonctionnalité de synchronisation native d’Adobe Commerce est limitée à 1 fois par jour au maximum. Cela est suffisant pour certains cas d’utilisation, mais peut ne pas être suffisamment fréquent pour d’autres. Veuillez lire [Importation et exportation planifiées](https://experienceleague.adobe.com/fr/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export) pour plus d’informations.

La deuxième approche serait `near real-time`. En temps quasi réel, utilise toujours la fonctionnalité native. Toutefois, cela inclut du travail supplémentaire pour fournir une intégration qui alimente fréquemment Commerce afin de mettre à jour son inventaire selon un calendrier précis. Par exemple, toutes les heures. Cette option nécessite de réfléchir à la manière dont une intégration peut fonctionner, mais l’utilisation de l’« api en masse » et de logiciels intermédiaires pour effectuer la transformation des données et les pousser vers Commerce est une approche attrayante. Envisagez d’utiliser Adobe App Builder ou des plateformes similaires pour effectuer l’essentiel du travail et transmettre les informations à Adobe Commerce plus fréquemment.

La troisième approche, la plus complexe et celle qui implique le plus de risques et de responsabilités, consiste à effectuer des contrôles d’inventaire en temps réel sur une API ou une source de données externe. Effectuer une vérification d&#39;inventaire en temps réel sur un système externe est risqué et comporte plusieurs autres éléments à prendre en compte. Voici un petit ensemble d’autres éléments qui doivent être évalués :

* Le système externe peut-il accepter des requêtes REST ou GraphQL ?
* y a-t-il des limites au point d’entrée, telles que X nombre de requêtes par minute qui peuvent ne pas correspondre au trafic du site web ?
* Qu’advient-il du temps de réponse sous charge ?
* Que se passe-t-il lorsque les temps de réponse sont longs ? Y mettez-vous fin automatiquement et utilisez une option de secours telle que l’inventaire natif ?
* Quel type de surveillance est disponible pour s’assurer que les requêtes API se trouvent dans les limites de tolérance ?

## Considérations relatives à la gestion des stocks non natifs

Veillez à ce que les personnalisations restent aussi simples que possible.
L’organisation de l’inventaire peut-elle être plate ? S’agit-il d’un seul SKU et de la quantité totale de stock disponible ? OU d’autres attributs doivent-ils être pris en compte ?

Si les informations d’inventaire sont assez uniformes (par exemple, un SKU et la quantité totale disponible), les options relatives au temps quasi réel sont étendues. Le concept de temps quasi réel signifie qu’il existe une opération en arrière-plan qui collecte l’inventaire à partir de la source, puis renseigne un moteur de stockage pour être utilisé afin de répondre à la requête. Pour cela, vous pouvez utiliser des éléments tels que Redis, Mongo ou d’autres bases de données non relationnelles. Ces options sont intéressantes car elles sont très rapides et fonctionnent parfaitement pour les paires clé/valeur. Si les données sont un peu plus complexes, l’utilisation d’une base de données de relations, à l’intérieur ou à l’extérieur de l’application commerciale, est probablement nécessaire. En le déchargeant de la base de données commerciale, vous isolez l’application commerciale principale de ces transactions. Un autre ensemble d’avantages permet d’économiser l’E/S de l’application de commerce, CPU, la RAM et d’autres de l’utilisation. Au lieu d’utiliser les ressources des serveurs d’applications Adobe Commerce, tirez parti des nouvelles API pour extraire les données du stockage hors site.  Un middleware sera probablement nécessaire pour aider à transformer toutes les données. Assurez-vous ensuite que l’application appelante peut obtenir le résultat attendu. En utilisant Adobe App Builder avec un maillage API, les données peuvent être transformées et renvoyées correctement formatées.

L’utilisation d’Adobe App Builder avec un maillage API est également une excellente option lorsqu’il existe plusieurs sources d’inventaire.


## Déplacement de la logique d’exécution vers un emplacement hors processus

Adobe Developer App Builder fournit un framework d’extensibilité tiers unifié permettant d’intégrer et de créer des expériences personnalisées pour étendre les solutions Adobe. Adobe Commerce peut utiliser Adobe Developer App Builder. Ce serait un excellent cas d’utilisation pour étendre certaines fonctionnalités qui se produisent normalement dans l’application principale et la déplacer hors site. En supprimant certaines fonctionnalités de l’application Commerce, vous réduisez le nombre de modules et la complexité de l’application Commerce. En retour, un nombre réduit de personnalisations en cours de processus réduit la complexité de la mise à niveau et de la maintenance.

Pour vous inspirer de la manière dont cela pourrait être réalisé, l’équipe d’Adobe a créé une documentation qui peut être une excellente source d’inspiration et fournir des exemples de code de travail. Lorsqu’un acheteur ajoute un produit au panier, un système de gestion des stocks tiers vérifie si l’article est en stock. Si c’est le cas, autorisez l’ajout du produit. Sinon, affichez un message d’erreur.  Pour obtenir des exemples de code et des informations supplémentaires, accédez à [Cas d’utilisation de Webhook](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart).

## Quand effectuer des vérifications d’inventaire

Le moment où il faudra vérifier si l&#39;inventaire est toujours disponible reviendra éventuellement à l&#39;acteur commercial, l&#39;architecte logiciel avec l&#39;apport d&#39;autres acteurs clés. Voici quelques moments appropriés à inclure lors de l’ajout d’un article au panier et lors de la saisie du workflow de passage en caisse. Tous les autres événements ajoutent simplement de la charge aux systèmes principaux lorsque cela n’est pas nécessaire. Gardez à l’esprit que l’objectif est de détecter un problème d’inventaire uniquement lorsqu’il est primordial. D’autres vérifications peuvent être utiles, mais si elles peuvent avoir un impact sur l’objectif global des vérifications de statut des stocks, elles doivent être soigneusement examinées et autorisées uniquement si les parties prenantes de l’entreprise ou d’autres sont conscientes du risque potentiel de charge supplémentaire sur les systèmes principaux.

## Recherche de la source de l’inventaire

Il faut mener une enquête approfondie sur la source de l&#39;inventaire externe. Les éléments à évaluer sont les options d’API disponibles, la prise en charge de GraphQL et les temps de réponse attendus. Si la source d’inventaire a une bande passante de connexion limitée ou n’a jamais été conçue pour être utilisée dans une requête en temps réel, la possibilité d’utilisation est exclue et l’architecte doit plutôt envisager une utilisation en temps quasi réel.  Si les délais de requête de l’API dépassent les paramètres définis, cela exclura également qu’il s’agisse d’une option viable.  Par exemple, les réponses de l’api sont 200MS® pour une requête unique, mais elles atteignent 500 à 900MS® avec une charge modérée.  La situation empirerait probablement avec une charge plus importante et exclurait la disponibilité des appels d’inventaire dynamique.

Veillez à tester les temps de réponse de l’api avec des requêtes simples et avec un volume élevé similaire au trafic attendu sur le site web en ligne. N’oubliez pas de tester simultanément toutes les zones de Commerce pour simuler des scénarios réels. S’il est attendu qu’un appel d’inventaire actif se produise sur les pages de produits, lors de l’affichage et de la modification d’un panier et lors du passage en caisse, le test de chargement doit simuler tous ces éléments en même temps pour imiter le comportement réel du client et les attentes d’un magasin.

## Options de secours

SI la source d’inventaire est en panne et que la surveillance est disponible, il est recommandé d’utiliser la fonctionnalité native d’Adobe Commerce. Cependant, avec une surveillance appropriée, l’expérience client peut changer de manière dynamique pour refléter la perte des contrôles d’inventaire en temps réel. Cela peut signifier qu&#39;une vente ou un événement est annulé de manière anticipée ou retiré de l&#39;affichage pour éviter les ventes excessives. Il faut tenir compte des attentes à l&#39;égard de ce qu&#39;il faut faire lorsque la source d&#39;inventaire est en panne pour quelque raison que ce soit et en discuter avec le propriétaire du magasin afin que tout le monde soit au courant de tout processus automatique qui prend le relais dans ces circonstances malheureuses.

## Conclusion

La décision d&#39;effectuer des vérifications d&#39;inventaire en temps réel est importante. Assurez-vous que le propriétaire du site web, l’équipe de développement et d’autres personnes sont parfaitement informés et conscients de tous les gains et pièges potentiels incombe au développeur ou à l’architecte. En fournissant un plan réfléchi qui couvre les raisons et un processus de secours est la clé du succès.

Les contrôles d’inventaire en direct peuvent être effectués, mais nécessitent des recherches et une réflexion sur les tests et la validation pendant le cycle d’assurance qualité. Les tests de chargement et les tests automatisés de bout en bout permettent de s’assurer que tous les problèmes potentiels sont détectés et triés.

En envisageant les actions à effectuer si la surveillance détecte une tendance d’échecs d’appels au système externe ou si les temps de réponse sont supérieurs aux seuils acceptables, le site peut rester en ligne et offrir le moins d’irritation possible aux clients. Les options de secours peuvent être aussi simples que de désactiver les contrôles d’inventaire externe et d’utiliser la fonctionnalité native, ou peuvent être aussi avancées que de désactiver une promotion, d’informer l’équipe de développement des problèmes qui se produisent ou même de réacheminer les requêtes vers un deuxième ou un troisième système principal, le cas échéant. La manière dont le mécanisme de secours est exercé doit être simplement considérée comme la mise en œuvre réelle, car chaque intégration présente des problèmes à un moment donné. Tout élément automatisé ou nécessitant une action manuelle doit être clairement documenté.
