---
title: Détecter les adresses IP
description: Découvrez comment détecter des adresses IP pour les environnements Adobe Commerce Cloud afin d’améliorer la sécurité et de rationaliser la communication serveur
feature: Cloud, Configuration
topic: Commerce, Development, Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 0
last-substantial-update: 2025-04-07T00:00:00Z
jira: KT-17553
exl-id: beb0a6e1-e6b1-4ec0-976c-77a22a27e8a2
source-git-commit: b015b9c64be631b43ad63d180c003dda8fdd198a
workflow-type: tm+mt
source-wordcount: '1095'
ht-degree: 0%

---

# Détection des adresses IP pour différents environnements

Découvrez comment détecter des adresses IP pour différents environnements dans un projet Adobe Commerce Cloud. En utilisant une série de commandes, notamment Adobe Commerce CLI, sed, xargs, dig, grep et sort -u, les utilisateurs peuvent identifier les adresses IP pour les environnements de développement, d’évaluation et de production.

## À qui s&#39;adresse cette vidéo ?

* Développeurs : cherchent à comprendre comment collecter les adresses IP pour le projet Adobe Commerce Cloud.
* Opérations de développement et équipes de sécurité devant restreindre l’accès aux systèmes tiers ou principaux

## Contenu vidéo {#video-content}

* Découvrez comment découvrir l’adresse IP de n’importe quel environnement dans Adobe Commerce Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3457493/?learn=on)

## Commande pour obtenir l&#39;adresse IP

Notez que vous devez utiliser votre ID de projet et le nom de l’environnement au lieu des informations d’espace réservé.  Il peut également être nécessaire de modifier la `{1..3}` pour qu’elle corresponde au nombre de nœuds dans votre cluster Adobe Commerce Cloud, mais 3 est le plus courant.

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1 | sed 's/.\.c\.(.)/\1/;s/.$//' | xargs -I% dig +short {1..3}."%" | grep '^\d' | sort -u
```

## Interface de ligne de commande Adobe Commerce Cloud

```bash
magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1
```

L’outil d’interface de ligne de commande Magento-Cloud est conçu pour aider les développeurs et les administrateurs système à gérer efficacement les projets et environnements Adobe Commerce Cloud. Il étend les fonctionnalités de la console cloud, permettant aux utilisateurs d’effectuer des tâches de routine et d’exécuter l’automatisation localement. Les fonctionnalités clés incluent la gestion des environnements d’intégration, l’extraction et la fusion des environnements, la liste des variables et l’utilisation de SSH pour se connecter aux environnements distants. L’outil simplifie les workflows en permettant l’exécution directe des commandes à partir du poste de travail local, ce qui améliore le processus global de développement et de déploiement.

Dans cette première section de l’exemple de code, `magento-cloud environment:url -p InsertYourProjectID -e UseYourEnvironmentName --pipe -1` demande l’URL de l’environnement . La valeur renvoyée ressemble à ce qui suit `http://integration-1ajmyuq-mk7xr7zmslfg.us-4.magentosite.cloud/`. De temps en temps, ça ressemble plus à ce `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`.  Cette première commande est assez simple, et il est maintenant temps de passer à la commande suivante.

Pour plus d’informations, consultez [Présentation de l’interface de ligne de commande Cloud](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-overview){target="_blank"}

## Utilisation de `sed` pour la recherche et le remplacement

```bash
sed 's/.\.c\.(.)/\1/;s/.$//'
```

La commande `sed` sous UNIX®Linux® signifie Éditeur de flux. Il est utilisé pour effectuer des transformations de texte de base sur un flux d’entrée (un fichier ou une entrée à partir d’un pipeline). Les utilisations courantes comprennent la recherche, la recherche et le remplacement, l’insertion et la suppression de texte. La `sed` de commande traite le texte ligne par ligne et applique des opérations spécifiées, ce qui en fait un outil puissant pour la manipulation de texte et les scripts.

Comme mentionné précédemment, il existe généralement 2 types d’URL renvoyés par l’interface de ligne de commande `magento-cloud`. Il existe une variation qui contient des `.com.c.c` au milieu. Cette variante est celle qui doit être manipulée. Si cette structure est détectée, elle nécessite la suppression de tout, du début de l’URL jusqu’à `.com.c.c`.  Il ne reste alors que la dernière partie de l’URL. Un exemple d’URL ressemble à `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`.  Lorsque ce modèle est détecté, l’objectif est de tout conserver après `.c.`.  Dans cet exemple de code fourni, `sed 's/.\.c\.(.)/\1/'` est utilisé pour saisir cette partie et ignorer le reste de la valeur renvoyée d’origine. La partie restante de l’URL ressemble à quelque chose comme `abcikdxbg789.ent.magento.cloud/`.\
Deux commandes sont en cours d’exécution dans `sed`. Ils sont séparés par un point-virgule. La deuxième partie de mon `sed` de commande `;s/.$//'` consiste à supprimer les barres obliques de fin si elles existent, à nettoyer cette URL pour qu’elle ressemble à `abcikdxbg789.ent.magento.cloud`.  À ce stade, l’URL a été nettoyée et prête pour la commande suivante.

