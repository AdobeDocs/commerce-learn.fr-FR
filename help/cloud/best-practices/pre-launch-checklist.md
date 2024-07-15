---
title: Liste de contrôle de prélancement de Adobe Commerce Cloud
description: Découvrez la liste de contrôle de prélancement de Adobe Commerce Cloud.
feature: Cloud
topic: Commerce, Architecture, Development
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-17T00:00:00Z
jira: KT-15180
kt: 15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
source-git-commit: 00a8d6883473de796abc79ef2e9be34f56429a17
workflow-type: tm+mt
source-wordcount: '1605'
ht-degree: 0%

---

# Liste de contrôle de prélancement des Commerces Cloud

Voici une synthèse de la [documentation de lancement de site](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"} d’Adobe Commerce.

Cette liste de contrôle a pour but de vous aider à planifier et exécuter un lancement réussi du site Adobe Commerce Cloud. Collaborez avec votre intégrateur système pour Adobe Commerce Cloud afin de vous assurer que toutes les tâches de configuration et les éléments de liste de contrôle sont terminés et vérifiés. Si vous rencontrez des difficultés avec des éléments de liste de contrôle ou si vous avez des questions, contactez le conseiller technique du client ou l’ingénieur du service client désigné. Si aucun CTAS/ingénieur du service client n’est affecté à votre compte, vous pouvez créer un ticket d’assistance pour obtenir de l’aide.

Si une CTA/ingénieur du service client est affecté au compte, contactez-le et le gestionnaire de compte au moins 4 semaines avant le lancement du nouveau site Adobe Commerce Cloud pour leur signaler votre **intention** de lancer.

- Certaines vérifications sont surlignées avec [!BADGE Blocker]{type=caution tooltip="Blocage potentiel"}
- Veillez à collaborer avec votre développeur ou votre partenaire d’intégration du système pour vous aligner sur votre approche de mise en oeuvre.

>[!IMPORTANT]
> Si vous ne parvenez pas à utiliser et à remplir cette liste de contrôle, vous acceptez la [responsabilité](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"} pour tout effet négatif et les risques associés au calendrier de lancement de la production et à la stabilité continue du site.

## 1. Pré-activation

1. Consultez la documentation sur le test et la mise en ligne [Documentation sur le lancement du site](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >Assurez-vous qu’un &quot;plan de préparation en direct&quot;_complet est entièrement préparé avec votre partenaire ou votre intégrateur système, en incorporant tous les éléments d’action nécessaires._ Souvenez-vous que, même si la liste de contrôle de prélancement met l’accent sur les bonnes pratiques de l’Adobe, elle _**ne remplace pas**_ la nécessité de votre propre plan de préparation à la mise en service.

2. [!BADGE Blocker]{type=caution tooltip="Blocage potentiel"}[Guide de l’utilisateur](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"})
3. L’utilisateur final/le marchand a effectué des tests UAT (User Acceptation Testing), y compris des opérations principales.
4. L’équipe d’intégrateur système a effectué des opérations UAT de bout en bout lors de l’évaluation et de la production. Reportez-vous à la [documentation Experience League](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}.
5. Confirmez le déploiement et le test du code dans les environnements d’évaluation et de production ([En savoir plus](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}).
6. La grappe de production a été redimensionnée de manière permanente par rapport à la ligne de base quotidienne convenue. Pour plus d’informations, adressez-vous au CTA/ingénieur du service client affecté ou soulevez un ticket d’assistance.

## 2. Configurations en cours

1. Mettez à niveau Adobe Commerce et les packages/services associés vers la [dernière version](https://experienceleague.adobe.com/en/docs/commerce-operations/release/notes/overview){target="_blank"}
2. Passez en revue les configurations et services actuels avec votre SI/Partner et [suivez les bonnes pratiques](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}.
3. Examinez MySQL/Shared-Files [usage du disque](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/storage/manage-disk-space){target="_blank"}

## 3. Configurations rapides

1. [!BADGE Blocker]{type=caution tooltip="Blocage potentiel"}[Cache de page complète](https://developer.adobe.com/commerce/frontend-core/guide/caching/){target="_blank"} ou [Mise en cache de GraphQL](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}). Lisez le [guide de configuration rapide](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/cdn/fastly){target="_blank"}.
2. Utilisez la méthode de GET pour les requêtes GraphQL sur les sites web PWA/sans affichage, le cas échéant.

   >[!NOTE]
   > Seules les requêtes envoyées avec une opération de GET HTTP peuvent être mises en cache (le cas échéant). [Les requêtes de POST ne peuvent pas être mises en cache](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}.

3. Assurez-vous que l’optimisation rapide des images est activée ([Voir Optimisation rapide des images](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/cdn/fastly-image-optimization){target="_blank"})
4. Vérifiez que l’emplacement de protection correct est configuré ([Configurez le cache, les serveurs principaux et l’protection d’origine ](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}).
5. Le pare-feu d’applications web (**WAF**) fonctionne. (Voir [Dépannage des requêtes bloquées](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/cdn/fastly-waf-service){target="_blank"}, le cas échéant, et limites)
6. Mettez à jour la liste Fastly [&quot;Paramètres d’URL ignorés&quot;](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"} dans le panneau d’administration pour améliorer les performances du cache.

   >[!NOTE]
   > Dans la configuration Fastly sous _Admin > Magasins > Configurations > Système > Cache de page complète > Configuration rapide > Configuration avancée > Paramètres d’URL ignorés (Global)_, vous pouvez trouver une liste de paramètres séparés par des virgules qui doivent Fastly être ignorés lors de la recherche de pages mises en cache. Veillez à télécharger à nouveau le VCL après avoir modifié cette liste.

## 4. DNS et SSL

1. [!BADGE Blocker]{type=caution tooltip="Blocage potentiel"}_(Envoyez un ticket d’assistance à l’avance pour tout domaine ajouté ou modifié)_
2. [!BADGE Blocker]{type=caution tooltip="Blocage potentiel"}[cet article](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"} pour plus d’informations.
3. Mettez à jour la valeur DNS [TTL (Time to Live)](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/checklist#to-update-dns-configuration-for-site-launch){target="_blank"} au minimum possible pour la mise en ligne.
4. Activer Sendgrid SPF et DKIM

   >[!NOTE]
   > Ajoutez les enregistrements CNAME SendGrid pour chaque domaine à la configuration DNS. Lisez le [service de messagerie SendGrid](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"} pour voir comment modifier les domaines d’expéditeur et plus encore.

## 5. Configurations de la base de données

Adobe Commerce Cloud utilise une grappe MariaDB Galera comme base de données pour les environnements d’évaluation et de production. Les grappes Galera sont essentielles pour améliorer les performances et l’évolutivité. Pour obtenir des informations sur les pratiques optimales et les contraintes des réplications de cluster Galera, reportez-vous aux articles suivants.

- [Bonnes pratiques relatives aux configurations MySQL](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
- Alertes gérées sur Adobe Commerce : [Alertes MariaDB](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
- Bonnes pratiques relatives à la [configuration de base de données](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"}
- Analyse approfondie de [réplications de cluster Galera et contrôle de flux.](https://experienceleague.adobe.com/en/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication){target="_blank"}

1. [La connexion esclave MYSQL](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"} est recommandée pour améliorer les performances lors de charges de base de données élevées.
2. Assurez-vous que le format de ligne de toutes les tables de base de données est défini sur [DYNAMIC au lieu de COMPACT](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"} (cela est particulièrement vrai pour les migrations sur site vers le cloud).
3. Remplacez le moteur de stockage de base de données [MyISAM par InnoDB](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"} pour toutes les tables.
4. Vérifiez et optimisez les tables de base de données dont la taille dépasse 1 Go bien à l’avance.
5. Les informations du schéma de la base de données sont à jour et à jour. (Reportez-vous à [ce guide](https://mariadb.com/kb/en/engine-independent-table-statistics/#collecting-statistics-with-the-analyze-table-statement){target="_blank"}).

## 6. Déploiements

1. Examinez l’état idéal du déploiement de contenu statique (SCD) pour réduire le temps de maintenance pendant les déploiements dans l’environnement de production. Examinez les [stratégies de déploiement de contenu statique (SCD)](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/deploy/static-content){target="_blank"} et le guide [Gestion de la configuration du magasin](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure-store/store-settings){target="_blank"}.
2. Vérifiez les paramètres de minimisation pour HTML, JavaScript et CSS. (Cela ne s’applique pas aux sites web sans PWA/sans affichage).
3. Vérifiez que l’utilisation des variables cloud suivantes s’aligne sur leurs objectifs prévus. ([SCD_MATRIX](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}, [SCD_ON_DEMAND](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"} et [SKIP_SCD](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"})

## 7. Test et dépannage

1. Testez les emails transactionnels sortants. En savoir plus sur la [fonctionnalité Adobe Commerce Cloud - SendGrid Mail](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"}.
2. [!BADGE Blocker]{type=caution tooltip="Blocage potentiel"}
3. [!BADGE Blocker]{type=caution tooltip="Blocage potentiel"}

   >[!NOTE]
   > Un [test de charge et de stress a pour but ](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"} d&#39;identifier les goulets d&#39;étranglement et de découvrir les problèmes de performances dans l&#39;application. Il joue un rôle essentiel dans la gestion des attentes concernant la taille des grappes et dans la détermination des ajustements de mise à l’échelle nécessaires pour répondre efficacement aux besoins de l’entreprise.

   >[!IMPORTANT]
   > **_AVERTISSEMENT :_** Lors de la préparation d’un test de charge, veuillez_ **_ne pas envoyer_** d’emails de transaction en direct (même aux adresses factices). L’envoi d’emails pendant le test peut entraîner le projet à atteindre la limite d’envoi par défaut (12k) configurée pour SendGrid avant son lancement.
   > 
   > - Comment désactiver la communication par e-mail :
   >   Accédez à _Magasin > Configuration > Avancé > Système > Paramètres d’envoi de courriers électroniques_.

4. Effectuez des tests de pénétration de la sécurité sur l&#39;instance de production dans le cadre du [modèle de sécurité de responsabilité partagée](https://business.adobe.com/products/magento/secure-ecommerce.html){target="_blank"}. Pour la conformité PCI (secteur des cartes de paiement), le site personnalisé nécessite des tests de pénétration.

## 8. Autres configurations

1. Passez l’indexation à _&quot;update on schedule_&quot;, à l’exception de **_customer_grid_** qui reste sur &quot;SAVE&quot; (voir [Modes d’indexation](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"}).
2. Utilisez-vous des moteurs de recherche ou des extensions tiers ?
3. Confirmez que les configurations [SEO (Search Engine Optimization, optimisation du moteur de recherche) sont correctement configurées](https://experienceleague.adobe.com/en/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"} pour permettre aux indexeurs/moteurs de recherche d’analyser le site web, le cas échéant.
4. Ajouter des redirections et des itinéraires (voir [Configuration des itinéraires](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/routes/routes-yaml){target="_blank"})

   >[!NOTE]
   >Ajoutez des redirections et des itinéraires au fichier routes.yaml dans l’environnement d’intégration et vérifiez la configuration dans cet environnement avant de procéder au déploiement vers l’évaluation et la production.

       &quot;http://{all}/&quot;:
       type : amont
       en amont : &quot;mymagento:http&quot;
       
       &quot;http://{all}/&quot;:
       type : amont
       en amont : &quot;mymagento:http&quot;
   
5. Assurez-vous que XDebug est désactivé s’il est activé pendant le développement (voir [Configuration de Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/){target="_blank"}).
6. Vérifiez que l’op-cache et d’autres configurations ont été mis à jour avec précision dans le fichier php.ini ([voir cet exemple](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}).
7. Abonnez-vous à la [**page d’état Adobe Commerce**](https://status.adobe.com/cloud/experience_cloud#/){target="_blank"}.
8. Abonnez-vous aux canaux de notification New Relic &quot;[Alertes gérées pour Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce){target="_blank"}&quot; pour surveiller les mesures de performances données ([en savoir plus](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service){target="_blank"}).

## 9. Sécurité

1. Configuration de l’analyse de sécurité Adobe Commerce

   >[!NOTE]
   > [Adobe Commerce Security Scan est un outil utile](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-scan){target="_blank"} qui permet de découvrir des versions de logiciels obsolètes, une configuration incorrecte et des logiciels malveillants potentiels sur le site. Inscrivez-vous, planifiez son exécution fréquente et assurez-vous que les emails sont envoyés au bon contact technique de sécurité.
   > 
   > Effectuez cette tâche lors de l’UAT. Si vous utilisez l’option d’analyse périodique, veillez à planifier les analyses aux moments de faible demande. Consultez la page [Analyse de sécurité](https://account.magento.com/scanner/index/dashboard/){target="_blank"} dans le compte Adobe Commerce. Vous devez vous connecter à un compte Adobe Commerce pour accéder à l’analyse de sécurité.

2. Modifiez les paramètres par défaut de l’administrateur Adobe Commerce.
3. Modifiez le mot de passe administrateur (voir [Configuration de la sécurité d’administration](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
4. Modifiez l’URL d’administration (voir [Utilisation d’une URL d’administration personnalisée](https://experienceleague.adobe.com/en/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"}).
5. Supprimez les utilisateurs qui ne se trouvent plus sur le projet (voir [Création et gestion des utilisateurs](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/project/user-access){target="_blank"}).
6. Les mots de passe des administrateurs sont configurés (voir [Exigences en matière de mot de passe administrateur](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
7. Configurez l’authentification à deux facteurs (voir [Authentification à deux facteurs](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/){target="_blank"}).

## 10. Go Live

Lorsque l’heure est venue de se couper, procédez comme suit (pour plus d’informations, voir [Configurations DNS](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}) :

1. Accédez à votre service DNS et mettez à jour les enregistrements A et CNAME pour chacun de vos domaines et noms d’hôtes :
   1. Ajoutez un enregistrement CNAME pour _&quot;www.yourdomain.com>_, en pointant vers **prod.magentocloud.map.fastly.net**
   2. Définissez quatre enregistrements A pour _&quot;yourdomain.com>_, en pointant vers :\
      151.101.1.124\
      151.101.65.124\
      151.101.129.124\
      151.101.193.124
2. Remplacez l’URL de base Adobe Commerce par _&quot;www.yourdomain.com>_
3. Patientez jusqu’à l’expiration du délai d’activation, puis redémarrez le navigateur web.
4. Testez le site web.

### Si un problème bloque la mise en ligne :

Si vous rencontrez des problèmes ou si vous ne parvenez pas à démarrer pendant la coupure, la méthode la plus rapide pour obtenir une assistance opportune est d’utiliser le service d’assistance et d’ouvrir un ticket avec la raison &quot;Impossible de lancer mon magasin&quot;, et d’appeler un numéro d’assistance téléphonique (voir [la liste des numéros de la hotline Adobe Commerce P1 (Priorité 1)](https://support.magento.com/hc/en-us/articles/360042536151){target="_blank"}) :

- Numéro d’appel gratuit aux États-Unis : (+1) 877 282 7436 (direct vers la hotline Adobe Commerce P1)
- Numéro d’appel gratuit aux États-Unis : (+1) 800 685 3620 (dans le premier menu, appuyez sur la touche 7 pour la hotline Adobe Commerce P1)
- Local américain : (+1) 408 537 8777

## 11. Activation de Post

Une fois le site actif, envoyez par courrier électronique le CTA (Customer Technical Advisory), l’ingénieur du service client (Customer Success Engineer) et le gestionnaire de compte (AEM) qui vous ont été attribués. Cependant, si aucun gestionnaire de compte n’est affecté au projet, vous pouvez créer un ticket d’assistance demandant l’activation de la surveillance de niveau élevé de contrat de niveau de service une fois le site mis en ligne. L’ACT/ingénieur du service client effectue les tâches suivantes dès que le site est vérifié pour être lancé avec l’option Fastly activée et mise en cache :

- Marquez la grappe comme étant active et créez un ticket d’assistance pour activer la surveillance des accords de niveau de service (High SLA).
- Activez New Relic Synthetics pour la surveillance de la disponibilité.

>[!MORELIKETHIS]
> 
> - [Présentation de Launch Readiness - manuel d’implémentation](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/launch/overview){target="_blank"}
> - [Liste de contrôle de Launch - Guide de l’utilisateur](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}
> - [Liste de contrôle de prélancement - Guide de l’administrateur Site Manager/Commerce](https://experienceleague.adobe.com/en/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> - [Modèle de sécurité de la responsabilité partagée](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}
