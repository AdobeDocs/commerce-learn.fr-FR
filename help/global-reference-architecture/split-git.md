---
title: Configuration d’Adobe Commerce avec l’architecture de référence globale Git partagée
description: Découvrez comment configurer Adobe Commerce à l’aide de l’architecture de référence globale Split Git pour une gestion du code efficace et un déploiement rationalisé. ​
kt: 16725
doc-type: tutorial
audience: all
last-substantial-update: 2025-1-6
feature: Best Practices, Configuration, Install
badge: label="Contribution Tony Evers, architecte technique principal, Adobe" type="Informative" url="https://www.linkedin.com/in/evers-tony/" tooltip="Contribution Tony Evers"
topic: Architecture, Commerce, Development
role: Architect, Developer, User, Leader
level: Beginner, Intermediate
exl-id: ac544f77-8f5f-4ad1-92b2-bdf323100c13
source-git-commit: dacd43ef84dcb2c2633221a90642a469b2ff5a30
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 0%

---

# Modèle d’architecture de référence globale Split Git

Ce guide explique comment configurer Adobe Commerce avec le modèle d’architecture de référence globale (GRA) Git partagé.

Le modèle de Git Git partagé implique deux référentiels Git pour le développement et un référentiel Git par instance d’Adobe Commerce. Dans les exemples, on suppose que chaque instance représente une marque unique.

![Diagramme indiquant où le code est stocké dans un modèle de répartition GRA](/help/assets/global-reference-architecture/split-git-gra-pattern-diagram.png){align="center"}

## Avantages et inconvénients de ce modèle

Avantages :

- Réutilisation du code via un référentiel de code partagé
- Modèle GRA simple, adapté même aux équipes ayant des connaissances limitées en compositeur
- Outre les modules Adobe Commerce, les thèmes et les modules linguistiques, il est possible d’installer n’importe quel type de package Composer via ce modèle, y compris composer-plugin, compositeur-metapackage, magento2-component et patchs
- Publication possible par phases, en planifiant des versions pour les régions dans leurs propres fenêtres de maintenance.
- Prise en charge des balises Git à des fins d’administration et non de contrôle du déploiement
- Garantir que la combinaison de packages dans un déploiement en production est développée et testée dans cette configuration exacte

Inconvénients :

- Aucune flexibilité supplémentaire par rapport aux autres modèles GRA
- Impossible de mettre à niveau ou de rétrograder des modules individuels par instance. Mettez toujours à niveau ou rétrogradez la GRA dans son ensemble.
- Dans la plupart des cas, le modèle des emballages en vrac est mieux adapté car il est tout aussi simple, mais plus conventionnel

## Configuration d’Adobe Commerce avec le modèle de graphique Git partagé

### La structure du répertoire

Le modèle de Git Git partagé comporte deux types de référentiels : les référentiels de développement et les référentiels d’installation. Les référentiels de développement ne contiennent qu’une partie d’une installation Adobe Commerce complète. Les référentiels d’installation contiennent l’installation Adobe Commerce complète et sont utilisés pour le déploiement, mais pas pour le développement.

La structure de répertoires finale d’une installation Adobe Commerce complète avec le modèle de répartition Git GRA se présente comme suit :

