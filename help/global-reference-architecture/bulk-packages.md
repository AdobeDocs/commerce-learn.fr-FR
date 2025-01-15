---
title: Optimisation d’Adobe Commerce avec l’architecture de référence globale des packages en bloc
description: Découvrez comment configurer Adobe Commerce à l’aide de l’architecture de référence globale des packages en bloc pour une gestion du code et un contrôle de version efficaces.
jira: KT-16726
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
topic: Architecture, Commerce, Development
badge: label="Contribution Tony Evers, architecte technique principal, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contribution Tony Evers"
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac63e31e-3047-410a-a6f9-a578b495bd8c
source-git-commit: a182b97b7d9a8bf114944d5d920afb2328adbc18
workflow-type: tm+mt
source-wordcount: '1172'
ht-degree: 0%

---

# Modèle d’architecture de référence globale des packages en bloc

Ce guide explique comment configurer Adobe Commerce avec le modèle d’architecture de référence globale (GRA) de packages en bloc.

Le modèle GRA des packages en bloc implique un référentiel Git unique pour héberger toutes les personnalisations courantes. Ce référentiel Git unique est exposé via le compositeur sous la forme d’un package de compositeur unique, qui contient plusieurs modules Adobe Commerce.

![Diagramme indiquant où le code est stocké dans un modèle GRA de packages en masse](/help/assets/global-reference-architecture/bulk-gra-pattern-diagram.png){align="center"}

## Avantages et inconvénients de ce modèle

Avantages :

- Réutilisation du code via un référentiel de code partagé
- Flexibilité pour installer différentes versions historiques de la GRA sur différentes instances, ce qui permet des versions par phases
- Flexibilité pour rétroporter et maintenir plusieurs versions majeures de la GRA
- Prise en charge du contrôle de version sémantique de la GRA
- En toute simplicité, les développeurs n’ont pas besoin de davantage de compétences que dans les modèles de développement standard d’un magasin unique
- Aucun outil spécial, infrastructure complexe ou stratégie de branchement spéciale requis
- La combinaison de packages dans une version est toujours développée et testée ensemble

Inconvénients :

- Seule une mise à niveau de l’ensemble de la GRA est possible, y compris tous les packages qu’elle contient.
- Pas de prise en charge dans le package en bloc GRA pour les packages de compositeurs autres que les modules Adobe Commerce, les modules linguistiques et les thèmes, donc pas de métapaquets, de packages de composants magento2, de modules externes de compositeur et de correctifs

## Configuration d’Adobe Commerce avec le modèle de graphique Git partagé

### La structure du répertoire

La version GRA des packages en bloc installe tout le code réutilisable via les référentiels du compositeur. Le code spécifique à une seule instance réside dans le référentiel Git de cette instance. Le code spécifique à l’instance n’est pas réutilisé dans d’autres instances d’Adobe Commerce.

La structure de répertoires finale d’une installation Adobe Commerce complète avec le modèle GRA des packages en bloc ressemble à ceci :

```text
.
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    └── local/
```

Les répertoires `app/code`, `app/i18n` et `app/design` sont délibérément omis, car le compositeur n’évalue pas le code dans ces répertoires. Par conséquent, les dépendances déclarées dans les packages ne sont pas automatiquement installées. Le modèle GRA des packages en bloc résout ce problème en installant du code personnalisé dans `packages/` et en traitant ce répertoire en tant que référentiel de compositeur. Le compositeur associe symboliquement des packages dans `packages/` à `vendor/`.

### Préparation des référentiels Git

Créez deux référentiels Git pour le code GRA partagé et pour le premier magasin. Commencez par le référentiel GRA, qui présente la structure de fichiers suivante :

Il en résulte la structure de répertoires suivante :

```text
.
├── composer.json
└── src/
    ├── GraOne/
    │   ├── composer.json
    │   └── registration.php
    ├── GraTwo/
    │   ├── composer.json
    │   └── registration.php
    └── registration.php
```

Cette structure de répertoires ne mentionne que le fichier composer.json et le fichier registration.php des modules GraOne et GraTwo. En réalité, il y a plus de fichiers à l’intérieur de ces modules.

