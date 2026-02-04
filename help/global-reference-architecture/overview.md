---
title: Optimisation de la réutilisation du code avec Adobe Commerce
description: Découvrez comment optimiser la réutilisation du code dans Adobe Commerce avec des modèles d’architecture de référence globale, ce qui améliore les performances et la conformité sur plusieurs instances.
kt: 15773
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Contribution de Tony Evers, architecte technique principal, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contribution Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: 5475ade8-028c-4b24-a563-60dcda5ba93a
source-git-commit: 79d57d2c04c42a8dc23b5735e72e841b7e51cc63
workflow-type: tm+mt
source-wordcount: '1119'
ht-degree: 0%

---

# Techniques de mise en œuvre de l’architecture de référence globale

{{only-for-on-prem-commerce-cloud}}

Il existe plusieurs façons d’optimiser la réutilisation du code avec Adobe Commerce. Ces quatre techniques de mise en œuvre présentent chacune leurs avantages propres. Les exemples de cet article sont classés de simples à plus complexes. Choisissez la stratégie qui correspond le mieux à votre projet et à la feuille de route future. La migration d’une stratégie vers une autre peut prendre du temps.

## Quand utiliser l’architecture de référence globale

L’architecture de référence globale peut s’avérer utile, selon le nombre d’instances que vous possédez. Une instance est une installation autonome d’Adobe Commerce utilisant sa propre base de données. Comptez le nombre de bases de données de production pour connaître le nombre d’instances que vous possédez. Si vous conservez plusieurs instances ou si vous envisagez ce scénario à l’avenir, vous pouvez bénéficier d’une architecture de référence globale. Plus les instances partagent de fonctionnalités, plus une architecture de référence globale ajoute de valeur.

Dans l’un de ces scénarios, il est conseillé d’explorer l’utilisation de plusieurs instances d’Adobe Commerce.

1. **Différents propriétaires de boutique** : si vous conservez du code pour plusieurs propriétaires de boutique, chacun ayant sa propre boutique, des instances distinctes peuvent être nécessaires pour gérer efficacement leurs besoins individuels.
2. **Conformité aux réglementations nationales** : certaines réglementations exigent que les données clients soient stockées dans des régions spécifiques. Dans de tels cas, des instances distinctes sont essentielles pour assurer le respect de ces règlements.
3. **Variances opérationnelles entre les régions géographiques** : l&#39;exploitation dans plusieurs régions peut entraîner des calendriers et des exigences de maintenance différents. L’utilisation d’instances distinctes offre une certaine souplesse dans la gestion efficace de ces variations.
4. **Ventes Flash haute intensité** : les magasins qui réalisent des ventes flash à grande échelle nécessitent souvent des performances de serveur optimisées. Une infrastructure dédiée fournie par des instances distinctes garantit des performances optimales pendant ces périodes de forte demande.
5. **Différences significatives entre les marques ou les pays** : lorsque la différence entre les marques ou les pays est importante, l’utilisation d’une seule instance entraîne l’utilisation d’un code uniquement pour certaines marques ou certains pays. Des instances distinctes peuvent améliorer les performances et la stabilité en éliminant le code inutile pour les marques et les pays qui n’en ont pas besoin.

## Modèles d’architecture de référence globale

À côté de « aucun motif GRA », il existe quatre styles de motifs GRA.

![5 icônes de motifs GRA : pas de GRA, split, bulk, separée et monorepo.](/help/assets/global-reference-architecture/gra-patterns-horizontal.png){align="center"}

### Aucun modèle GRA

![Icône représentant « pas de GRA »](/help/assets/global-reference-architecture/no-gra.png){align="center"}

Lorsqu’aucun modèle GRA n’est utilisé, chaque instance Adobe Commerce est une application unique. Il n’y a pas de réutilisation du code, sauf en déplaçant manuellement le code d’une instance à une autre. Ces copies divergent toujours. La quantité d’effort nécessaire pour s’assurer que chaque instance comporte les mêmes modifications mais fonctionne toujours comme prévu peut devenir écrasante. Dans ce scénario, trois instances nécessitent trois fois plus d’efforts de maintenance qu’une seule instance.

![Diagramme représentant 3 magasins, où chacun est une copie du premier, avec un développement unique se produisant dans les 3 copies.](/help/assets/global-reference-architecture/no-gra-pattern-diagram.png){align="center"}

### Modèle Git GRA fractionné

![Icône représentant le modèle GRA « partagé »](/help/assets/global-reference-architecture/split-git.png){align="center"}

