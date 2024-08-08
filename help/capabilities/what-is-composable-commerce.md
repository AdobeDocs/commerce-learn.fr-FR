---
title: Comment Adobe crée un Commerce composite
description: Découvrez le commerce composable, privilégiez une approche API-premier et implémentez une architecture modulaire et orientée service.
feature: App Builder, Saas
topic: Architecture, Commerce, Development, Headless, Integrations, Performance, Personalization
role: Admin, Architect, User
level: Beginner, Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-06-27T00:00:00Z
jira: KT-15730
thumbnail: KT-15730.jpeg
exl-id: 4d811a2f-8488-4de7-babd-449aced42e3a
source-git-commit: 8314041fa2649ce76c6819dced1694e0e0cb08b2
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 0%

---

# Commerce composite

Le commerce composite est une approche architecturale du commerce électronique qui implique de découpler la couche de présentation frontale de la fonctionnalité commerciale principale. &#x200B; Il permet aux entreprises de sélectionner et de combiner les meilleurs composants ou modules pour créer une solution personnalisée. Cette approche implique de décomposer la plateforme monolithique traditionnelle de l&#39;e-commerce en services ou microservices plus petits et indépendants pouvant être composés ensemble. Le commerce composite offre des avantages tels que la flexibilité, l’évolutivité, la personnalisation, l’agilité et la possibilité d’intégrer plus facilement d’autres systèmes et technologies.

Adobe Commerce fournit de nombreuses fonctionnalités et outils pour aider les commerçants à adopter et à mettre en oeuvre un commerce composable. Adobe Commerce offre une méthodologie de commerce composable et des expériences front-end hybrides et non headless. En gardant à l’esprit l’extensibilité hors processus, Adobe propose le maillage API pour l’intégration de plusieurs services et Adobe App Builder pour la création de microservices personnalisés.

## Pourquoi le commerce composable est-il important ?

Pour plusieurs raisons, il est important de comprendre le commerce composable pour les entreprises et les individus impliqués dans l&#39;industrie de l&#39;e-commerce. Il offre flexibilité et personnalisation, évolutivité et agilité, capacités d’intégration, contrôle de l’avenir et responsabilisation des développeurs. Le commerce composite permet aux entreprises d’adapter et d’évoluer leurs plates-formes de commerce électronique, d’adapter et de développer leurs activités. Quelques avantages plus impressionnants incluent la possibilité de s’intégrer à d’autres systèmes et technologies, de suivre l’évolution des attentes des clients et de tirer parti de l’expertise des développeurs.

Il permet aux entreprises de naviguer dans le paysage évolutif du commerce électronique, de prendre des décisions éclairées et de tirer parti des avantages de la flexibilité, de l’évolutivité, de la personnalisation, de l’agilité et des fonctionnalités d’intégration. En outre, la connaissance du commerce composable est importante pour la croissance des entreprises, la personnalisation, l&#39;agilité et l&#39;innovation, l&#39;efficience des coûts, l&#39;intégration et la flexibilité, et l&#39;imperméabilité à l&#39;avenir. Il permet aux entreprises de s’adapter rapidement aux nouvelles fonctionnalités et de répondre aux changements des conditions du marché, de créer des solutions hautement personnalisées. Il y a d’autres avantages qui incluent la possibilité d’innover rapidement, de réduire les coûts de développement et de maintenance et de s’intégrer à des services et à des systèmes tiers.

Dans l’ensemble, la compréhension du commerce composable permet aux entreprises de prendre des décisions éclairées, d’optimiser leur architecture et leur stratégie de commerce électronique, ainsi que de stimuler la croissance et le succès sur le marché numérique.

## Comment les entreprises peuvent-elles bénéficier d&#39;un commerce composable ?

Les entreprises peuvent bénéficier d’un commerce composable de plusieurs façons :