Exécutez les commandes suivantes pour lancer le référentiel Git :

```bash
mkdir gra-bulk-foundation
cd gra-bulk-foundation
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-foundation.git
vim composer.json # see code snippet below for contents
git add composer.json
git commit -m 'initialize GRA foundation repository'
git push -u origin main
```

Le contenu du fichier composer.json :

```json
{
    "name": "antonevers/gra-bulk-foundation",
    "description": "Shared code repository",
    "type": "library",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "require": {},
    "autoload": {
        "files": [
            "src/registration.php"
        ],
        "psr-0": {
            "": "src/"
        }
    }
}
```

Remplacez l’espace de noms du package composer.json par votre propre espace de noms.

Le contenu du fichier src/registration.php :

```php
<?php

declare(strict_types=1);

$pathList[] = dirname(__DIR__) . '/src/*/*/registration.php';
foreach ($pathList as $path) {
    $files = glob($path, GLOB_NOSORT | GLOB_ERR);
    if ($files === false) {
        throw new RuntimeException('glob() returned error while searching in \'' . $path . '\'');
    }
    foreach ($files as $file) {
        require_once $file;
    }
}
```

Le fichier registration.php recherche d&#39;autres fichiers registration.php dans les modules Adobe Commerce et les exécute.

Créez les deux exemples de module à l’aide du code dans <https://github.com/AntonEvers/gra-bulk-foundation>. Le compositeur n’évalue pas les fichiers composer.json dans les exemples de modules. C&#39;est une habitude. Si vous décidez de déplacer les modules vers un autre emplacement, les fichiers composer.json redeviennent nécessaires.

### Configurer le référentiel de magasin

Le référentiel de déploiement contient l’ensemble de l’installation d’Adobe Commerce, y compris le code GRA. Créez le référentiel de déploiement :

```bash
mkdir gra-bulk-brand-x
cd gra-bulk-brand-x
composer create-project --repository-url=https://repo.magento.com/ magento/project-enterprise-edition .
git init
git remote add origin git@github.com:AntonEvers/gra-bulk-brand-x.git 
git add composer.json composer.lock
git commit -m 'initialize Brand X repository'
git push -u origin main
```

Installez Adobe Commerce avec `bin/magento setup:install`. Installez les exemples de module GRA dans le référentiel de déploiement avec le compositeur :

```bash
composer config repositories.gra-foundation vcs git@github.com:AntonEvers/gra-bulk-foundation.git
composer require antonevers/gra-bulk-foundation:@dev
bin/magento module:enable AntonEvers_GraOne AntonEvers_GraTwo
bin/magento test:gra-one
bin/magento test:gra-two
git add app/etc/config.php composer.json composer.lock
git commit -m 'install GRA foundation'
git push origin main
```

Cette dernière commande doit générer la sortie suivante pour prouver que le module est installé et fonctionne :

```bash
GRA One module is installed successfully and working!
GRA Two module is installed successfully and working!
```

Vous pouvez créer plusieurs packages en bloc pour organiser le code. Par exemple, un package tiers en bloc pour le code tiers qui n’est pas disponible via le compositeur. Tout ce que vous installez traditionnellement dans `app/code` doit maintenant se trouver dans le répertoire `src/` du package en bloc. Une exception à cette règle est le code qui n’est utilisé que dans une seule instance. Ces packages sont appelés packages locaux.

### Installation des packages locaux

Le référentiel de déploiement héberge les packages locaux. Ils ne vivent pas dans le package de masse GRA. L’emplacement des packages locaux n’est pas `app/code` mais `packages/local`. Demandez au compositeur de traiter ce répertoire comme un référentiel :

```bash
composer config repositories.local path 'packages/local/*/*'
git add composer.json
git commit -m 'initialize local composer package storage'
git push origin main
```

Ajoutez l’exemple de module hébergé sur <https://github.com/AntonEvers/module-example-local> :