Ce modèle se compose de référentiels Git pour le développement et d’un référentiel Git par instance. Chaque fichier de l’instance est conservé dans l’un des référentiels de développement. Ils se rassemblent en une tresse formant l&#39;ensemble de la GRA. Chaque ligne de code n’existe que dans un seul référentiel de développement et est installée sur les instances à l’aide de la technique de tressage, ce qui entraîne la réutilisation du code.

![Diagramme indiquant où le code est stocké dans un modèle de répartition GRA](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

### Modèle GRA des packages en masse

![Icône représentant le modèle GRA « en bloc »](/help/assets/global-reference-architecture/bulk-packages.png){align="center"}

Les modules principaux Adobe Commerce et tiers sont directement installés via les référentiels du compositeur. Les référentiels Git peuvent être utilisés comme référentiels de compositeur. Dans ce modèle, l’ensemble de la base de code partagée GRA est hébergé dans un ou plusieurs référentiels Git et installé via le compositeur. La caractéristique clé est que plusieurs modules, modules linguistiques ou thèmes sont hébergés dans un seul package de compositeur, ce qui simplifie le développement.

![Diagramme indiquant où le code est stocké dans un modèle GRA de packages en masse](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

### Le modèle GRA des packages distincts

![Icône représentant le modèle GRA « packages distincts »](/help/assets/global-reference-architecture/separate-packages.png){align="center"}

Chaque module, module linguistique ou thème d’Adobe Commerce est installé en tant que package de compositeur distinct. Chaque personnalisation possède son propre référentiel Git. Cela se traduit par une flexibilité maximale dans la composition des instances et offre une gestion fiable des dépendances du compositeur. Pour l’optimisation des performances, tous les packages sont mis en miroir dans un seul référentiel de compositeur privé.

![Diagramme indiquant où le code est stocké dans un modèle GRA de packages distinct](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

### Modèle GRA monorepo

![Icône représentant le modèle GRA « monorepo »](/help/assets/global-reference-architecture/monorepo.png){align="center"}

Tous les développements ont lieu dans un seul référentiel de code. L’automatisation génère des packages pour les nouvelles versions et les publie dans un référentiel de compositeur. Le modèle combine la faible surcharge de développement de l’approche des packages en bloc avec la flexibilité de l’approche des packages distincts. Le modèle monorepo est également idéal pour exécuter des tests fonctionnels automatisés.

![Diagramme indiquant où le code est stocké dans un modèle GRA monorepo](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## Choisir un modèle GRA

Le choix d&#39;un modèle GRA se fait en évaluant la complexité du projet, le besoin de flexibilité et la capacité d&#39;adaptation de l&#39;équipe de développement.

Il est préférable que les équipes peu expérimentées dans Adobe Commerce commencent simplement. Toutefois, si le projet nécessite un modèle GRA plus complexe en raison de ses caractéristiques, ne faites aucun compromis.

Caractéristiques communes du projet liées à chaque modèle :

1. **Pas de modèle GRA** : instance unique d’Adobe Commerce sans plan d’extension. Plusieurs instances d’Adobe Commerce avec un minimum de points communs.

2. **Modèle Git partagé** : les équipes qui souhaitent éviter le compositeur pour leurs personnalisations, dans la plupart des cas le modèle de packages en bloc est un modèle préféré au modèle Git partagé.

3. **Modèle GRA de package en masse** : base de code de personnalisation avec une interdépendance élevée. Les instances ont toutes des combinaisons très similaires de packages personnalisés. Aucune promotion ou rétrogradation fréquente des packages individuels. Équipes peu expérimentées dans la gestion de code et ayant besoin de simplicité.

4. **Modèle GRA de packages distinct** : une gestion flexible de la portée de publication est nécessaire. 50 packages personnalisés ou moins sont prévus dans les 5 prochaines années. Éventuellement, couches mondiales et régionales de code commun. Aucun plan de migration vers un modèle Monorepo. L’équipe est techniquement compétente et respecte strictement les processus.

5. **Modèle GRA Monorepo** : toutes les caractéristiques du modèle GRA des emballages séparés. Plus de 50 forfaits sont prévus au cours des cinq prochaines années. Besoin de tests automatisés approfondis. Prise en charge des environnements éphémères. Base de code complexe à l’échelle de l’entreprise qui doit évoluer sans augmenter les coûts de maintenance à un taux égal.

### Le coût d’un mauvais choix

La migration d’un modèle à un autre est possible. Le diagramme ci-dessous montre le niveau d’impact du passage d’un modèle à un autre. Les lignes vertes montrent un impact faible, les lignes jaunes et ambres montrent des migrations modérément complexes à complexes.

![Diagramme montrant des flèches colorées entre les 4 motifs GRA, indiquant le niveau de difficulté à passer de l&#39;un à l&#39;autre.](/help/assets/global-reference-architecture/wrong-choice.png){align="center"}