**Flexibilité et personnalisation :** Le commerce composite permet aux entreprises de sélectionner et de combiner les meilleurs composants ou microservices pour créer une solution de commerce électronique personnalisée qui répond à leurs besoins spécifiques. Il permet aux entreprises d’adapter leur plateforme pour offrir des expériences client uniques, mettre en oeuvre des fonctionnalités spécialisées et se différencier sur le marché.

**Évolutivité et agilité :** Avec une architecture composable, les entreprises peuvent mettre à l’échelle différents composants de leur plateforme de commerce électronique indépendamment. Cela signifie qu’ils peuvent allouer des ressources et mettre à l’échelle des fonctionnalités spécifiques en fonction de la demande, assurant des performances optimales et une rentabilité optimale. Le commerce composite permet également des pratiques de développement agiles, ce qui permet aux équipes de travailler simultanément sur différents composants et de déployer les modifications indépendamment, ce qui se traduit par un délai de mise sur le marché plus rapide.

**Fonctionnalités d’intégration :** Le commerce composite facilite l’intégration transparente à des systèmes, services et technologies tiers. Les entreprises peuvent facilement connecter leur plateforme de commerce électronique à divers outils tels que des passerelles de paiement, des systèmes ERP, des systèmes CRM, des plateformes d’automatisation marketing, etc. Cette fonctionnalité d’intégration permet aux entreprises de tirer parti des meilleures solutions et de créer un écosystème unifié qui améliore l’efficacité opérationnelle et l’expérience client.

**Futur-Proofing :** Le commerce composite offre aux entreprises la flexibilité d’adapter et d’évoluer leur plateforme d’e-commerce à mesure que la technologie et les tendances du marché évoluent. Il permet aux entreprises d’ajouter ou de remplacer facilement des composants, d’intégrer de nouvelles technologies et de rester en avance sur la concurrence. Grâce à cette capacité d’anticipation, les entreprises peuvent continuellement innover et répondre aux attentes changeantes des clients.

**Développeurs :** Le commerce composite permet aux développeurs de travailler avec différentes technologies, langues et structures. Il permet aux développeurs de se concentrer sur des composants ou des microservices spécifiques, ce qui leur permet de bénéficier d’une expertise et d’une spécialisation spécifiques. Cette prise de pouvoir se traduit par une productivité accrue des développeurs, des cycles de développement plus rapides et la capacité à tirer parti des outils et technologies les plus récents.

Dans l’ensemble, le commerce composable permet aux entreprises de créer une plateforme d’e-commerce hautement flexible, évolutive et personnalisable qui peut s’adapter aux besoins changeants de l’entreprise, offrir des expériences client exceptionnelles et stimuler la croissance et le succès sur le marché numérique.

## Fonctionnalités d’Adobe Commerce pour un commerce composable

Adobe Commerce offre plusieurs fonctionnalités pour aider les commerçants à adopter et à implémenter un commerce composable :

**Méthodologie Commerce composite :** Adobe Commerce offre une méthodologie de commerce composable qui guide les commerçants dans la compréhension et la mise en oeuvre des principes de l’architecture composable. Cette méthodologie permet aux entreprises de tirer parti de la flexibilité, de l’évolutivité et de la personnalisation tout en tenant compte de facteurs tels que la complexité, la maturité technique et la taille du projet.

**Fonctionnalité riche en fonctionnalités :** Adobe Commerce fournit un ensemble complet de fonctionnalités accessibles par le biais de son GraphQLAPI. Cette fonctionnalité riche en fonctionnalités permet aux vendeurs de minimiser le nombre de fournisseurs requis dans leur pile commerciale, ce qui réduit les défis de délai de mise sur le marché. Il permet aux entreprises de lancer plus rapidement tout en maintenant la flexibilité de composer et d’intégrer des services ou fonctionnalités tiers supplémentaires à mesure que leur pile de commerce évolue.

