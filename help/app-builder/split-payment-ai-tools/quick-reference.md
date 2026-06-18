---
title: Split Payment POC — Tutoriel de référence rapide
description: Découvrez le mappage des fichiers POC de paiement partagé. La page Experience League qui correspond à chaque invite d’IA, ordre de section suggéré et notes de création pour l’extraction Luma.
feature: App Builder, Extensibility, Paas, REST, Eventing
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Tutorial
duration: 398
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '1444'
ht-degree: 0%

---

# Split Payment POC : tutoriel de référence rapide

Cette page résume l’organisation de la série de tutoriels de validation de principe sur le paiement fractionné. Il mappe chaque fichier d’invite source numéroté à sa page Experience League publiée, répertorie un ordre de section suggéré pour les lecteurs et collecte des notes pour les auteurs et les responsables.


## Référence fichier par fichier

### [Créer un PDC de paiement partagé : outils App Builder et d’IA](./overview.md)

**Objectif :** introduction et orientation du tutoriel.

**Pourquoi est-ce nécessaire :** donne aux développeurs le « pourquoi » avant le « comment ». Explique ce qu’ils vont créer, qui est l’audience et, de manière critique, ce qu’ils doivent personnaliser si leur site diffère d’une installation Luma épurée. Il sert également de table des matières pour le reste de la série.

**Utilisation du tutoriel :** ouverture de la section. Définit le contexte avant toute étape technique.


### [Paiement fractionné POC : décisions d&#39;architecture et de conception](./architecture-and-decisions.md)


**Objectif :** explication approfondie de chaque décision architecturale dans la preuve de concept.

**Pourquoi est-ce nécessaire :** le point le plus commun de confusion lorsque vous travaillez avec cette preuve de concept est de comprendre *pourquoi* quelque chose se trouve dans PHP par rapport à App Builder. Sans ce contexte, les développeurs tentent de déplacer les éléments qui ne peuvent pas l’être (comme l’application de crédit de magasin) et échouent. Ce document anticipe ces échecs avec un raisonnement clair.

**Sujets clés abordés :**

* La règle « ce qui doit rester en PHP » et pourquoi
* Modèle de double application de seuil
* Pourquoi la demande de crédit de magasin ne peut pas être asynchrone
* Les cinq cas de Commerce Edge gérés par les modules externes
* Le flux de données d’attribut d’extension à partir de l’extraction → citer → ordre → Événement I/O → App Builder

**Utilisation du tutoriel :** section « Architecture » ou « Fonctionnement ». Peut être ignoré par les développeurs et développeuses Commerce expérimentés, mais est essentiel pour les nouveaux venus sur App Builder.


### [Validation de principe de paiement fractionné : conditions préalables et configuration de l’environnement](./prerequisites-and-setup.md)


**Objectif :** remplir la liste de vérification avant vol avant que le code soit écrit ou que des invites soient exécutées.

**Pourquoi elle est nécessaire :** la phase de configuration présente le taux d’échec le plus élevé dans les tutoriels. Ce document garantit que l’administrateur Commerce est correctement configuré (titre COD exact, crédit de magasin activé, solde du client testé créé, intégration créée), que les événements I/O et le fournisseur d’événements Commerce sont connectés au projet App Builder et que l’environnement local dispose des versions appropriées.

**Éléments de soulignement :**

* Le titre de l’option Contre remboursement doit être exactement `Cash` (critique - le JavaScript effectue une correspondance de chaînes).
* L’intégration de Commerce doit être *activée* (et pas seulement enregistrée) pour que les informations d’identification OAuth fonctionnent
* `entity_id` et `increment_id` expliqués ici pour éviter toute confusion

**Utilisation du tutoriel :** section « Conditions préalables » ou « Prise en main ». Doit être complété de manière interactive, pas seulement lu.


### [PDC de paiement fractionné : référence des variables d’environnement](./env-reference.md)


**Objectif :** toutes les variables d’environnement pour les trois composants sont regroupées au même endroit.

**Pourquoi est-ce nécessaire** les quatre mêmes informations d’identification OAuth apparaissent dans trois fichiers différents avec des noms de variable différents (`COMMERCE_*` ou `COMMERCE_INTEGRATION_*`). Le fait d’avoir une seule référence empêche le débogage « pourquoi cela ne fonctionne-t-il pas » lorsqu’un jeu d’informations d’identification comporte une faute de frappe.

**Composants couverts :**

* `split-payment-orchestrator/.env`
* `commerce-checkout-starter-kit/.env` (IMS + OAuth)
* `commerce-backend-ui-1/.env.simulation`

