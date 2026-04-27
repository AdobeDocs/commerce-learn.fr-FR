---
title: Monorepo d’architecture de référence globale
description: Découvrez comment utiliser l’approche monorepo pour une architecture de référence mondiale afin d’établir une expérience commerciale évolutive et résiliente
jira: KT-16728
doc-type: tutorial
duration: 441
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Contribution de Tony Evers, architecte technique principal, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contribution Tony Evers"
topic: Architecture, Commerce, Development
old-role: Architect, Developer
role: Developer, User, Leader
level: Experienced
exl-id: ebdc13cf-c452-4728-af00-c3ea1149c2fa
TQID: https://experienceleague.adobe.com/22ayPTG7ZgpcWr5l53Ide2sZeGTN-9P0jept0ZQ6njQ
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: dac87252-6066-4d6e-a9d2-f6d84c323de7id: e8818fe6-9c8b-4bc0-9ef8-377a10b7bc75
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: f8a45b24-4be7-4f1b-909b-60d06b483a20id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1418
ht-degree: 0%

---

# Modèle d’architecture de référence globale Monorepo

{{only-for-on-prem-commerce-cloud}}

Ce guide explique comment configurer Adobe Commerce avec le modèle d’architecture de référence globale (GRA) Monorepo.

Le modèle GRA Monorepo implique un référentiel Git unique pour héberger toutes les personnalisations courantes. Ce référentiel Git unique est exposé via le compositeur en tant que packages de compositeur distincts.

![Diagramme indiquant où le code est stocké dans un modèle GRA monorepo](/help/assets/global-reference-architecture/monorepo-gra-pattern-diagram.png){align="center"}

## Avantages et inconvénients de ce modèle

Avantages :

* Idéal pour les tests fonctionnels
* Réutilisation du code dans des référentiels de code partagés
* Flexibilité totale dans l&#39;installation des packages, chaque package GRA peut être mis à niveau, rétrogradé ou rétroporté individuellement
* Prise en charge complète du contrôle de version sémantique
* Aucun outil spécial, infrastructure complexe ou stratégie de branchement spéciale requis
* Prise en charge de tous les types de packages pris en charge par le compositeur
* Idéal pour les environnements éphémères, qui sont facultatifs, mais très utiles pour les équipes de diffusion à volume élevé

Inconvénients :

* Possible to deploy combinations of packages that were not developed in the same configuration, need for strict test procedures
* The monorepo GRA pattern can be complex at the start. Assign a lead that helps the team work with the system

## Set up Adobe Commerce with the Separate Packages GRA pattern

### La structure du répertoire

The final directory structure of a full Adobe Commerce installation with the Separate Packages GRA pattern has this directory structure:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── packages/
│   └── ...
├── composer.json
└── composer.lock
```

A production Git repository has this directory structucture:

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
└── composer.lock
```

The difference is that the production instances install from Composer, where the monorepo holds its own copy of every package inside the packages directory.

### Prepare a production repository

Créez un référentiel pour la première instance d’Adobe Commerce, qui représente un magasin web pour Brand X.