**Expériences front-end hybrides :** Adobe Commerce prend en charge les expériences front-end sans tête et non sans tête dans le même écosystème. Cette flexibilité permet aux commerçants de choisir l’approche architecturale la plus appropriée pour chaque cas d’utilisation frontale spécifique. Elle permet une transition progressive vers une architecture sans interface et compostable sans avoir besoin d’une migration simultanée de l’ensemble du système.

**Mesh API :** Adobe Commerce API Mesh simplifie l’intégration de plusieurs microservices, outils tiers et applications dans un point de terminaison d’API unifié pour les développeurs front-end. Il permet aux développeurs de combiner plusieurs sources de données en un seul point de terminaison GraphQL, ce qui réduit la complexité et rationalise le développement et la maintenance de nouvelles fonctionnalités et expériences.

**Adobe App Builder :** Adobe App Builder est une plateforme d’extensibilité sans serveur qui permet aux marchands de créer des microservices personnalisés, de créer des expériences personnalisées et d’étendre les solutions d’Adobe. Avec App Builder, les marchands peuvent créer des applications sécurisées et évolutives qui étendent les fonctionnalités natives d’Adobe Commerce et s’intègrent à des solutions tierces. Cela évite aux commerçants de créer et de gérer leur propre infrastructure pour la personnalisation et les microservices, ce qui réduit la complexité et le coût total de propriété.

Ces fonctionnalités fournies par Adobe Commerce simplifient l’adoption et la mise en oeuvre d’un commerce composable, ce qui permet aux commerçants de tirer parti des avantages de la flexibilité, de l’évolutivité, de la personnalisation et des capacités d’intégration tout en réduisant la complexité et l’effort de développement.

## Réflexions finales

Le commerce composite est une approche architecturale du commerce électronique qui implique de découpler la couche de présentation frontale de la fonctionnalité commerciale principale. Voici les principales leçons à tirer du commerce composable :

**Architecture :** Le commerce composite suit une architecture modulaire et découplée, ce qui permet aux entreprises de sélectionner et de combiner les meilleurs composants ou microservices pour créer une solution personnalisée. Cette architecture offre flexibilité, évolutivité et agilité.

**Avantages :** Le commerce composite offre plusieurs avantages, notamment la flexibilité et la personnalisation, l’évolutivité et l’agilité, les fonctionnalités d’intégration, la future-vérification et l’autonomisation des développeurs. Il permet aux entreprises d’adapter, d’adapter, d’intégrer, d’innover et d’exploiter l’expertise des développeurs.

**Considérations :** Lorsque vous envisagez un commerce composable, des facteurs tels que la complexité, la maturité technique interne, la taille et la structure du projet, la personnalisation par rapport à la normalisation, le coût total de propriété, la sécurité et la confidentialité des données doivent être soigneusement évalués.

**Adobe Commerce :** Adobe Commerce fournit des fonctionnalités et des outils pour aider les commerçants à adopter et à mettre en oeuvre un commerce composable. Il s’agit notamment d’une méthodologie de commerce composable, de fonctionnalités riches en fonctionnalités, d’expériences frontales hybrides, d’un maillage d’API pour l’intégration et d’Adobe App Builder pour les microservices personnalisés.

**Impact sur l’entreprise :** Le commerce composite permet aux entreprises de créer une plateforme de commerce électronique hautement flexible, évolutive et personnalisable. Il leur permet de proposer des expériences client uniques, de s’adapter à la demande, de s’intégrer à d’autres systèmes, d’optimiser leurs opérations et de tirer parti de l’expertise des développeurs.

Il est essentiel pour les entreprises du secteur du commerce électronique de rester compétitives, de s’adapter à l’évolution des conditions du marché et de proposer des expériences client exceptionnelles. En adoptant un commerce composable, les entreprises peuvent exploiter les avantages de la flexibilité, de l&#39;évolutivité, de la personnalisation, de l&#39;agilité et des capacités d&#39;intégration, ce qui stimule la croissance et la réussite sur le marché numérique.
