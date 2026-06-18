---
title: PdC de paiement partagé — Démonstration complète d’App Builder
description: Découvrez comment fonctionnent le paiement partagé, le REST, App Builder I/O et l’acceptation/le refus de l’opérateur dans cette démonstration de Luma, ainsi qu’un total de précommande configurable pouvant bloquer le panier.
feature: App Builder, Paas, Payments
topic: App Builder, Commerce, Development, I/O Events, Integrations, Runtime
role: Developer, User
level: Intermediate
doc-type: Technical Video
duration: 861
jira: KT-20902
last-substantial-update: 2026-04-27T00:00:00Z
source-git-commit: 63ac13d8c5a97ee81dcdd1f3785a9875aaf2a4db
workflow-type: tm+mt
source-wordcount: '1029'
ht-degree: 0%

---

# Création d’un PDC de paiement partagé : démonstration complète d’App Builder

Il s’agit de la présentation complète de la preuve de concept du paiement partagé basée sur Adobe Commerce et Adobe App Builder. La démonstration suppose que vous avez déjà utilisé les outils d’IA et une invite pour produire l’extension Commerce en cours de traitement et l’application App Builder. Cette vidéo montre ce qui se passe après la fusion de ce code, son déploiement sur un site web Adobe Commerce Cloud à l’aide du thème Luma natif et la mise en ligne du projet App Builder.

Un acheteur paie en partie en espèces et en partie en **[!UICONTROL Store Credit]**. Commerce possède des extractions synchrones et les API dont le storefront a besoin ; App Builder gère l’orchestration, les workflows d’opérateur et les consommateurs d’événements d’E/S. L’implémentation de référence utilise un projet Commerce (PaaS) et le passage en caisse natif de Luma plutôt qu’un storefront Edge Delivery Services, qui reste un chemin d’accès courant pour de nombreux commerçants. Si vous utilisez **Adobe Commerce as a Cloud Service** dans une topologie différente, le code App Builder reste similaire, mais le storefront et le travail en cours semblent différents. Pour le code local, auto-hébergé et Commerce en mode cloud sur Luma, cette vidéo présente une répartition pratique entre le code en cours de traitement et App Builder pour de nouvelles fonctionnalités.

## Vidéo