```bash
mkdir gra-monorepo-brand-x
cd gra-monorepo-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

Installez Adobe Commerce avec `bin/magento setup:install`. Commit the resulting `app/etc/config.php` and the composer files. Composer manages anything else so nothing else should be in Git.

### Prepare the monorepo repository

The monorepo repository starts with the same steps.

```bash
mkdir gra-monorepo 
cd gra-monorepo
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
```

Install Adobe Commerce with `bin/magento setup:install`, commit and push.

```bash
git init
git remote add origin git@github.com:AntonEvers/gra-monorepo.git 
git add composer.json composer.lock app/etc/config.php
git commit -m 'initialize monorepo GRA development repository'
git push -u origin main
```

### Prepare for monorepo development

For monorepo development, make the following changes to your composer.json file:

1. Change the name and description of the package so that it is clear that this package is your monorepo package.
1. Delete the version attribute from composer.json, because the version is managed using Git tags for this repository.
1. Replace the require section with a meta package which is created later.
1. Change minimum stability to dev.
1. Add a path type repository that points to `packages/*/*` to host monorepo packages, including branch aliases for each package it contains
1. Add a branch alias for the project itself

The following Git diff shows the difference between a clean Adobe Commerce install and the changes mentioned above:

```diff
@@ -1,6 +1,6 @@
 {
-    "name": "magento/project-enterprise-edition",
-    "description": "eCommerce Platform for Growth (Enterprise Edition)",
+    "name": "antonevers/gra-monorepo",
+    "description": "Monorepo repository for Global Reference Architecture development",
     "type": "project",
     "license": [
         "proprietary"
@@ -15,11 +15,8 @@
         "preferred-install": "dist",
         "sort-packages": true
     },
-    "version": "2.4.7-p3",
     "require": {
-        "magento/product-enterprise-edition": "2.4.7-p3",
-        "magento/composer-dependency-version-audit-plugin": "~0.1",
-        "magento/composer-root-update-plugin": "^2.0.4"
+        "antonevers/gra-meta-brand-x": "self.version"
     },
     "autoload": {
         "exclude-from-classmap": [
@@ -69,16 +66,33 @@
             "Magento\\Tools\\Sanity\\": "dev/build/publication/sanity/Magento/Tools/Sanity/"
         }
     },
-    "minimum-stability": "stable",
+    "minimum-stability": "dev",
     "prefer-stable": true,
     "repositories": [
         {
+            "type": "path",
+            "url": "packages/*/*",
+            "options": {
+                "versions": {
+                    "antonevers/gra-meta-brand-x": "1.4.x-dev",
+                    "antonevers/gra-meta-foundation": "1.4.x-dev",
+                    "antonevers/gra-component-foundation": "1.4.x-dev",
+                    "antonevers/module-gra": "1.4.x-dev",
+                    "antonevers/module-3rdparty": "1.4.x-dev",
+                    "antonevers/module-local": "1.4.x-dev"
+                }
+            }
+        },
+        {
             "type": "composer",
             "url": "https://repo.magento.com/"
         }
     ],
     "extra": {
-        "magento-force": "override"
+        "magento-force": "override",
+        "branch-alias": {
+            "dev-main": "1.4.x-dev"
+        }
     }
 }
```

### Use metapackages

Download the example code from [AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation) on GitHub to get the metapackages and the sample modules that are used in this example.

Composer metapackages bundle multiple composer packages together under a single package. When a metapackage is required, all packages it bundles are automatically installed through the Composer require section of the metapackage.

For this example, there are two metapackages:

1. **antonevers/gra-meta-brand-x**: A metapackage that contains everything that makes up &quot;Brand X&quot;
1. **antonevers/gra-meta-foundation**: A metapackage that contains everything that is always installed in any brand

The brand metapackage requires the foundation metapackage. When brand metapackage is required, the foundation metapackage is automatically required as well. Please see the two composer.json files of the metapackages to see how they relate:

antonevers/gra-meta-brand-x:

```json
{
    "name": "antonevers/gra-meta-brand-x",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-meta-foundation": "^1.4",
        "antonevers/module-local": "^1.4"
    }
}
```

antonevers/gra-meta-foundation:

```json
{
    "name": "antonevers/gra-meta-foundation",
    "type": "metapackage",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {
        "antonevers/gra-component-foundation": "^1.4",
        "antonevers/module-gra": "^1.4",
        "antonevers/module-3rdparty": "^1.4",
        "magento/composer-dependency-version-audit-plugin": "~0.1",
        "magento/composer-root-update-plugin": "^2.0.4",
        "magento/product-enterprise-edition": "2.4.7-p3"
    }
}
```

Metapackages are a good way to organize code that belongs together. Use metapackages to define groups of packages that are regional, global, brand-specific or any grouping that makes sense for you. If you maintain multiple installations of Adobe Commerce, matapackages a safe and versatile way of defining the context in which a package is expected.

Metapackages exist in the monorepo inside the `packages` directory. There, the directory structure of the `vendor` is mirrored:

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       │   └── composer.json
│       └── gra-meta-foundation
│           └── composer.json
├── composer.json
└── composer.lock
```

### Add and develop modules

Modules in the monorepo exist in the `packages` directory. This way Composer can find them through the path type repository.