```text
.
├── .gitignore
├── app/
│   └── etc/
│       └── config.php
├── composer.json
├── composer.lock
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

Les répertoires `app/code`, `app/i18n` et `app/design` sont délibérément omis, car le compositeur n’évalue pas le code dans ces répertoires. Par conséquent, les dépendances déclarées dans les packages ne sont pas automatiquement installées. Le modèle Git partagé GRA résout ce problème en installant tout le code personnalisé dans `packages/` et en traitant ce répertoire comme un référentiel de compositeur. Le compositeur associe symboliquement des packages dans `packages/` à `vendor/`.

### Préparation des référentiels Git

Créez 3 référentiels Git pour :

1. Une instance Adobe Commerce
2. Code tiers qui n’est pas installé via le compositeur
3. Vos personnalisations sous la forme de modules, thèmes, modules linguistiques, etc. ; votre GRA

Dans ce guide, les noms suivants sont utilisés pour ces référentiels :

1. gra-split-brand-x
2. gra-split-3rdparty
3. gra-split-gra

Tous les référentiels du modèle de Git partagé sont fusionnés en un seul. Pour que Git permette la fusion de plusieurs référentiels, ces trois référentiels ont besoin d’un historique partagé. Créez un projet Git avec une validation unique et envoyez-le à toutes les ressources distantes.

```bash
mkdir gra-split-brand-x
cd gra-split-brand-x
git init
git remote add origin git@github.com:AntonEvers/gra-split-brand-x.git
git remote add 3rdparty git@github.com:AntonEvers/gra-split-3rdparty.git
git remote add gra git@github.com:AntonEvers/gra-split-gra.git
touch .gitkeep
git add .gitkeep
git commit -m 'initialize repository'
git push -u origin main
git push 3rdparty main
git push gra main
```

L’envoi du `.gitkeep` de fichier temporaire à toutes les télécommandes crée la même validation initiale avec le même hachage de validation, ce qui crée un historique partagé. Chaque modification créée dans une télécommande peut être fusionnée avec les autres.

À partir de là, les référentiels divergent. Le référentiel gra-split-brand-x contient du code spécifique à la marque. Le référentiel tiers gra-split-3rd contient uniquement du code tiers. Le référentiel gra-split-gra contient uniquement votre base d’architecture de référence globale, qui comprend tout votre code personnalisé.

Installez Adobe Commerce dans le référentiel gra-split-brand-x.

```bash
composer create-project --no-install --repository-url=https://repo.magento.com/ magento/project-enterprise-edition temp
mv temp/composer.json ./
rmdir temp
git add composer.json
git commit -m 'install Adobe Commerce'
git push origin main
```

Créez des validations initiales dans les référentiels gra-split-3rdparty et gra-split-gra. Le moyen le plus simple est d’extraire ces référentiels dans des répertoires distincts.

```bash
cd ..
git clone git@github.com:AntonEvers/gra-split-3rdparty.git
git clone git@github.com:AntonEvers/gra-split-gra.git
cd gra-split-3rdparty
mkdir -p packages/3rdparty
touch packages/3rdparty/.gitkeep
git add packages/3rdparty/.gitkeep
git commit -m 'initialize 3rd party package storage'
git push origin main
cd ../gra-split-gra
mkdir -p packages/gra
touch packages/gra/.gitkeep
git add packages/gra/.gitkeep
git commit -m 'initialize GRA package storage'
git push origin main
```

Ces deux référentiels stockent les packages tiers et les packages GRA. Il peut y avoir un code exclusif à chaque instance d’Adobe Commerce. Créez un emplacement pour stocker ces packages locaux dans le référentiel gra-split-brand-x.

```bash
cd ../gra-split-brand-x
mkdir -p packages/local
touch packages/local/.gitkeep
git add packages/local/.gitkeep
git commit -m 'initialize local package storage'
git push origin main
```

### Où stocker différents types de code

Adobe Commerce est une application de compositeur. La méthode d’installation préférée consiste toujours à utiliser les référentiels du compositeur. Vous pouvez stocker des modules tiers dans le référentiel tiers uniquement si un fournisseur de modules ne propose pas d’installation par le biais d’un référentiel de compositeur. L’emplacement idéal pour le code personnalisé est le référentiel GRA. Lorsqu’un module n’est utilisé que par une seule instance spécifique, il devient du code local.

En résumé :

- **Adobe Commerce** : stocké dans un référentiel de compositeur.
- **Modules tiers** : stockés dans un référentiel Composer.
- **Option de secours des modules tiers** : stocké dans le référentiel Git gra-split-3rdparty.
- **code de base GRA** : stocké dans le référentiel Git gra-split-gra.
- **Code local** : stocké dans le référentiel Git gra-split-brand-x.

### Connecter le stockage de package au compositeur

Le compositeur peut traiter le répertoire des packages comme un référentiel du compositeur. Informez le compositeur de l’emplacement des packages dans le répertoire des packages.

```json
"repositories": [
  {"type": "path", "url": "packages/local/*/*"},
  {"type": "path", "url": "packages/gra/*/*"},
  {"type": "path", "url": "packages/3rdparty/*/*"},
  {"type": "composer", "url": "https://repo.magento.com"}
]
```

Le compositeur recherche les fichiers composer.json à deux niveaux de profondeur dans les trois répertoires de stockage. Créez des sous-répertoires à l’intérieur des trois répertoires de stockage de code, exactement comme ils apparaîtraient dans le répertoire `vendor/`.

Par exemple : si un package est normalement installé dans `vendor/example-corp/module-example/`, vous pouvez le stocker dans `packages/3rdparty/example-corp/module-example/`. Le compositeur associe symboliquement le package au `vendor/example-corp/module-example/` lorsque vous en avez besoin.

Utilisez l’espace de noms et le nom du package du compositeur comme structure de répertoires. Par exemple : un module qui existe traditionnellement dans `app/code/MyCorp/MyCustomization/` porte le nom `my-corp/module-my-customization` dans composer.json. Conserver ce kit en `packages/gra/my-corp/module-my-customization`.

### Inclure de nouveaux packages dans les référentiels d’instance

Fusionnez les packages des télécommandes tierces et GRA dans le référentiel gra-split-brand-x.

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
git push origin main
```

