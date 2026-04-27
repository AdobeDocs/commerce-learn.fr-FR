---
title: Separate Packages Global Reference Architecture
description: Optimize Adobe Commerce with Separate Packages GRA. Learn setup, benefits, and best practices for flexible, versioned package management.
jira: KT-16727
doc-type: tutorial
duration: 594
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Contribution de Tony Evers, architecte technique principal, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contribution Tony Evers"
old-role: Architect, Developer
role: Developer, User, Leader
level: Beginner, Intermediate
exl-id: cbddc4a3-602f-4208-85cd-b906d2b81f8b
TQID: https://experienceleague.adobe.com/ihTCXVhaBPi5-6Xs1tiB-wDbVX-1CwHSgz80X0B02ts
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
  - id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: b5a62a22-46f7-4f0d-b151-3fc640bef588
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 2132
ht-degree: 0%

---

# The Separate Packages global reference architecture pattern

{{only-for-on-prem-commerce-cloud}}

This guide explains how to set up Adobe Commerce with the Separate Packages Global Reference Architecture (GRA) Pattern.

The Separate Packages GRA pattern involves one Git repository for each common package and a Git repository for each Adobe Commerce instance. Common packages are exposed through Composer with a private composer repository.

This global reference architecture pattern is completely Composer based and it is designed to get the maximum benefit from all Composer features.

![A diagram showing where code is stored in a Separate Packages GRA pattern](/help/assets/global-reference-architecture/separate-packages-gra-pattern-diagram.png){align="center"}

## Avantages et inconvénients de ce modèle

Avantages :

* Code reuse through a shared code repositories
* Complete flexibility in package installation, each GRA package can be upgraded, downgraded or backported individually
* Full support for semantic versioning
* Aucun outil spécial, infrastructure complexe ou stratégie de branchement spéciale requis
* Support for all package types that Composer supports

Inconvénients :

* Development within this GRA pattern is slightly more difficult at the start, there is a small learning curve
* Possible to deploy combinations of packages that were not developed in the same configuration, need for strict test procedures

## Set up Adobe Commerce with the Separate Packages GRA pattern

### La structure du répertoire

The final directory structure of a full Adobe Commerce installation with the Separate Packages GRA pattern looks like this:

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

* <https://github.com/AntonEvers/module-example-gra>
* <https://github.com/AntonEvers/module-example-3rdparty>
* <https://github.com/AntonEvers/module-example-local>

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

Add third-party packages to the GRA metapackage. If third-party code is not available to be installed from a Composer repository, then create a package for it. Create a Git repo, add the packages contents (everything that would be in app/code/Vendor/Package) and make sure that there is a valid composer.json file at the root of the repository. You can now install this package through Composer.

## Set up a private Composer repository

A private repository is not mandatory in global reference architecture. It makes deployments and installation faster, reduces repository configuration in composer.json, and it increases security. Credentials to other Composer repositories and the Adobe Commerce marketplace are stored in your private repository. No more sensitive credentials bundled with the code or on developers&#39; machines.

Additionally, some private repositories offer extra functionalities such as email notifications when one of your stores contains a security vulnerability in one of its dependencies.

The slowness issue is what occurs when you have multiple VCS repositories in composer.json. Each Composer repository needs to be read when doing upgrades and having 50 repositories for 50 packages has at least 50 times the overhead of just a single Composer repository.

![A diagram showing where slowness occurs when a composer repository is missing](/help/assets/global-reference-architecture/separate-packages-without-mirror-diagram.png){align="center"}

Include a Composer mirror in the form of a private Composer repository. The mirror contains a copy of all packages from other Composer repositories as well as all Git hosted packages. With a private Composer repository, you additionally get fine grained access control.

With Git synchronization, a private Composer repository automatically detects new packages in your Git repositories as well as new versions of existing packages.

You can host your own private repository with Satis: <https://composer.github.io/satis/>. See an example public repository at <https://antonevers.github.io/gra-composer-repository/>. This repo is used as the composer repository in the code examples. Additional measures are necessary to make a Satis repository private.

There are solutions that you can configure and forget about: Private Packagist <https://packagist.com/>, which is made by the same people that wrote Composer or JFrog Artifactory <https://jfrog.com/artifactory/>.

## Deliver code

With metapackages, there are 3 steps to deliver code.

1. Merge changes into packages and version the changed packages.
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