## Cartes avec dig

```bash
xargs -I% dig +short {1..3}."%"
```

La commande `xargs` sous UNIX®Linux® est utilisée pour créer et exécuter des lignes de commande à partir d’une entrée standard. Il prend l’entrée d’une barre verticale ou d’un fichier et la convertit en arguments pour une autre commande. Elle est particulièrement utile pour gérer un grand nombre d’arguments qui dépassent la limite du shell. La commande `xargs` peut être utilisée pour effectuer des opérations telles que le déplacement, la copie ou la suppression de fichiers. Il permet un traitement par lots efficace en transmettant plusieurs arguments aux commandes dans une seule exécution.

La commande `dig`, abréviation de Domain Information Groper, est un outil d’administration réseau utilisé pour interroger les serveurs DNS (Domain Name System). Il permet de récupérer des informations sur les enregistrements DNS, tels que les enregistrements A, AAAA, MX et CNAME. La commande `dig` est généralement utilisée pour résoudre les problèmes de DNS, vérifier les configurations DNS et collecter des informations détaillées sur les noms de domaine et leurs adresses IP associées. À l’aide de plusieurs options et indicateurs, les utilisateurs et utilisatrices peuvent personnaliser la sortie pour afficher des détails spécifiques ou un résumé concis.

L’utilisation de `xargs` avec `dig` complique les choses, mais elle est nécessaire. L’objectif est de prendre cette URL nettoyée et de l’enregistrer.  Une fois l’URL enregistrée en tant que variable, elle est insérée dans la commande `dig`.

La commande `dig` a été créée pour collecter des informations DNS. Pour réduire la quantité de données renvoyées, l’argument `+short` est utilisé. En utilisant `dig` combiné avec `+short`, les adresses IP et parfois les chaînes sont renvoyées.

Dans cette partie de la commande, `xargs` enregistrera cette URL `abcikdxbg789.ent.magento.cloud` et l’insérera dans la `dig` de commande suivante. La technique d’enregistrement de l’URL, associée à l’itération, facilite son utilisation avec la commande `dig`. Gardez à l’esprit que mon exemple de code est un moyen d’atteindre l’objectif, n’hésitez pas à modifier les éléments pour répondre à vos besoins et attentes.

À ce stade, l’URL est prête. Voyons ensuite comment il est possible de vérifier chaque serveur du cluster. Pour Adobe Commerce Cloud, chaque serveur du cluster possède un numéro. L’identifiant du serveur est un préfixe de l’URL qui vient d’être nettoyée et rendue prête à l’emploi. Un moyen rapide et facile de vérifier les serveurs consiste à utiliser `{1..3}`. En utilisant `{1..3}` qui notifie l’exécution de la commande `dig` à 3 reprises. Voici une représentation de ce qui se passe si vous regardez l’exécution en temps réel.

```bash
dig +short 1.<projectid>.ent.magento.cloud
dig +short 2.<projectid>.ent.magento.cloud
dig +short 3.<projectid>.ent.magento.cloud
```

À des fins d’illustration, cet exemple d’URL utilisé par `dig` ressemblerait à ce qui suit :

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
```

Si `{1..3}` a été modifié pour être `{1..6}`, il ressemblerait à ceci :

```bash
dig +short 1.aabcikdxbg789.ent.magento.cloud
dig +short 2.abcikdxbg789.ent.magento.cloud
dig +short 3.abcikdxbg789.ent.magento.cloud
dig +short 4.aabcikdxbg789.ent.magento.cloud
dig +short 5.abcikdxbg789.ent.magento.cloud
dig +short 6.abcikdxbg789.ent.magento.cloud
```

## La commande `grep`

Il arrive qu’une chaîne soit renvoyée dans le cadre du résultat de `dig`. Pour ce faire, l’objectif est uniquement les adresses IP. Elles sont composées de nombres avec des points. Pour vous assurer que la sortie finale ne contient que des chiffres, il est possible d’ajuster la commande . Une fois terminée, la syntaxe finale est ` grep '^\d'`.  En ajoutant `'^\d'`, la `grep` de commande ne conserve que les nombres et ignore tout le reste.

## La commande `sort`

En utilisant `sort -u`, il supprime tous les doublons de la liste des adresses IP. Les doublons ne se produisent que lors de la recherche dans les niveaux de développement.

Ces environnements de niveau inférieur sont à clients multiples et partagent des serveurs sous-jacents avec de nombreux autres projets. Les environnements de développement sont des serveurs uniques et ne constituent jamais un cluster. Par conséquent, lorsque la commande dig effectue une boucle sur chaque itération, elle renvoie la même adresse IP plusieurs fois. Ainsi, en utilisant la commande `sort -u`, toutes les adresses IP en double sont supprimées et il ne reste que des adresses IP uniques.



## Documentation connexe

* [ Adresses IP régionales ](https://experienceleague.adobe.com/en/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses){target="_blank"}