**Utilisation du tutoriel :** barre latérale de référence ou section « Configuration ». Également utilisé comme complément aux invites de build.


### [PDC de paiement partagé : invite d’IA du module Commerce](./commerce-module-prompt.md)


**Objectif :** invite complète et autonome d’IA pour générer l’ensemble du module PHP `Client_SplitPayment`.

**Pourquoi est-ce nécessaire :** il s’agit de l’artefact de build principal de l’IA pour le côté PHP. Il est écrit pour qu&#39;un développeur sans expérience PHP puisse le remettre à Cursor (avec Claude) et obtenir un module fonctionnel. Chaque fichier est spécifié, chaque contrat comportemental est défini, des notes d’implémentation critiques sont incluses et les commandes permettant d’activer le module par la suite sont fournies.

**Couverture :**

* Structure complète des fichiers (plus de 30 fichiers)
* Schéma de base de données, attributs d’extension, points d’entrée REST, configuration des événements I/O
* Toutes les classes PHP avec des spécifications comportementales complètes (pas seulement les signatures)
* Spécifications du composant de passage en caisse KnockoutJS
* Les cinq modules externes de cas Edge avec des explications de leur existence
* Légendes de personnalisation pour les thèmes autres que Luma

**Utilisation du tutoriel** section « Créer le module Commerce ». L’invite elle-même est l’artefact : les développeurs la copient dans leur outil d’IA et l’exécutent.


### [PDC de paiement partagé : invite de l’IA de l’orchestrateur App Builder](./orchestrator-prompt.md)


**Objectif :** invite d’IA complète et autonome permettant de générer l’application App Builder `split-payment-orchestrator`.

**Pourquoi c&#39;est nécessaire :** Les quatre actions d&#39;App Builder (`payment-orchestrator`, `payment-accept`, `payment-decline`, `demo-dashboard`) sont au cœur de l&#39;histoire « ce qui a quitté PHP ». Cette invite les génère toutes avec des spécifications comportementales complètes, une structure de `app.config.yaml` correcte et la configuration d’enregistrement d’événement.

**Couverture :**

* `app.config.yaml` avec les quatre actions et l’enregistrement de l’événement I/O
* Client partagé OAuth 1.0a `commerce-client.js`
* Les quatre actions avec des spécifications logiques complètes
* Le bouchon obsolète `store-credit.js` (avec explication de *pourquoi* il n’est pas utilisé - cela a une importance pédagogique)
* Tableau de bord de démonstration avec rendu HTML, filtrage des commandes et sécurité
* `.env.example` avec toutes les variables

**Utilisation du tutoriel** section « Créer l’application App Builder ». Accompagnement de l’invite du module Commerce.


### [PDC de paiement partagé : invite d’IA de l’extension de l’interface utilisateur Experience Cloud](./experience-cloud-ui-prompt.md)


**Objectif :** invite de l’IA pour générer l’extension SDK facultative de l’interface utilisateur d’administration Experience Cloud.

**Pourquoi est-ce nécessaire :** le tableau de bord de démonstration dans l’invite de l’orchestrateur est volontairement approximatif ; il s’agit d’une preuve de concept, et non d’une production. Cette section présente l’étape suivante aux développeurs : incorporer le tableau de bord du paiement fractionné dans Commerce Admin Shell à l’aide de l’interface utilisateur d’administration SDK. Il était complètement absent des invites d&#39;origine.

**Couverture :**

* `ext.config.yaml` de l’extension SDK de l’interface utilisateur d’administration
* Composants React pour le tableau de bord des commandes et les détails des commandes
* Actions du serveur principal utilisant l’authentification IMS pour l’inscription et OAuth pour accepter/refuser
* Le script de simulation (également utilisé pour les tests)

**Utilisation du tutoriel :** section facultative « Aller plus loin » ou « Chemin de production ». Peut être ignoré si le tutoriel se concentre uniquement sur la preuve de concept.


### [Paiement fractionné POC : guide de test et de vérification](./testing-and-verification.md)


**Objectif :** guide de test détaillé couvrant chaque composant dans l’ordre de vérification approprié.

**Pourquoi est-ce nécessaire :** la création des composants représente la moitié du travail. L’équipe de développement a besoin d’un chemin de vérification clair qui commence au niveau le plus bas (colonnes de base de données, points d’entrée REST) et s’étend au flux de bout en bout complet. Les invites d&#39;origine avaient une liste de contrôle de configuration, mais aucun flux de test explicite.

**Couverture :**