Appliquez le contrôle de version sémantique. Vous pouvez tout savoir sur le contrôle de version sémantique à l’adresse <https://semver.org/>. En particulier, la FAQ est une lecture incontournable. Avec le contrôle de version sémantique, les nombres dans « 1.0.0 » sont appelés MAJEUR.MINEUR.PATCH. Les versions mineures et de correctif d’un package doivent pouvoir être introduites en toute sécurité sans interrompre l’application.
You can automatically include patches and manually choose minor upgrades. Be aware that doing so costs you extra overhead by picking each minor change manually:

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

Of course, all this only works if you apply semantic versioning consistently, always. And not only in metapackages, but the requirements of your regular packages should define dependencies loosely too. If you have one strict dependency in your system, that package is limited to the strict definition.

Find these strict dependencies by typing composer depends \&lt;package name\>. See <https://getcomposer.org/doc/03-cli.md#depends-why> for more information.

## Branching strategy

You can use various branching strategies to support this global reference strategy pattern, provided that the main branch is the only branch where you version your packages. If you version across several branches, it introduces the risk of randomly losing functionality between versions. Only create stable versions on the main branch.

Only create feature branches in your package repositories. Not on your store installation repositories. Remain able to introduce any change to your store just by using Composer. Avoid the necessity of Git merges on the deployment repository.

Branch types that are common in branching strategies and repositories they should exist in:

**Feature branches**: exist in your package repositories, nowhere else.

**Release branches**: are created in any repository: packages, metapackages, store installation repositories. When you plan a release, group changes in release branches of packages before versioning them. Suppose you are preparing a release with the code name &quot;Unicorn&quot;. You can create a release-unicorn branch in packages with changes. Merge anything in there and then require &quot;dev-release-unicorn as 1.4.0&quot; in your metapackage. Learn more about aliases to see what is happening there: <https://getcomposer.org/doc/articles/aliases.md>.

**QA/Dev branches**: similar to release branches.

**Main branch**: must exist in every repository and should always be the branch that represents production or a production-ready state. La branche principale est l’emplacement où vous balisez le code vers les versions de publication.
Veillez à choisir une stratégie d’embranchement avec peu de frais de maintenance. Par exemple, la fusion de la branche principale en branches AQ, UAT, version ou développement après une publication de correctif est une tâche de maintenance planifiée. Plus il y a de packages, plus il y a de référentiels et plus les tâches répétitives sont lourdes.

Utilisez un outil tel que mixu/gr pour effectuer des opérations de routine sur plusieurs référentiels Git dans un lot : <https://github.com/mixu/gr>

## Conventions de dénomination

Avec le modèle GRA Separate Packages, les packages font partie de la base GRA si le métapaquet de base les requiert. Ajoutez ou supprimez des packages du métapaquet pour les déplacer dans et hors de la base.

Les métapaquets offrent une certaine flexibilité quant à la portée de l’installation des packages. Il est très important que les noms des packages ne contiennent aucun mot se rapportant à l’utilisation prévue du package. Le nom antonevers/module-gra-store-locator peut prêter à confusion lorsque vous décidez de retirer ce package de la base GRA. Évitez la portée (GRA, de base, locale). Évitez la région (EMEA, Espagne, Global). Évitez très certainement le nom du magasin pour lequel un package est créé. Choisissez des noms qui ne concernent que la fonctionnalité ajoutée dans le package. De cette façon, vous pouvez les réutiliser partout où vous le souhaitez, y compris dans des scénarios futurs imprévus. Le nom antonevers/module-store-locator serait excellent.

Assurez-vous que les packages associés s’affichent ensemble dans les vues d’ensemble. Noms de build du générique au spécifique. Ainsi, antonevers/module-b2b-tax-exempts au lieu d’antonevers/tax-exempted-module-b2b.

## Exemples de code

Les exemples de code de cet article de blog ont été combinés dans un ensemble de référentiels Git, que vous pouvez utiliser pour jouer avec la preuve de concept.

* Exemple de magasin de production : <https://github.com/AntonEvers/gra-separate-brand-x>
* Exemple de module de base : <https://github.com/AntonEvers/module-example-gra>
* Exemple de module tiers : <https://github.com/AntonEvers/module-example-3rdparty>
* An example local module: <https://github.com/AntonEvers/module-example-local>
* An example foundation metapackage: <https://github.com/AntonEvers/gra-meta-foundation>
* An example local metapackage (optional): <https://github.com/AntonEvers/gra-meta-brand-x>
* An example Composer repository: <https://github.com/AntonEvers/gra-composer-repository>