Il en résulte la structure de répertoires suivante :

```text
.
├── composer.json
└── packages/
    ├── 3rdparty/
    ├── gra/
    └── local/
```

Les modifications apportées aux référentiels tiers et de base GRA sont fusionnées dans les référentiels de marque. Ainsi, le code tiers et GRA n’est conservé qu’à un seul endroit. Déplacez les modifications vers les marques avec une fusion Git.

Adobe Commerce ne reconnaît pas automatiquement les nouveaux modules. L’exécution du compositeur nécessite l’ajout d’un nouveau package après une fusion. Exécutez la mise à jour du compositeur chaque fois que vous mettez à jour l’un de vos packages après une fusion.

### Installer des exemples de modules

Pour valider le concept, installez des exemples de modules pour voir comment fonctionne le modèle GRA.

Exécutez `composer install` et `bin/magento install` avant de passer à autre chose.

Il existe 3 modules de test pour sur GitHub :

1. [module-example-local](https://github.com/AntonEvers/module-example-local)
2. [module-example-gra](https://github.com/AntonEvers/module-example-gra)
3. [module-example-3rdparty](https://github.com/AntonEvers/module-example-3rdparty)

### Installation d’un module local

L’ajout d’un module au pool de code local est simple. Téléchargez et extrayez le module . Exigez-le avec le compositeur . Activez-le avec `bin/magento` et validez les fichiers dans le référentiel de marque.

```bash
cd gra-split-brand-x
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

Si la sortie ci-dessus s’affiche, vous pouvez la valider en toute sécurité dans le référentiel de marque.

```bash
git add packages/local/antonevers/module-local app/etc/config.php composer.json composer.lock 
git commit -m 'add local module'
git push origin main
```

### Installation et développement d’un module de base GRA

L’ajout d’un module au référentiel GRA est différent de l’installation de modules locaux. Par défaut, les validations sont ajoutées à l’origine/principal, qui est le référentiel gra-split-brand-x. Les modifications apportées aux modules GRA doivent être transmises au référentiel gra-split-gra et fusionnées dans le référentiel gra-split-brand-x par la suite.

### Créer un environnement de développement

Créez un environnement de développement avec une combinaison de tous les pools de code en un seul endroit. Vous pouvez envoyer le code vers le référentiel local, GRA et tiers individuellement par le biais de liens symboliques. Commencez par créer un nouveau répertoire de développement en regard de vos répertoires de marque, GRA et de référentiel tiers.

```text
.
├── gra-development    # <---
├── gra-split-3rdparty
├── gra-split-brand-x
└── gra-split-gra
```

```bash
cd ..
mkdir gra-development
cd gra-development
cp ../gra-split-brand-x/composer.json ../gra-split-brand-x/composer.lock .
mkdir packages
ln -s ../../gra-split-brand-x/packages/local/ packages/
ln -s ../../gra-split-3rdparty/packages/3rdparty/ packages/
ln -s ../../gra-split-gra/packages/gra/ packages/
```

Il en résulte la structure de répertoires suivante :

```text
.
├── packages/
│ ├── 3rdparty -> ../../gra-split-3rdparty/packages/3rdparty/
│ ├── gra -> ../../gra-split-gra/packages/gra/
│ └── local -> ../../gra-split-brand-x/packages/local/
├── composer.lock
└── composer.json
```

Exécutez `composer install` et `bin/magento install` dans le répertoire gra-development.

Il est désormais possible de valider les modifications directement à partir des répertoires `packages/3rdparty`, `packages/gra` et `package/local`. Git valide les modifications apportées au référentiel Git vers lequel les répertoires sont liés. Il est important que la commande de validation Git soit émise dans le répertoire `packages/3rdparty`, `packages/gra` ou `package/local`. N’exécutez pas la validation Git à la racine du projet.

### Installer des exemples de modules

Installez les exemples de module tiers et GRA dans les répertoires des packages.

```bash
cd packages/gra
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-gra/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-gra-main module-gra
git add module-gra
 
cd ../../3rdparty
mkdir antonevers
cd antonevers
curl -OL https://github.com/AntonEvers/module-example-3rdparty/archive/refs/heads/main.zip
unzip main.zip
rm main.zip
mv module-example-3rdparty-main module-3rdparty
git add module-3rdparty
 
cd ../../..
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
bin/magento test:gra
bin/magento test:3rdparty
```

Cette dernière commande doit générer la sortie suivante pour prouver que le module est installé et fonctionne :

```bash
GRA module is installed successfully and working!
3rd party module is installed successfully and working!
```

Si la sortie ci-dessus s’affiche, vous pouvez la valider en toute sécurité dans le référentiel de marque. Exécutez `git remote -v` pour vérifier que vous vous engagez sur la bonne télécommande.

```bash
cd packages/gra
git remote -v 
origin git@github.com:AntonEvers/gra-split-gra.git (fetch)
origin git@github.com:AntonEvers/gra-split-gra.git (push)
git add antonevers/module-gra
git commit -m 'add GRA module'
git push origin main
 
cd ../3rdparty
git remote -v 
origin git@github.com:AntonEvers/gra-split-3rdparty.git (fetch)
origin git@github.com:AntonEvers/gra-split-3rdparty.git (push)
git add antonevers/module-3rdparty
git commit -m 'add third-party module'
git push origin main
```

### Diffusion du code vers les instances

Fusionnez les référentiels GRA et tiers avec le référentiel gra-split-brand-x pour diffuser le code vers une instance Adobe Commerce. Exécutez `composer require`, `bin/magento module:enable` et validez le résultat.

```bash
cd gra-split-brand-x
git fetch - all
git merge gra/main 3rdparty/main
composer require antonevers/module-gra:@dev antonevers/module-3rdparty:@dev
bin/magento module:enable AntonEvers_Gra AntonEvers_ThirdParty
git add app/etc/config.php composer.lock composer.json
git commit -m 'install GRA and third-party modules'
git push origin main
```

## Stratégie d’embranchement

Ce modèle GRA fonctionne avec toutes les stratégies d’embranchement, si vous reflétez la stratégie d’embranchement des référentiels de magasin dans vos référentiels tiers et GRA. Pour les versions , créez une branche de version portant le même nom dans les trois référentiels. Fusionnez les branches de version sur le référentiel de magasin pendant la préparation de la version.

Parfois, vous disposez d’une branche de ticket qui nécessite la modification du code local et du code tiers ou du code GRA. Dans ce cas, les branches de ticket doivent être créées dans tous les référentiels associés.

Ne fusionnez jamais les validations tierces et GRA dans le référentiel de marque au sein des branches de ticket. Au lieu de cela, consultez les branches appropriées dans votre environnement de développement pour chaque pool de code. La fusion dans le référentiel de marque n’est effectuée que lors de la composition de la version ou de la composition d’une branche d’assurance qualité.

## Exemples de code

Les exemples de code de cet article sont disponibles sous la forme d’un ensemble de référentiels Git, que vous pouvez utiliser pour tester la validation de principe.

- Exemple de magasin de production : <https://github.com/AntonEvers/gra-split-brand-x>
- Référentiel de code tiers : <https://github.com/AntonEvers/gra-split-3rdparty>
- Le référentiel de code GRA : <https://github.com/AntonEvers/gra-split-gra>
- Exemple de module local : <https://github.com/AntonEvers/module-example-local>
- Exemple de module GRA : <https://github.com/AntonEvers/module-example-gra>
- Exemple de module tiers : <https://github.com/AntonEvers/module-example-3rdparty>