* Vérification de l&#39;installation du module Commerce
* Vérification de la configuration admin.
* Tests de vérification des points d’entrée REST (commandes curl)
* Présentation de l’interface utilisateur de passage en caisse (y compris les cas de validation)
* Test de protection du seuil
* Test de placement de commande complet
* Utilisation du script de simulation pour accepter et refuser
* Utilisation du tableau de bord de démonstration
* Inspection du journal des actions App Builder
* Dix modes d’échec courants avec des correctifs spécifiques

**Utilisation du tutoriel :** section « Test » ou « Vérification ». Également utile comme référence de dépannage.


### [Paiement fractionné POC : prochaines étapes après la preuve de concept](./next-steps.md)


**Objectif :** feuille de route pour faire évoluer la preuve de concept en modèles prêts pour la production.

**Pourquoi est-ce nécessaire :** Un tutoriel de preuve de concept risque de laisser les développeurs avec un « quoi maintenant ? » sentiment. Ce document mappe les progressions naturelles de la démonstration à la production : remplacement du tableau de bord de démonstration par une extension Experience Cloud, connexion d’un véritable ERP, ajout du maillage API, développement du workflow App Builder et liste de contrôle du déploiement en production.

**Contenu clé :**

* Schéma d&#39;évolution de l&#39;architecture (PoC → Phase 2 → Phase 3)
* Modèle d&#39;intégration ERP (ce qui change, ce qui reste le même)
* Modèle de courtier Maillage API
* Liste de contrôle du déploiement en production
* Liens de référence clés

**Utilisation du tutoriel :** section de fermeture « Étapes suivantes ». Également utile comme démarreur de conversation pour la définition de la portée d’un projet réel.


## Sections du tutoriel suggérées

Sur la base de ces fichiers, une structure type pour les lecteurs est la suivante :

| Section du tutoriel | page Experience League |
| --- | --- |
| Introduction et objectifs d’apprentissage | [Créer un PDC de paiement partagé : outils App Builder et d’IA](./overview.md) |
| Démonstration de bout en bout (vidéo) | [Création d’une preuve de concept de paiement partagé : démonstration complète d’App Builder](./full-demo.md) |
| Architecture : ce qui vit où | [Paiement fractionné POC : décisions d&#39;architecture et de conception](./architecture-and-decisions.md) |
| Conditions préalables et configuration | [Validation de principe de paiement fractionné : conditions préalables et configuration de l’environnement](./prerequisites-and-setup.md) |
| Variables d’environnement | [PDC de paiement fractionné : référence des variables d’environnement](./env-reference.md) |
| Étape 1 : création du module Commerce | [PDC de paiement partagé : invite d’IA du module Commerce](./commerce-module-prompt.md) |
| Étape 2 : création de l’orchestrateur App Builder | [PDC de paiement partagé : invite de l’IA de l’orchestrateur App Builder](./orchestrator-prompt.md) |
| Étape 3 : tester le flux de bout en bout | [Paiement fractionné POC : guide de test et de vérification](./testing-and-verification.md) |
| Étape 4 (facultatif) : extension de l’interface utilisateur d’administration | [PDC de paiement partagé : invite d’IA de l’extension de l’interface utilisateur Experience Cloud](./experience-cloud-ui-prompt.md) |
| Étapes suivantes et chemin d’accès en production | [Paiement fractionné POC : prochaines étapes après la preuve de concept](./next-steps.md) |


## Notes importantes à l’intention des auteurs de tutoriels

**Selon l’hypothèse du thème Luma :** chaque invite de génération génère du code pour une installation Luma propre. Le tutoriel doit clairement l’indiquer en haut de l’écran : les développeurs et développeuses qui effectuent des passages en caisse personnalisés doivent adapter les chemins d’injection `LayoutProcessorPlugin` et éventuellement la configuration RequireJS. L’introduction à la série et les invites de build incluent des légendes de personnalisation pour cela.

**Sur le module PHP :** Le tutoriel ne fournit pas explicitement le code PHP comme un téléchargement copier-coller. L’invite d’IA *génère*. C’est intentionnel : cela enseigne le modèle d’utilisation de l’IA pour créer des extensions Commerce, et pas seulement pour copier-coller des éléments standard. Cependant, le code généré lorsque vous y êtes invité dans un environnement propre doit correspondre exactement au code de travail dans `app/code/Client/SplitPayment/`.

**Sur le seuil de 100 $ :** il s’agit d’une valeur PoC codée en dur. Notez dans le tutoriel que les véritables magasins souhaitent configurer ce paramètre via la configuration d’administration de Commerce plutôt qu’une constante de temps de compilation.

**Dépendance crédit magasin :** le flux de paiement fractionné tel que créé nécessite `Magento_CustomerBalance` (module de crédit magasin natif).


{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
