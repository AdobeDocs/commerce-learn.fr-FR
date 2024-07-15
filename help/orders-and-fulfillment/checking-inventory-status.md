---
title: Considérations sur le développement et les performances des contrôles de l’état du stock
description: Découvrez quelques idées et considérations à prendre en compte pour effectuer des vérifications d’état de stock pour Adobe Commerce.
feature: Best Practices, Inventory
topic: Development, Performance
role: Architect, Developer
level: Intermediate, Experienced
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-05-09T00:00:00Z
jira: KT-15462
exl-id: bd2be562-5738-4398-8afb-2faeb0ba6b83
source-git-commit: 84f9139181bf57d967494da97d4d27d297513c31
workflow-type: tm+mt
source-wordcount: '1932'
ht-degree: 0%

---

# Considérations sur le développement et les performances des contrôles de l’état du stock

La précision de l’inventaire est une considération très importante. Certaines fonctionnalités natives peuvent aider à s’assurer que ce risque est aussi bas que possible, comme les commandes en amont et la définition du seuil d’absence du stock. Ces deux rubriques peuvent être lues sur [Experience League](https://experienceleague.adobe.com/en/docs/commerce-admin/inventory/configuration/backorders) pour plus d&#39;informations.

Il existe des projets et des cas d’utilisation pour lesquels des vérifications d’état du stock en temps réel sont demandées pour un magasin Adobe Commerce. Ce tutoriel fournit des informations sur la gestion de cette conversation avec des considérations de développement et de performances.

## Valider si cette requête est absolument nécessaire

Préparez-vous à engager la conversation avec le plus d&#39;informations possible. La chose la plus importante à faire est de vérifier que les fonctionnalités natives ne sont pas acceptables pour ce projet. Trouvez le motif derrière cette requête pour vérifier que les fonctionnalités natives d’Adobe Commerce ne parviennent pas à satisfaire cette requête.

Autre point à prendre en compte : le coût de développement, de test et de maintenance de cette fonctionnalité. Ce n&#39;est pas parce que certaines parties prenantes pensent que c&#39;est nécessaire que c&#39;est nécessaire. La validation de l’inventaire en dehors des fonctionnalités de base d’Adobe Commerce entraîne des coûts. Ces coûts s’accompagnent d’une dette technique, d’un plus grand nombre de tests et de validations, ainsi que de documents d’utilisation et de documents annexes pour son architecture.

## Déterminer ce qui est une cadence de mise à jour de stock acceptable

Essayez de tenir compte des vérifications d’inventaire et de la manière dont elles sont effectuées en 3 approches. Chacun a ses avantages et ses limites. Elles augmentent également la complexité et nécessitent davantage de tests et de réflexion pour la gestion des erreurs. Souvenez-vous que lorsque vous décidez d’emprunter un itinéraire personnalisé, il y a des responsabilités et des considérations supplémentaires. Voici quelques exemples : un processus de secours, une surveillance, des tests et un dépannage pour l’équipe de développement. Voici quelques bons éléments à inclure : documentation, formation et surveillance à l’appui, afin que l’équipe de développement puisse prendre en charge l’ensemble de la fonctionnalité. Autre effet secondaire : l’équipe de développement est entièrement responsable du processus et n’exploite plus les fonctionnalités natives fournies par l’application Adobe Commerce principale. La prise en charge des Adobes ne peut pas faciliter ce niveau de personnalisation.

La première méthode consiste à utiliser la fonctionnalité native. L’utilisation de fonctionnalités natives présente le moins de risques et présente de nombreux avantages. Cette approche vous permet de vous appuyer sur la documentation et les tutoriels existants fournis par Adobe Commerce pour l’utilisation de la fonctionnalité. La gestion des stocks comporte de nombreux aspects. L’utilisation de ce qui est fourni avec l’application doit donc être la première considération. Cependant, dans certains cas d’utilisation, les données trouvées dans le commerce au moment de la commande peuvent ne pas être entièrement exactes. Par exemple, les ventes sont autorisées en dehors de l’application Adobe Commerce directement dans le système de gestion des commandes. L’une des raisons est que pour s’assurer que les niveaux de stock exacts sont représentés dans Adobe Commerce, il faudrait une sorte d’intégration pour que les informations Adobe Commerce soient aussi proches que possible de leur précision. Si la vente excessive n’est pas acceptable, l’ajout d’un seuil de rupture de stock est une bonne méthode pour arrêter la vente des articles avant d’atteindre zéro. La fonctionnalité de synchronisation native d’Adobe Commerce est d’au plus 1 fois par jour. Cela est suffisant pour certains cas d’utilisation, mais peut ne pas être assez fréquent pour d’autres. Pour plus d’informations, consultez la section [Import et export planifiés](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/data-transfer/data-scheduled-import-export) .

La seconde approche serait `near real-time`. Le temps quasi réel utilise toujours la fonctionnalité native. Toutefois, cela inclut un travail supplémentaire pour fournir une intégration qui alimente le commerce fréquemment pour mettre à jour son inventaire selon un calendrier. Par exemple, toutes les heures. Cette option nécessite une réflexion sur la manière dont une intégration peut fonctionner, mais l’utilisation de l’&quot;api en masse&quot; et la transformation des données par un certain middleware est une bonne approche. Envisagez d’utiliser Adobe App Builder ou des plateformes similaires pour effectuer l’essentiel du travail et transmettre les informations à Adobe Commerce plus fréquemment.

La troisième approche, la plus complexe avec le plus de risques et de responsabilités, consiste à vérifier les stocks en temps réel sur une API ou une source de données externe. L’exécution d’une vérification d’inventaire en temps réel sur un système externe est risquée et comporte plusieurs autres éléments à prendre en compte. Voici quelques autres éléments qui doivent être évalués :

* Le système externe peut-il accepter les demandes REST ou GraphQL ?
* Existe-t-il des limites au point de terminaison telles que X nombre de requêtes par minute qui peuvent ne pas correspondre au trafic du site web ?
* Qu’advient-il du temps de réponse sous le chargement ?
* Que se passe-t-il lorsque les temps de réponse sont longs, arrêtez-vous automatiquement et utilisez une option de secours telle que l’inventaire natif.
* Quel type de surveillance est disponible pour s’assurer que les demandes d’API sont conformes aux limites de tolérance ?

## Remarques concernant la gestion des stocks non native

Veillez à ce que les personnalisations restent aussi simples que possible.
L’organisation de l’inventaire peut-elle être plate, n’est-ce qu’un SKU et la quantité totale de stock disponible OU y a-t-il d’autres attributs à prendre en compte ?

Si les informations d’inventaire sont assez plates, par exemple un SKU et la quantité totale disponible, les options pour le temps quasi réel sont développées. Le concept de temps quasi réel signifie qu’il existe une opération en arrière-plan qui rassemble l’inventaire de la source, puis remplit un moteur de stockage à utiliser pour répondre à la demande. Pour cela, vous pouvez utiliser des éléments tels que Redis, Mongo ou d’autres bases de données non relationnelles. Ces options sont intéressantes, car elles sont très rapides et fonctionnent parfaitement pour les paires clé/valeur. Si les données sont un peu plus complexes, il est probable que l’utilisation d’une base de données de relation, à l’intérieur ou à l’extérieur de l’application commerciale, soit nécessaire. En le déchargeant de la base de données commerciale, vous gardez l’application commerciale principale isolée de ces transactions. Un autre ensemble d’avantages est d’économiser les E/S de l’application commerciale, du processeur, de la mémoire RAM et d’autres sur l’utilisation. Au lieu d’utiliser les ressources des serveurs d’applications Adobe Commerce, utilisez les nouvelles API pour extraire les données de l’espace de stockage hors site.  Cela nécessitera probablement un middleware pour aider à transformer toutes les données. Assurez-vous ensuite que l’application appelante peut obtenir le résultat escompté. L’utilisation d’Adobe App Builder avec l’impression d’API permet de transformer les données et de les renvoyer correctement formatées.

L’utilisation d’Adobe App Builder avec le maillage d’API est également une excellente option lorsqu’il existe plusieurs sources d’inventaire.


## Déplacement de la logique d’exécution vers un emplacement hors processus

Adobe Developer App Builder fournit un cadre d’extensibilité tiers unifié pour l’intégration et la création d’expériences personnalisées afin d’étendre les solutions Adobe. Adobe Commerce peut utiliser Adobe Developer App Builder. Il s’agit d’un excellent cas d’utilisation pour étendre certaines fonctionnalités qui se trouvent normalement dans l’application principale et la déplacer hors site. En supprimant les fonctionnalités de l’application Commerce, vous réduisez le nombre de modules et la complexité de l’application Commerce. En retour, un nombre plus faible de personnalisations en cours de processus réduit la complexité de la mise à niveau et de la maintenance.

Pour vous inspirer de la manière dont cela peut être accompli, l’équipe d’Adobe a créé une documentation qui peut être une excellente source d’inspiration et fournir des exemples de code de travail. Lorsqu’un acheteur ajoute un produit au panier, un système tiers de gestion de l’inventaire vérifie si l’article est en stock. Si tel est le cas, autorisez l’ajout du produit. Sinon, affichez un message d’erreur.  Pour obtenir des exemples de code et des informations supplémentaires, consultez la page [Cas d’utilisation de Webhook](https://developer.adobe.com/commerce/extensibility/webhooks/use-cases/#add-product-to-cart).

## Quand effectuer des vérifications d’inventaire

Quand vérifier si l’inventaire est toujours disponible, le responsable de l’entreprise sera finalement tenu de vérifier s’il y a toujours un inventaire, l’architecte du logiciel avec la participation d’autres intervenants clés. Quelques heures appropriées sont incluses lors de l’ajout d’un élément au panier et lors de la saisie du workflow de passage en caisse. Tous les autres événements ajouteront simplement de la charge aux systèmes principaux lorsque cela peut ne pas être nécessaire. Gardez à l’esprit que l’objectif est de ne capturer un problème d’inventaire que lorsqu’il est essentiel. D’autres vérifications peuvent s’avérer utiles, mais si elles peuvent avoir une incidence sur l’objectif global des vérifications d’état des stocks, elles doivent être soigneusement examinées et uniquement autorisées si les parties prenantes de l’entreprise ou d’autres personnes sont conscientes du risque potentiel de charge supplémentaire sur les systèmes principaux.

## Recherche de la source de l’inventaire

Une enquête approfondie de la source d’inventaire externe est nécessaire. Les éléments qui doivent être évalués sont les options d’API disponibles, la prise en charge de GraphQL et les temps de réponse attendus. Si la source d’inventaire dispose d’une bande passante de connexion limitée ou n’a jamais été conçue pour être utilisée dans une demande en temps réel, la possibilité d’utilisation est exclue et l’architecte doit prendre en compte le temps quasi réel à la place.  Si les délais de requête de l’API dépassent les paramètres définis, cela l’exclura également d’être une option viable.  Par exemple, les réponses de l’api sont de 200 MS® pour une fois les demandes, mais passent à 500 à 900 MS® sous charge modérée.  Cela ne ferait probablement qu’empirer avec plus de charge et excluait la disponibilité des appels d’inventaire en direct.

Veillez à tester les temps de réponse de l’api avec des requêtes simples et un volume élevé similaire au trafic attendu sur le site web en direct. N’oubliez pas de tester toutes les zones du commerce en même temps pour simuler des scénarios du monde réel. Si l’attente est qu’un appel d’inventaire en direct se produise sur les pages du produit, lors de l’affichage et de la modification d’un panier et lors du passage en caisse, le test de charge doit simuler tous ces événements en même temps pour imiter le comportement réel du client et l’attente d’un magasin.

## Options de secours

Si la source d’inventaire est en panne et que la surveillance est disponible, il est recommandé d’utiliser la fonctionnalité native d’Adobe Commerce. Toutefois, avec une surveillance appropriée, l’expérience client peut changer de manière dynamique pour refléter la perte des contrôles d’inventaire en temps réel. Cela peut signifier qu’une vente ou un événement est annulé tôt ou retiré de l’affichage pour éviter les ventes excessives. Il faut tenir compte de l’attente de savoir quoi faire lorsque la source de l’inventaire est épuisée pour une raison quelconque et en discuter avec le propriétaire du magasin afin que tout le monde soit au courant de tout processus automatique qui prend le relais dans cette malheureuse situation.

## Conclusion

La décision d’effectuer des vérifications d’inventaire en temps réel est importante. S&#39;assurer que le propriétaire du site web, l&#39;équipe de développement et d&#39;autres personnes soient pleinement formés et conscients de tous les gains et pièges potentiels repose sur le chef de projet ou l&#39;architecte. En proposant un plan réfléchi qui couvre les raisons et un processus de secours est la clé du succès.

Les contrôles d’inventaire en direct peuvent être effectués, mais nécessitent des recherches et des réflexions sur les tests et la validation pendant le cycle d’assurance qualité. En veillant à ce que les tests de charge et les tests automatisés de bout en bout permettent de s’assurer que tous les problèmes potentiels sont capturés et triés.

En prenant en compte les actions à effectuer si la surveillance détecte une tendance des appels en échec vers le système externe ou si les délais de réponse sont supérieurs aux seuils acceptables, le site reste en ligne et offre le moins d&#39;irritation aux clients. Les options de secours peuvent être aussi simples que la désactivation des vérifications d’inventaire externes et l’utilisation de la fonctionnalité native ou peuvent être aussi avancées que la désactivation d’une promotion, l’avertissement à l’équipe de développement des problèmes de réaffectation ou même le reroutage des demandes vers un deuxième ou troisième système principal si disponible. La manière dont le mécanisme de secours est appliqué doit simplement être considérée comme la mise en oeuvre proprement dite, car chaque intégration présente des problèmes à un moment donné. Tout ce qui est automatisé ou nécessite une action manuelle doit être clairement documenté.