>[!VIDEO](https://video.tv.adobe.com/v/3484087?learn=on)

## À qui s&#39;adresse cette vidéo ?

* Architectes techniques planifiant l’extensibilité de Commerce
* Développeurs et développeuses mettant en œuvre des procédures de passage en caisse et des intégrations
* Équipes d’implémentation qui ont besoin d’une répartition réaliste entre PHP et App Builder

## Configurer le scénario sur le storefront

Le compte de démonstration a **[!UICONTROL Store Credit]** et vous voyez ce solde lors du passage en caisse. Ajoutez des produits jusqu’à ce que le total de la commande soit d’environ 80 $. Cette taille vous donne de l’espace pour afficher à la fois le chemin d’acceptation réussi et un chemin de refus, similaire à un déclin de carte, sans que les mathématiques ne vous combattent.

**[!UICONTROL Split payment]** n’est proposé qu’aux clients connectés, car la session doit exposer le crédit de la boutique. Le passage en caisse des invités n’affiche pas l’option de partage.

1. À l’étape **[!UICONTROL Payment]**, choisissez la méthode Cash (la démonstration renomme **[!UICONTROL Cash on delivery]** en **Juste de l’argent**).
1. Une zone de paiement fractionné s&#39;affiche sous le mode de paiement. Le champ Cash est prérempli sur le total de la commande et le composant lit les **[!UICONTROL Store Credit]** de la session.
1. Pour un panier d’environ 80 $, définissez **50** $ en espèces et **30** $ en crédit de magasin afin que le composant affiche 50 $ + 30 $ = 80 $.
1. Lorsque les montants sont valides, passez la commande.

L’interface utilisateur déclenche un appel à une nouvelle API REST **[!UICONTROL Commerce]** pour la division. Le passage en caisse reste synchrone : Commerce applique le crédit de magasin, stocke les lignes de fractionnement sur la commande et émet un événement d’E/S qu’App Builder consomme ultérieurement. La page de succès est immédiate pour le client.

## Vérifier la commande dans l’administrateur Commerce

Dans **[!UICONTROL Sales]** > **[!UICONTROL Orders]**, ouvrez la nouvelle commande. La zone **[!UICONTROL Payment information]** affiche le partage, par exemple **50 $** en espèces et **30 $** en crédit de magasin. Le statut est **En attente** lorsque l’argent n’est pas encore collecté ; le crédit de la boutique est déjà utilisé lors de la création de la commande.

**[!UICONTROL Comments]** pouvez inclure du texte ajouté par App Builder. Le **[!UICONTROL Payment orchestrator action]** dans App Builder a reçu l’événement d’E/S, vérifié les montants de partage et utilisé un REST authentifié pour ajouter des commentaires. **[!UICONTROL Commerce]** traite cela comme tout autre appel REST et n’a pas besoin de connaître le rédacteur.

## Utiliser le tableau de bord de l&#39;opérateur App Builder (ERP ou back office)

Une expérience HTML simple s’exécute sous la forme d’une action web **[!UICONTROL App Builder]** (aucune **[!UICONTROL Admin]** pour cet affichage). Le tableau de bord appelle l’API REST **[!UICONTROL Commerce Order]** et filtre sur **En attente** afin que vous puissiez trouver la composante en espèces de 50 $.

Vous intégrez généralement ce modèle à un outil ERP ou de fraude. Deux actions sont démontrées :

* **Accepter** — Confirme que l&#39;argent a été reçu (en production, souvent un courrier automatisé provenant d&#39;un tiroir-caisse ou d&#39;un système de magasin).
* **Refuser** — Simule une fraude ou une étape de recouvrement ayant échoué (en production, généralement automatisée à partir de votre service des risques).

**Accepter et terminer la commande**

1. Dans l’application **[!UICONTROL App Builder]**, utilisez **Accepter** pour confirmer l’argent comptant.
1. L’application appelle un nouveau point d’entrée **[!UICONTROL Commerce]** qui finalise la ligne de trésorerie. Le flux de commande de base (facturation et, dans cette version, expédition automatisée) s’exécute avec le comportement natif de **[!UICONTROL Commerce]**. Aucun de ces chemins ne nécessite la présence d’un humain dans **[!UICONTROL Admin]** ; l’orchestration et le point d’entrée résident dans App Builder.

Actualisez la même commande dans **[!UICONTROL Admin]** : le statut passe à **Terminé** lorsque l’argent est accepté, avec la facture et, lorsque le produit est livrable, une expédition pour raccourcir le cycle de vie complet dans la démonstration.

**Refuser et annuler**

1. Ouvrez une commande préconfigurée qui se trouve toujours dans le scénario de déclin.
1. Dans l’application **[!UICONTROL App Builder]**, utilisez **Refuser** pour simuler une fraude ou un échec de collecte.
1. En **[!UICONTROL Admin]**, la commande est **Annulée**. L&#39;historique indique un statut d&#39;encaissement refusé, les commentaires expliquent le résultat, l&#39;acheteur n&#39;a pas été facturé la ligne de crédit du magasin comme s&#39;il s&#39;agissait d&#39;une vente finale, et la facture de crédit du magasin est annulée avec l&#39;annulation. Un système de production avertirait également le client et libérerait tout blocage de stock ou de service.

## Seuil total des commandes (validation avant création de la commande)

La configuration d’App Builder ou d’**[!UICONTROL Commerce]** peut prendre en charge les règles de validation ; cette version bloque les commandes supérieures à 100 **** de la valeur du panier (limite de démonstration que vous pouvez modifier). Une vérification de valeur n’est pas la règle métier la plus courante (les vérifications d’inventaire ou de disponibilité sont plus courantes), mais elle indique que la validation peut s’exécuter au **[!UICONTROL Payment]** dans **[!UICONTROL Commerce]** avant la création d’une commande, tandis qu’App Builder gère toujours l’automatisation du suivi.

1. Ajoutez des produits jusqu’à ce que le total soit supérieur **100** de dollars.
1. Le **[!UICONTROL UI]** de fractionnement apparaît toujours ; le résultat de dépassement de limite n&#39;apparaît que sur **Ordre de placement**.
1. L’acheteur voit une erreur générique telle que **Le paiement n’a pas pu être traité. Réessayez ou contactez l’assistance.**
1. Le **[!UICONTROL cart]** reste inchangé afin que le client puisse réduire le total, choisir une autre méthode ou contacter l’assistance.

Une note de risque, une règle de région ou autre règle similaire peut s’appliquer ici ; **[!UICONTROL Commerce]** bloque la commande et **[!UICONTROL App Builder]** pouvez posséder des notifications et un travail sur les dossiers après coup.

## Commerce on-premise, Commerce en mode cloud et **Adobe Commerce as a Cloud Service**

La présentation correspond **[!UICONTROL Adobe Commerce]** une infrastructure que vous gérez ou **[!UICONTROL Commerce in the cloud]** (PaaS) avec une vitrine traditionnelle. Pour les **[!UICONTROL Adobe Commerce as a Cloud service]** (SaaS) avec un front-end différent et aucun module en cours de traitement de cette forme, les éléments d’App Builder sont largement les mêmes, tandis que le storefront et le travail d’extension seraient différents. Dans tous les cas, le même principe s’applique : **[!UICONTROL Commerce]** ce qui doit se produire dans la requête **[!UICONTROL Commerce]** et utilisez **[!UICONTROL App Builder]** pour le reste de l’expérience.

{{$include /help/_includes/split-payment-ai-tools-related-links.md}}