```bash
mkdir -p packages/local
cd packages/local
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-local/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-local-main module-local
git add module-local
cd ../../..
composer require antonevers/module-local:@dev
bin/magento module:enable AntonEvers_Local
bin/magento test:local
```

Cette dernière commande doit générer la sortie suivante pour prouver que le module est installé et fonctionne :

```bash
Local module is installed successfully and working!
```

Validez le module local dans le référentiel de marque :

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

## Présentation des emplacements de code

Ce n’est que si le tiers ne propose pas d’installation par le biais d’un référentiel de compositeur que vous pouvez stocker les modules tiers dans le répertoire `src/` de votre référentiel de base ou dans un package tiers en bloc dédié.

- **Adobe Commerce core** : disponible via repo.magento.com.
- **Modules tiers** : disponibles via la Marketplace ou le référentiel Composer d’un fournisseur.
- **Option de secours des modules tiers** : stocké en `src/` d&#39;un package en masse.
- **code de base GRA** : stocké en `src/` du package de base en masse.
- **Code local** : stocké dans le répertoire `packages/local` du référentiel de déploiement.

## Développement d’un module GRA

Installez le package en bloc à partir de la source pour activer Git dans le répertoire des packages en bloc :

```bash
rm -r vendor/antonevers/gra-bulk-foundation
composer install --prefer-source
```

Le package en bloc a été extrait à l’aide de Git. Lorsque vous accédez au répertoire `vendor/antonevers/gra-bulk-foundation`, vous accédez également au référentiel Git de création en masse. Vous pouvez créer, extraire et fusionner des branches dans ce répertoire.

Ajoutez des dépendances du compositeur au fichier composer.json à la racine du package en bloc GRA, qui est le seul fichier dans le package en bloc que le compositeur évalue.

## Inclure des modules tiers dans le package en bloc GRA

Ajoutez des packages tiers dans la section obligatoire du fichier composer.json à la racine des bases GRA pour les ajouter à votre GRA. Ainsi, les packages sont toujours installés dans toutes vos instances via le compositeur.

## Diffuser votre code

Pour diffuser du code vers la branche principale, il existe 2 chemins d’accès. Tout d’abord, les modules locaux, qui sont fusionnés à la branche principale. Exécutez la mise à jour du compositeur pour ces modules. N’autorisez pas les développeurs à mettre à jour composer.lock dans leurs branches de ticket pour réduire les conflits. Mettez uniquement à jour le fichier composer.lock dans les branches d’évaluation et de production, ce qui réduit le risque de conflits.

Deuxièmement, les packages de GRA en vrac, qui sont fusionnés dans la branche principale du référentiel de GRA en vrac. Vous pouvez ensuite ajouter une balise Git à la branche principale pour créer le contrôle de version du package du compositeur. Nécessite votre nouvelle version du package de masse GRA dans le fichier composer.json du référentiel de déploiement pour l’installer.

## Stratégie d’embranchement

Ce modèle GRA fonctionne avec toutes les stratégies d’embranchement à condition de refléter la stratégie d’embranchement des référentiels de déploiement dans votre référentiel en bloc GRA. Pour les versions, créez une branche de version portant le même nom dans les deux référentiels. Pour le développement, créez une branche de ticket dans les deux référentiels.

Dans les branches de ticket, vous ne devriez presque jamais avoir à mettre à jour le fichier composer.lock. Il vous suffit d’examiner les branches appropriées dans votre environnement de développement pour le magasin et le référentiel de base GRA avec Git. Une exception se produit lorsque vous mettez à jour les exigences du fichier GRA foundation composer.json . La mise à niveau de GRA Foundation dans le référentiel de déploiement est effectuée uniquement lors de la création de la version ou de la création d’une branche d’assurance qualité.

## Exemples de code

Les exemples de code de cet article sont disponibles sous la forme d’un ensemble de référentiels Git, que vous pouvez utiliser pour tester la validation de principe.

- Exemple de magasin de production : <https://github.com/AntonEvers/gra-bulk-brand-x>
- Le référentiel de code GRA : <https://github.com/AntonEvers/gra-bulk-foundation>
- Exemple de module local : <https://github.com/AntonEvers/module-example-local>