Download the example code from [AntonEvers/gra-meta-foundation](https://github.com/AntonEvers/gra-meta-foundation) on GitHub to get the metapackages and the sample modules that are used in this example.

```text
.
├── packages/
│   └── antonevers
│       ├── gra-meta-brand-x
│       ├── gra-meta-foundation
│       ├── module-3rdparty
│       ├── module-gra
│       └── module-local
├── composer.json
└── composer.lock
```

You can have multiple namespaces inside the `packages` directory if needed.

Development takes place in the packages directory. Symlinks to the packages inside the `packages` directory are created in the `vendor` directory once you run `composer update`. This way, the code becomes part of the Adobe Commerce installation.

Run `bin/magento module:enable --all` or for only specific modules to enable the modules that were added.

By now you should have a working Adobe Commerce installation with the three sample modules installed and working. You can validate if the modules are installed and working by running the commands:

```bash
bin/magento test:gra
bin/magento test:3rdparty
bin/magento test:local
```

### Achieve automated package creation

There are multiple options to achieve automated package creation. Some options are:

1. [Private Packagist](https://packagist.com/)
1. [Simplyfy Monorepo Builder](https://github.com/symplify/monorepo-builder)
1. Build your own solution

[Private Packagist](https://packagist.com/) automates recognizing packages in the Git monorepo and exposes them through Composer. It is compatible with Adobe Commerce, fast, low-maintenance and error-prone, which is why this guide focuses on the Private Packagist option.

It is beyond the scope of this guide to explain how to set up Private Packagist, please see the [docs](https://packagist.com/docs).

There is the possibility to turn a package into a monorepo once you have set up organization syncing and your Git repositories are automatically syncing to Private Packagist.

First, go to the packages tab and find the monorepo:

![Private Packagist screen shot with the monorepo package visible in the packages screen](/help/assets/global-reference-architecture/packagist-packages-before-multi-package.png){align="center"}

Click on the monorepo package and click &quot;Edit&quot; in the details screen, which takes you to the following page:

![Private Packagist screen shot with the monorepo package edit page](/help/assets/global-reference-architecture/packagist-packages-edit.png)

Underneath the first input field, there is a link saying: Create a multi-package repository. Click this link.

![Private Packagist screen shot with the multi-package configuration](/help/assets/global-reference-architecture/packagist-packages-multi-package.png)

Define the location where composer packages can be found inside your monorepo. In the example, the location is `packages/**/composer.json`. Change the location to limit or broaden where Private Packagist searches for packages to extract.

L’onglet Packages affiche tous les packages trouvés après l’enregistrement et le monorepo lui-même n’est plus visible en tant que package de compositeur :

![Capture d’écran du Packagiste privé avec tous les packages monorepo visibles dans l’écran des packages](/help/assets/global-reference-architecture/packagist-packages-after-multi-package.png)

Une version est créée dans le compositeur pour chaque package du monoréférentiel, pour chaque balise ou branche créée sur le monoréférentiel dans Git.

## Installation des packages dans l’environnement de production

Suivez les instructions de Private Packagist pour ajouter Private Packagist en tant que référentiel de compositeur. Private Packagist peut et doit être utilisé comme miroir pour tous vos référentiels Composer et Git, y compris packagist.org. Ainsi, les informations d’identification ne doivent pas être partagées avec les développeurs et vous avez un contrôle total sur chaque package. Cet exemple ne suit pas cette bonne pratique, car il exposerait la base de code Adobe Commerce publiquement.

Téléchargez [GRA Monorepo Brand X](https://github.com/AntonEvers/gra-monorepo-brand-x) sur GitHub pour voir un exemple de magasin de production.

Dans le magasin de production, il n’existe aucun répertoire `packages` et tous les packages sont installés via le compositeur. Le seul package requis est :

```json
    "require": {
        "antonevers/gra-meta-brand-x": "^1.0"
    },
```

Pourtant, l’intégralité d’Adobe Commerce et de GRA est installée selon les exigences de ce métapaquet.

## Contrôle de version

Tous les packages du monorepo reçoivent la même version que le monorepo lui-même. Considérez cela comme la publication de nouvelles versions de l’ensemble de l’application. En production, cependant, vous pouvez installer un mélange de packages provenant de différentes versions de monorepo.

## Environnements éphémères

Si vous utilisez des environnements éphémères ou si vous envisagez de les utiliser, le monorepo est un excellent choix. Chaque version et branche du monorepo contient tous les fichiers de modules Adobe Commerce, tiers et personnalisés. Avec une installation complète dans chaque branche, il est possible d’exécuter tous les types de tests, y compris les tests fonctionnels. Avec d’autres configurations GRA, telles que les packages distincts ou les packages en masse GRA, vous devez d’abord créer un environnement Adobe Commerce fonctionnel avant de pouvoir exécuter des tests fonctionnels. Du point de vue de DevOps, monorepo le rend beaucoup plus simple.

## Exemples de code

Les exemples de code de cet article ont été combinés dans un ensemble de référentiels Git, que vous pouvez utiliser pour jouer avec la preuve de concept.

* Exemple de référentiel monorepo : <https://github.com/AntonEvers/gra-monorepo>
* Exemple de magasin de production : <https://github.com/AntonEvers/gra-monorepo-brand-x>
