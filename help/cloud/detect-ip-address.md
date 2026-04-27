---
title: Détecter les adresses IP
description: Découvrez comment détecter des adresses IP pour les environnements Adobe Commerce Cloud afin d’améliorer la sécurité et de rationaliser la communication serveur
feature: Cloud, Configuration
topic: Commerce, Development, Integrations
role: Developer
level: Beginner
doc-type: Technical Video
duration: 624
last-substantial-update: 2025-04-07T00:00:00.000Z
jira: KT-17553
exl-id: beb0a6e1-e6b1-4ec0-976c-77a22a27e8a2
TQID: https://experienceleague.adobe.com/evduXZiZpjjhXbDahgpPjmemxRYcMhFJIwfu4GsYI9c
product_v2:
  - id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2:
  - id: ba9e5be9-7de1-4f71-a5d2-baead0e425ee
  - id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 1127
ht-degree: 0%

---

# Détection des adresses IP pour différents environnements

Découvrez comment détecter des adresses IP pour différents environnements dans un projet Adobe Commerce Cloud. En utilisant une série de commandes, notamment Adobe Commerce CLI, sed, xargs, dig, grep et sort -u, les utilisateurs peuvent identifier les adresses IP pour les environnements de développement, d’évaluation et de production.

## À qui s&#39;adresse cette vidéo ?

* Développeurs : cherchent à comprendre comment collecter les adresses IP pour le projet Adobe Commerce Cloud.
* Opérations de développement et équipes de sécurité devant restreindre l’accès aux systèmes tiers ou principaux

## Contenu vidéo {#video-content}

* Découvrez comment découvrir l’adresse IP de n’importe quel environnement dans Adobe Commerce Cloud.

>[!VIDEO](https://video.tv.adobe.com/v/3457493?learn=on)

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

Pour plus d’informations, consultez [Présentation de l’interface de ligne de commande Cloud](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/dev-tools/cloud-cli/cloud-cli-overview){target="_blank"}

## Utilisation de `sed` pour la recherche et le remplacement

```bash
sed 's/.\.c\.(.)/\1/;s/.$//'
```

The command `sed` in UNIX®Linux® stands for Stream Editor. It is used to perform basic text transformations on an input stream (a file or input from a pipeline). Common uses include searching, finding and replacing, inserting, and deleting text. The command `sed` processes text line by line and applies specified operations, making it a powerful tool for text manipulation and scripting.

As mentioned earlier, there are typically 2 types of URLs returned from the `magento-cloud` cli. There is one variation which contains `.com.c.c` in the middle. This variant is the one which needs to be manipulated. If this structure is detected, it requires the removal everything starting from the beginning of the URL through `.com.c.c`.  Then, what is left is only the last part of the URL. An example URL looks like `http://mcprod.russell.dummycachetest.com.c.abcikdxbg789.ent.magento.cloud/`.  When this pattern is detected, the goal is to keep everything after `.c.`.  In this example code provided, `sed 's/.\.c\.(.)/\1/'` is used to grab this part and ignore the rest of the original returned value. The remaining part of the URL resembles something like `abcikdxbg789.ent.magento.cloud/`.\
There are two commands being executed in `sed`. They are separated by a semi-colon. The second part of my `sed` command `;s/.$//'` is to remove any trailing slashes if they exist, to clean up that URL to look like `abcikdxbg789.ent.magento.cloud`.  At this point, the URL has been cleaned up and ready for the next command.

## Xargs with dig

```bash
xargs -I% dig +short {1..3}."%"
```

The`xargs` command in UNIX®Linux® is used to build and execute command lines from standard input. It takes input from a pipe or a file and converts it into arguments for another command. It is particularly useful for handling large numbers of arguments that exceed the shell&#39;s limit. The command `xargs` can be used to perform operations like moving, copying, or deleting files. It allows for efficient batch processing by passing multiple arguments to commands in a single execution.

The `dig` command, short for Domain Information Groper, is a network administration tool used to query DNS (Domain Name System) servers. It helps retrieve information about DNS records, such as A, AAAA, MX, and CNAME records. La commande `dig` est généralement utilisée pour résoudre les problèmes de DNS, vérifier les configurations DNS et collecter des informations détaillées sur les noms de domaine et leurs adresses IP associées. À l’aide de plusieurs options et indicateurs, les utilisateurs et utilisatrices peuvent personnaliser la sortie pour afficher des détails spécifiques ou un résumé concis.

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

* [Adresses IP régionales](https://experienceleague.adobe.com/fr/docs/commerce-on-cloud/user-guide/project/regional-ip-addresses){target="_blank"}
