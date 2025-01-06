---
title: Architecture de référence globale des packages distincts
description: Optimisez Adobe Commerce avec des packages GRA distincts. Découvrez la configuration, les avantages et les bonnes pratiques pour une gestion des packages flexible et versionnée.
kt: 16727
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
source-git-commit: 916586d3b71b8b74baa04af2ab39abc86ec94f5b
workflow-type: tm+mt
source-wordcount: '2088'
ht-degree: 0%

---


# Modèle d’architecture de référence globale des packages distincts

Ce guide explique comment configurer Adobe Commerce avec le modèle GRA (Global Reference Architecture) de packages distincts.

Le modèle GRA pour les packages distincts implique un référentiel Git pour chaque package commun et un référentiel Git pour chaque instance Adobe Commerce. Les packages courants sont exposés via le compositeur avec un référentiel de compositeur privé.

Ce modèle d’architecture de référence globale est entièrement basé sur le compositeur et est conçu pour tirer le meilleur parti de toutes les fonctionnalités du compositeur.

![Diagramme indiquant où le code est stocké dans un modèle GRA de packages distinct](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

## Avantages et inconvénients de ce modèle

Avantages :

- Réutilisation du code dans des référentiels de code partagés
- Flexibilité totale dans l&#39;installation des packages, chaque package GRA peut être mis à niveau, rétrogradé ou rétroporté individuellement
- Prise en charge complète du contrôle de version sémantique
- Aucun outil spécial, infrastructure complexe ou stratégie de branchement spéciale requis
- Prise en charge de tous les types de packages pris en charge par le compositeur

Inconvénients :

- Le développement au sein de ce schéma GRA est un peu plus difficile au début, il y a une petite courbe d&#39;apprentissage
- Possibilité de déployer des combinaisons de packages qui n&#39;ont pas été développés dans la même configuration, nécessité de procédures de test strictes

## Configuration d’Adobe Commerce avec le modèle GRA de packages distincts

### La structure du répertoire

La structure de répertoires finale d’une installation Adobe Commerce complète avec le modèle GRA Packages distinct ressemble à ceci :

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

Les répertoires `app/code`, `app/i18n` et `app/design` sont délibérément omis. La GRA relative aux packages distincts installe chaque package du compositeur. Même si le package n’est installé que dans une seule instance Adobe Commerce.

### Préparation du référentiel de magasin

Créez un référentiel pour la première instance d’Adobe Commerce, qui représente un magasin web pour Brand X.

```bash
mkdir gra-separate-brand-x
cd gra-separate-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-separate-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

Installez Adobe Commerce avec `bin/magento setup:install`. Validez les `app/etc/config.php` résultantes.

### Créer des référentiels de package

Chaque package de ce modèle d’architecture de référence globale possède son propre référentiel Git. Vous trouverez ci-dessous des exemples de packages contenant des modules Adobe Commerce représentant un module GRA, un module tiers et un module local.

- <https://github.com/AntonEvers/module-example-gra>
- <https://github.com/AntonEvers/module-example-3rdparty>
- <https://github.com/AntonEvers/module-example-local>

Utilisez les exemples pour créer vos propres packages.

### Créer des référentiels de package métallique

Les métapaquets contrôlent la portée de la base de code commune GRA dans ce modèle GRA. Ils définissent ce qui se trouve dans la base : les combinaisons de versions de package qui sont toujours installées ensemble. Par exemple :

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "~2.0",
        "magento/product-enterprise-edition": "2.4.6-p3"
    }
}
```

Le fragment de code ci-dessus est le fichier composer.json d’un métapaquet. Parce que les métapaquets contiennent uniquement un fichier composer.json et aucun autre code. Le code ci-dessus est également le métapaquet complet. Hébergez-le dans un référentiel Git et vous disposez d’un référentiel du compositeur de métapaquets installable. Un exemple de module GRA et un module tiers, ainsi que le module principal Adobe Commerce sont nécessaires. Elle nécessite également la technologie gra-component-foundation, qui sera expliquée dans le chapitre suivant.

Les métapaquets permettent de regrouper des packages sans créer de dépendances entre les packages. Ainsi, même lorsqu’il n’existe aucune dépendance technique entre les packages, avec un métapaquet, vous pouvez faire en sorte qu’ils soient installés ensemble. Si vous avez besoin de ce métapaquet dans votre projet, tout package ou métapaquet requis par le métapaquet est installé. Ainsi, si vous créez un projet de compositeur vierge et que vous avez uniquement besoin de ce package, le compositeur installe Adobe Commerce ainsi que les modules GRA et tiers.

Vous pouvez ainsi vous assurer que chaque magasin contient le même ensemble de packages de base.

Vous pouvez également définir un métapaquet qui définit le magasin x. Il nécessite le métapaquet de base, qui nécessite la base GRA complète, ainsi qu’un module local :

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "require": {
        "antonevers/gra-meta-foundation": "~1.0",
        "antonevers/module-local": "~1.0"
    }
}
```

Le métapaquet Brand-X est facultatif. Vous pouvez également ignorer le métapaquet de marque et exiger ces dépendances directement dans le projet du compositeur de votre boutique. L’avantage de créer un métapaquet pour les modules locaux est que vous n’avez aucune branche de fonctionnalité ni requête d’extraction de fonctionnalité sur le référentiel Git du magasin, mais uniquement dans les référentiels de package. C&#39;est une mesure de sécurité. De plus, vous pouvez choisir d’appliquer un contrôle de version sémantique sur les référentiels de packages et d’utiliser différentes balises Git sur votre projet principal, par exemple pour effectuer le suivi des versions nommées. C&#39;est à toi de voir.

### Fichiers de base GRA situés en dehors du répertoire fournisseur

Vous devez parfois stocker des fichiers en dehors du répertoire du fournisseur. Par exemple, les `.gitignore`, les fichiers qui se trouvent dans le répertoire `dev/` ou les fichiers de vérification de domaine. Le type de package magento2-component est conçu à cet effet. Regarde-<https://github.com/AntonEvers/gra-component-foundation>.

```json
{
    "name": "antonevers/gra-component-foundation",
    "type": "magento2-component",
    "require": {
        "magento/magento-composer-installer": "*"
    },
    "extra": {
        "map": [
            [
                "src/gitignore",
                ".gitignore"
            ]
        ]
    }
}
```

Ce package est du type magento2-component et contient un répertoire src qui héberge les fichiers copiés dans le répertoire racine Adobe Commerce. Le mappage de ce fichier copie les `/src/gitignore` dans les `/.gitignore` du projet principal du compositeur.

De cette façon, vous pouvez également intégrer des fichiers situés en dehors du répertoire fournisseur à votre base GRA.

### Développement d’un module de base GRA

Le développement se produit dans le répertoire du fournisseur. Demandez au compositeur d’installer vos packages de base à partir de la source. Ce faisant, extrait les packages à partir de Git au lieu de les installer à partir d’une archive téléchargée.

```bash
rm -r vendor/antonevers/*
composer install --prefer-source
```

Avec cette commande, les packages de l’espace de noms antonevers ont été extraits à l’aide de Git. Lorsque vous saisissez le répertoire fournisseur/antonevers/module-gra, vous saisissez également le référentiel Git module-gra. Vous pouvez désormais créer, extraire et fusionner des branches sur place et les développer de cette manière, directement à partir du répertoire fournisseur.

### Inclure des modules tiers dans GRA Foundation

Ajoutez des packages tiers au métapaquet GRA. Si du code tiers n’est pas disponible pour être installé à partir d’un référentiel de compositeur, créez un package pour celui-ci. Créez un référentiel Git, ajoutez le contenu des packages (tout ce qui se trouverait dans app/code/Vendor/Package) et assurez-vous qu’il existe un fichier composer.json valide à la racine du référentiel. Vous pouvez désormais installer ce package via le compositeur.

## Configuration d’un référentiel Composer privé

Un référentiel privé n’est pas obligatoire dans l’architecture de référence globale. Cela accélère les déploiements et l’installation, réduit la configuration du référentiel dans composer.json et augmente la sécurité. Les informations d’identification d’autres référentiels Composer et d’Adobe Commerce Marketplace sont stockées dans votre référentiel privé. Plus d’informations d’identification sensibles incluses avec le code ou sur les ordinateurs des développeurs.

En outre, certains référentiels privés offrent des fonctionnalités supplémentaires, telles que des notifications par e-mail lorsque l’un de vos magasins contient une vulnérabilité de sécurité dans l’une de ses dépendances.

Le problème de lenteur se produit lorsque vous disposez de plusieurs référentiels VCS dans composer.json. Chaque référentiel de compositeur doit être lu lors des mises à niveau. En outre, disposer de 50 référentiels pour 50 packages représente au moins 50 fois la surcharge d’un seul référentiel de compositeur.

![Diagramme indiquant où la lenteur se produit lorsqu’un référentiel de compositeur est manquant](/help/assets/global-reference-architecture/separate-packages-without-mirror-diagram.png){align="center"}

Incluez un miroir du compositeur sous la forme d’un référentiel de compositeur privé. Le miroir contient une copie de tous les packages des autres référentiels du compositeur, ainsi que de tous les packages hébergés par Git. Avec un référentiel de compositeur privé, vous bénéficiez en outre d’un contrôle d’accès détaillé.

Grâce à la synchronisation Git, un référentiel Composer privé détecte automatiquement les nouveaux packages dans vos référentiels Git, ainsi que les nouvelles versions des packages existants.

Vous pouvez héberger votre propre référentiel privé avec Satis : <https://composer.github.io/satis/>. Consultez un exemple de référentiel public à l’adresse <https://antonevers.github.io/gra-composer-repository/>. Ce référentiel est utilisé comme référentiel du compositeur dans les exemples de code. Des mesures supplémentaires sont nécessaires pour rendre un référentiel Satis privé.

Il existe des solutions que vous pouvez configurer et oublier : Private Packagist <https://packagist.com/>, qui est créé par les mêmes personnes qui ont écrit Composer ou JFrog Artifactory <https://jfrog.com/artifactory/>.

## Code de diffusion

Avec les métapaquets, il existe 3 étapes pour diffuser le code.

1. Fusionner les modifications dans des packages et créer une version des packages modifiés.
2. (Facultatif, uniquement si de nouveaux packages sont ajoutés) Demandez les nouveaux packages dans des métapaquets et gérez la version des métapaquets.
3. (Facultatif, uniquement si de nouveaux packages sont ajoutés) Demandez les nouveaux métapaquets dans Adobe Commerce et déployez-les.

La portée du déploiement est contrôlée avec les versions de package. La création d’une version stable d’un package signifie que ce package est prêt pour le déploiement en production.

Pour créer une nouvelle version, exécutez la mise à jour du compositeur dans le projet principal du compositeur, qui contient l’installation complète du magasin. Toutes les dernières versions des packages sont installées.

## Contrôle de version

Le contrôle de version dans des packages distincts GRA est un synonyme des modules de balisage dans Git. Les balises Git créent des versions numérotées de vos packages que le compositeur installe.
La bonne approche de contrôle de version permet à vos packages de s’exécuter automatiquement, tout en restant sûrs.

Deux exemples :

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "1.1.4",
        "antonevers/module-gra": "1.0.0",
        "antonevers/module-3rdparty": "1.3.89"
    }
}
```

Cet exemple illustre une définition stricte des dépendances. 3 packages sont requis dans les versions exactes. La mise à jour du compositeur avec ce métapaquet dans votre installation ne fait rien. Il installe toujours ces 3 packages dans ces versions exactes, même si une version plus récente est disponible.

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.0",
        "antonevers/module-gra": "~1.0",
        "antonevers/module-3rdparty": "~1.0"
    }
}
```

Cet exemple illustre une définition lâche des dépendances. Avec `~1.0`, toute version de ces packages peut être installée si elle est supérieure ou égale à `1.0.0` et inférieure à `2.0.0`, avec une préférence pour la version disponible la plus élevée. Pour en savoir plus sur la définition des dépendances de version, consultez <https://getcomposer.org/doc/articles/versions.md> :

> L’opérateur ~ est mieux expliqué par l’exemple : `~1.2` équivaut à `>=1.2 <2.0.0`, tandis que `~1.2.3` équivaut à `>=1.2.3 <1.3.0`.

Dès que vous publiez une nouvelle version de l’un des packages mentionnés, elle est automatiquement installée avec la mise à jour du compositeur.

Appliquez le contrôle de version sémantique. Vous pouvez tout savoir sur le contrôle de version sémantique à l’adresse <https://semver.org/>. En particulier, la FAQ est une lecture incontournable. Avec le contrôle de version sémantique, les nombres de « 1.0.0 » sont appelés MAJEUR.MINEUR.PATCH. Les versions mineures et de correctif d’un package doivent pouvoir être introduites en toute sécurité sans interrompre l’application.
Vous pouvez inclure automatiquement des correctifs et choisir manuellement des mises à niveau mineures. Gardez à l’esprit que cela entraîne des frais généraux supplémentaires en sélectionnant manuellement chaque modification mineure :

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "require": {
        "antonevers/gra-component-foundation": "~1.1.0",
        "antonevers/module-gra": "~1.0.0",
        "antonevers/module-3rdparty": "~1.3.0"
    }
}
```

Bien sûr, tout cela ne fonctionne que si vous appliquez le contrôle de version sémantique de manière cohérente et permanente. Et pas seulement dans les métapaquets, mais les exigences de vos packages standard doivent également définir les dépendances de manière approximative. Si votre système comporte une dépendance stricte, ce package est limité à la définition stricte.

Recherchez ces dépendances strictes en saisissant composer dependencies \&lt;nom du package\>. Voir <https://getcomposer.org/doc/03-cli.md#depends-why> pour plus d’informations.

## Stratégie d’embranchement

Vous pouvez utiliser différentes stratégies d’embranchement pour prendre en charge ce modèle de stratégie de référence globale, à condition que la branche principale soit la seule branche dans laquelle vous créez des versions pour vos packages. Si vous gérez les versions sur plusieurs branches, cela introduit le risque de perdre aléatoirement des fonctionnalités entre les versions. Créez uniquement des versions stables sur la branche principale.

Créez uniquement des branches de fonctionnalités dans vos référentiels de packages. Pas dans les référentiels d’installation de votre boutique. N’hésitez pas à apporter des modifications à votre boutique en utilisant simplement le compositeur. Évitez d’avoir à effectuer des fusions Git sur le référentiel de déploiement.

Les types de branches courants dans les stratégies d’embranchement et les référentiels doivent exister dans :

**Branches de fonctionnalités** : existent dans vos référentiels de packages, nulle part ailleurs.

**branches de version** : elles sont créées dans n’importe quel référentiel : packages, métapaquets, magasins et référentiels d’installation. Lorsque vous planifiez une version, regroupez les modifications dans les branches de version des packages avant de créer des versions pour eux. Supposons que vous prépariez une version portant le nom de code « Licorne ». Vous pouvez créer une branche release-unicorn dans les packages avec des modifications. Fusionnez n’importe quel élément à l’intérieur de celui-ci, puis exigez « dev-release-unicorn as 1.4.0 » dans votre métapaquet. En savoir plus sur les alias pour voir ce qui se passe : <https://getcomposer.org/doc/articles/aliases.md>.

**branches QA/Dev** : similaire aux branches de version.

**Branche principale** : doit exister dans chaque référentiel et doit toujours être la branche qui représente la production ou un état prêt pour la production. La branche principale est l’emplacement où vous balisez le code vers les versions de publication.
Veillez à choisir une stratégie d’embranchement avec peu de frais de maintenance. Par exemple, la fusion de la branche principale en branches AQ, UAT, version ou développement après une publication de correctif est une tâche de maintenance planifiée. Plus il y a de packages, plus il y a de référentiels et plus les tâches répétitives sont lourdes.

Utilisez un outil tel que mixu/gr pour effectuer des opérations de routine sur plusieurs référentiels Git dans un lot : <https://github.com/mixu/gr>

## Conventions de dénomination

Avec le modèle GRA Separate Packages, les packages font partie de la base GRA si le métapaquet de base les requiert. Ajoutez ou supprimez des packages du métapaquet pour les déplacer dans et hors de la base.

Les métapaquets offrent une certaine flexibilité quant à la portée de l’installation des packages. Il est très important que les noms des packages ne contiennent aucun mot se rapportant à l’utilisation prévue du package. Le nom antonevers/module-gra-store-locator peut prêter à confusion lorsque vous décidez de retirer ce package de la base GRA. Évitez la portée (GRA, de base, locale). Évitez la région (EMEA, Espagne, Global). Évitez très certainement le nom du magasin pour lequel un package est créé. Choisissez des noms qui ne concernent que la fonctionnalité ajoutée dans le package. De cette façon, vous pouvez les réutiliser partout où vous le souhaitez, y compris dans des scénarios futurs imprévus. Le nom antonevers/module-store-locator serait excellent.

Assurez-vous que les packages associés s’affichent ensemble dans les vues d’ensemble. Noms de build du générique au spécifique. Ainsi, antonevers/module-b2b-tax-exempts au lieu d’antonevers/tax-exempted-module-b2b.

## Exemples de code

Les exemples de code de cet article de blog ont été combinés dans un ensemble de référentiels Git, que vous pouvez utiliser pour jouer avec la preuve de concept.

- Exemple de magasin de production : <https://github.com/AntonEvers/gra-separate-brand-x>
- Exemple de module de base : <https://github.com/AntonEvers/module-example-gra>
- Exemple de module tiers : <https://github.com/AntonEvers/module-example-3rdparty>
- Exemple de module local : <https://github.com/AntonEvers/module-example-local>
- Exemple de métapaquet de base : <https://github.com/AntonEvers/gra-meta-foundation>
- Exemple de métapaquet local (facultatif) : <https://github.com/AntonEvers/gra-meta-brand-x>
- Exemple de référentiel Composer : <https://github.com/AntonEvers/gra-composer-repository>