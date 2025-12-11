---
title: Liste de contrôle de prélancement d’Adobe Commerce Cloud
description: Découvrez la liste de contrôle de prélancement d’Adobe Commerce Cloud.
feature: Cloud
topic: Commerce, Architecture, Development
old-role: Architect, Developer
role: Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-04-17T00:00:00Z
jira: KT-15180
kt: 15180
exl-id: c6adb2c2-f194-4a3d-9290-e0837ef062ae
source-git-commit: afe0ac1781bcfc55ba0e631f492092fd1bf603fc
workflow-type: tm+mt
source-wordcount: '1668'
ht-degree: 0%

---

# Liste de contrôle de prélancement de Commerce Cloud

Vous trouverez ci-dessous un résumé de la documentation d’Adobe Commerce [lancement du site](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}.

Cette liste de contrôle a pour but de vous aider à planifier et à exécuter un lancement réussi du site Adobe Commerce Cloud. Collaborez avec votre intégrateur système pour Adobe Commerce Cloud afin de vous assurer que toutes les tâches de configuration et tous les éléments de la liste de contrôle sont terminés et vérifiés. Si vous rencontrez des problèmes avec des éléments de la liste de contrôle ou si vous avez des questions, contactez le conseiller technique client ou l’ingénieur du succès client désigné. Si aucun CTA/CSE n’est affecté à votre compte, vous pouvez créer un ticket d’assistance pour obtenir de l’aide.

Si un CTA/CSE est affecté(e) au compte, contactez-le ainsi que le gestionnaire de compte au moins 4 semaines avant le lancement du nouveau site Adobe Commerce Cloud afin de l’informer de votre **intention** de lancement.

- Certaines vérifications sont mises en surbrillance avec [!BADGE Bloqueur]{type=caution tooltip="Bloqueur potentiel"}, car elles peuvent potentiellement bloquer votre activation si elles ne sont pas soigneusement examinées.
- Veillez à collaborer avec votre développeur ou partenaire d’intégration système pour vous aligner sur votre approche de mise en œuvre.

>[!IMPORTANT]
> Vous acceptez la [responsabilité](https://experienceleague.adobe.com/fr/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"} de tout effet indésirable et des risques associés au planning de lancement de la production et à la stabilité continue du site, si vous n’utilisez pas et ne remplissez pas cette liste de contrôle.

## &#x200B;1. Avant la mise en production

1. Consultez la documentation sur le test et la mise en ligne [documentation du lancement du site](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/launch/overview){target="_blank"}

   >[!NOTE]
   >Assurez-vous qu’un _complet de « plan de préparation en ligne »_ est entièrement préparé avec votre partenaire ou votre intégrateur système et qu’il intègre toutes les mesures nécessaires. N’oubliez pas que même si la liste de contrôle avant lancement met l’accent sur les bonnes pratiques d’Adobe, elle _&#x200B;**ne remplace pas**&#x200B;_ le besoin de votre propre plan de préparation à la mise en production.

2. [!BADGE Bloqueur]{type=caution tooltip="Bloqueur potentiel"} consulter les recommandations et informations relatives aux informations d’assistance (SWAT) ([Guide de l’utilisateur](https://experienceleague.adobe.com/fr/docs/commerce-operations/tools/site-wide-analysis-tool/intro){target="_blank"})
3. L’utilisateur final/le commerçant a réalisé l’UAT (test d’acceptation utilisateur), y compris les opérations principales.
4. L’équipe de l’intégrateur système a effectué un test d’expérience utilisateur de bout en bout sur l’évaluation et la production. Consultez la [documentation Experience League](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}.
5. Confirmez le déploiement et le test du code dans les environnements d’évaluation et de production ([En savoir plus](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/develop/test/staging-and-production){target="_blank"}).
6. Le cluster de production a été augmenté de manière permanente par rapport à la base de référence quotidienne convenue. Contactez le CTA/CSE affecté pour plus d’informations ou ouvrez un ticket d’assistance.

## &#x200B;2. Configurations actuelles

1. Mettre à niveau Adobe Commerce et les packages/services associés vers la [dernière version](https://experienceleague.adobe.com/fr/docs/commerce-operations/release/notes/overview){target="_blank"}
2. Passez en revue les configurations et services actuels avec votre SI/partenaire et [suivez les bonnes pratiques](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/best-practices/planning/catalog-management){target="_blank"}.
3. Examinez MySQL/Shared-Files [utilisation du disque](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/develop/storage/manage-disk-space){target="_blank"}

## &#x200B;3. Configurations Fastly

1. [!BADGE Bloqueur]{type=caution tooltip="Bloqueur potentiel"} Assurez-vous que la mise en cache fonctionne ([Cache de page complète](https://developer.adobe.com/commerce/frontend-core/guide/caching/){target="_blank"} ou [Cache de GraphQL](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}). Lisez le [&#x200B; Guide de configuration rapide &#x200B;](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/cdn/fastly){target="_blank"}.
2. Utilisez la méthode GET pour les requêtes GraphQL sur les sites web PWA/Headless, le cas échéant.

   >[!NOTE]
   > Seules les requêtes envoyées avec une opération HTTP GET peuvent être mises en cache (le cas échéant). [Impossible de mettre en cache les requêtes POST](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/){target="_blank"}.

3. Assurez-vous que l’optimisation des images Fastly est activée ([Voir Optimisation des images Fastly](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/cdn/fastly-image-optimization){target="_blank"})
4. Vérifiez que l&#39;emplacement correct du blindage est configuré ([Configurer le cache, les serveurs principaux et le blindage d&#39;origine](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/cdn/setup-fastly/fastly-custom-cache-configuration){target="_blank"}).
5. Le pare-feu d&#39;application web (**WAF**) fonctionne. (Voir [&#x200B; Dépannage des requêtes bloquées](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/cdn/fastly-waf-service){target="_blank"} le cas échéant, et des limitations).
6. Mettez à jour la liste [&#x200B; rapidement « Paramètres d’URL ignorés »](https://github.com/iancassidyweb/magento2/commit/68fdecfcd26c957382b8d68b64887e0a83298524){target="_blank"} dans le panneau d’administration pour améliorer les performances du cache.

   >[!NOTE]
   > Dans la configuration Fastly sous _Admin > Magasins > Configurations > Système > Cache de page complet > Configuration Fastly > Configuration avancée > Paramètres d’URL ignorés (globaux)_, vous pouvez trouver une liste de paramètres séparés par des virgules que Fastly doit ignorer lors de la recherche de pages mises en cache. Veillez à charger à nouveau le VCL après avoir modifié cette liste

## &#x200B;4. DNS et SSL

1. [!BADGE Bloqueur]{type=caution tooltip="Bloqueur potentiel"} Vérifiez que tous les noms de domaine requis sont demandés. _(soumettez un ticket d’assistance à l’avance pour tout domaine ajouté ou modifié)_
2. Le certificat [!BADGE Blocker]{type=caution tooltip="Bloqueur potentiel"} SSL (TLS) a été appliqué au(x) domaine(s). Lisez [cet article](https://experienceleague.adobe.com/fr/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq){target="_blank"} pour plus d’informations.
3. Mettez à jour la valeur DNS [TTL (Time to Live)](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/launch/checklist#to-update-dns-configuration-for-site-launch){target="_blank"} au minimum possible, pour la mise en production.
4. Activer Sendgrid SPF et DKIM

   >[!NOTE]
   > Ajoutez les enregistrements CNAME SendGrid pour chaque domaine à la configuration DNS. Lisez [Service de messagerie SendGrid](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"} pour savoir comment modifier les domaines d&#39;expéditeur, etc.

## &#x200B;5. Configurations de la base de données

Adobe Commerce Cloud utilise un cluster MariaDB Galera comme base de données pour les environnements d’évaluation et de production. Les clusters Galera sont essentiels pour améliorer les performances et l’évolutivité. Pour obtenir des informations sur les pratiques et contraintes optimales des réplications en cluster Galera, reportez-vous aux articles suivants.

- [&#x200B; Bonnes pratiques relatives aux configurations MySQL &#x200B;](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration){target="_blank"}
- Alertes gérées sur Adobe Commerce : [alertes MariaDB](https://experienceleague.adobe.com/fr/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-mariadb-alerts){target="_blank"}
- Bonnes pratiques relatives à [configuration de la base de données](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud){target="_blank"}
- Analyse approfondie des [réplications des clusters Galera et contrôle de flux.](https://experienceleague.adobe.com/fr/docs/commerce-learn/tutorials/backend-development/galera-db-slow-replication){target="_blank"}

1. [Connexion esclave MYSQL](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration#slave-connections){target="_blank"} est recommandée pour améliorer les performances lors de chargements de bases de données élevés.
2. Assurez-vous que le format de ligne pour toutes les tables de la base de données est défini sur [DYNAMIQUE au lieu de COMPACT](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/best-practices/maintenance/mariadb-upgrade#convert-database-table-storage-format){target="_blank"} (cela est particulièrement vrai pour les migrations on-prem vers le cloud).
3. Remplacez le moteur de stockage de la base de données [MyISAM par InnoDB](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/best-practices/planning/database-on-cloud#convert-all-myisam-tables-to-innodb){target="_blank"} pour toutes les tables.
4. Examinez et optimisez les tables de base de données de plus de 1 Go à l&#39;avance.
5. Les informations sur le schéma de la base de données sont à jour. (Voir [ce guide](https://mariadb.com/kb/en/engine-independent-table-statistics/#collecting-statistics-with-the-analyze-table-statement){target="_blank"}).

## &#x200B;6. Déploiements

1. Examinez l’état idéal du déploiement de contenu statique (SCD) pour réduire le temps de maintenance pendant les déploiements sur l’environnement de production. Consultez les guides [Stratégies de déploiement de contenu statique (SCD)](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/develop/deploy/static-content){target="_blank"} et [Gestion de la configuration du magasin](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/configure-store/store-settings){target="_blank"} .
2. Consultez les paramètres de minimisation pour HTML, JavaScript et CSS. (Cela ne s’applique pas aux sites web PWA/Headless).
3. Vérifiez que l’utilisation des variables cloud suivantes s’aligne sur leurs objectifs prévus. ([SCD_MATRIX](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-build#scd_matrix){target="_blank"}, [SCD_ON_DEMAND](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-global#scd_on_demand){target="_blank"} et [SKIP_SCD](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy#skip_scd){target="_blank"})

## &#x200B;7. Tests et dépannage

1. Tester les e-mails transactionnels sortants En savoir plus sur [la fonctionnalité Adobe Commerce Cloud - SendGrid Mail](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/project/sendgrid){target="_blank"}.
2. [!BADGE Bloqueur]{type=caution tooltip="Bloqueur potentiel"} Des bloqueurs avec Adobe ?
3. [!BADGE Bloqueur]{type=caution tooltip="Bloqueur potentiel"} Effectuez des tests de charge et de contrainte sur l’instance de production avant la mise en production et partagez les résultats avec le CTA/CSE affecté.

   >[!NOTE]
   > Un test de charge et de contrainte [load and stress test](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/develop/test/guidance#:~:text=A%20load%20test%20can%20help,Scan%20Tool%20for%20your%20sites.){target="_blank"} a pour but d’identifier les goulets d’étranglement et de découvrir les problèmes de performances de l’application. Il joue un rôle essentiel dans la gestion des attentes concernant la taille des clusters et dans la détermination des ajustements d’échelle nécessaires pour répondre efficacement aux besoins de l’entreprise.

   >[!IMPORTANT]
   > **_WARNING:_** Lors de la préparation d’un test de charge, veuillez_ **_ne pas_** envoyer d’e-mails de transactions en direct (même à des adresses factices). L’envoi d’e-mails pendant le test peut entraîner l’atteinte de la limite d’envoi par défaut du projet (12 000) configurée pour SendGrid avant le lancement.
   > 
   > - Comment désactiver la communication par e-mail :
   >   Accédez à _Store > Configuration > Avancé > Système > Paramètres d’envoi des e-mails_.

4. Effectuez des tests de pénétration de la sécurité sur l’instance de production dans le cadre du [modèle de sécurité à responsabilité partagée](https://business.adobe.com/fr/products/magento/secure-ecommerce.html){target="_blank"}. Pour la conformité PCI (Payment Card Industry), le site personnalisé nécessite des tests de pénétration.

## &#x200B;8. Autres configurations

1. Basculez l’indexation sur _« mise à jour selon le calendrier_ », à l’exception de **_customer_grid_** qui reste sur « ENREGISTRER » (voir [Modes d’indexation](https://developer.adobe.com/commerce/php/development/components/indexing/#indexing-modes){target="_blank"}).
2. Utilisez-vous des moteurs de recherche ou des extensions tiers ?
3. Vérifiez que les configurations [SEO (Search Engine Optimization ou optimisation du moteur de recherche) sont correctement configurées](https://experienceleague.adobe.com/fr/docs/commerce-admin/marketing/seo/seo-overview){target="_blank"} pour permettre aux indexeurs/robots d’analyser le site web, le cas échéant.
4. Ajouter des redirections et des itinéraires (voir [Configuration des itinéraires](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/configure/routes/routes-yaml){target="_blank"})

   >[!NOTE]
   >Ajoutez des redirections et des itinéraires au fichier routes.yaml dans l’environnement d’intégration et vérifiez la configuration dans cet environnement avant de procéder au déploiement dans les environnements d’évaluation et de production.

        »http://{all}/ »:
       type : amont
       amont : « mymagento:http »
       
        »http://{all}/ »:
       type : amont
       amont : « mymagento:http »
   
5. Assurez-vous que XDebug est désactivé s’il est activé pendant le développement (voir [Configurer Xdebug](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/){target="_blank"}).
6. Vérifiez que l&#39;op-cache et les autres configurations ont été correctement mis à jour dans le fichier php.ini ([voir cet exemple](https://github.com/magento/magento-cloud/blob/master/php.ini#L41){target="_blank"}).
7. Abonnez-vous à la page de statut d’Adobe Commerce [**&#128279;**](https://status.adobe.com/cloud/experience_cloud#/){target="_blank"}.
8. Abonnez-vous aux canaux de notification « [Alertes gérées pour Adobe Commerce](https://experienceleague.adobe.com/fr/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce){target="_blank"} » de New Relic pour surveiller les mesures de performances données ([en savoir plus](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/monitor/new-relic/new-relic-service){target="_blank"}).

## &#x200B;9. Sécurité

1. Configuration de l’analyse de sécurité Adobe Commerce

   >[!NOTE]
   > [L&#39;analyse de sécurité Adobe Commerce est un outil utile](https://experienceleague.adobe.com/fr/docs/commerce-admin/systems/security/security-scan){target="_blank"} qui permet de découvrir des versions logicielles obsolètes, une configuration incorrecte et des programmes malveillants potentiels sur le site. Inscrivez-vous, planifiez une exécution fréquente et assurez-vous que les e-mails sont envoyés au contact technique de sécurité approprié.
   > 
   > Effectuez cette tâche pendant UAT. Si vous utilisez l’option d’analyses périodiques, veillez à planifier des analyses aux moments de faible demande. Voir la page [Analyse de sécurité](https://account.magento.com/scanner/index/dashboard/){target="_blank"} dans le compte Adobe Commerce. Vous devez vous connecter à un compte Adobe Commerce pour accéder à l’analyse de sécurité.

2. Modifiez les paramètres par défaut de l’administrateur Adobe Commerce.
3. Modifiez le mot de passe administrateur (voir [Configuration de la sécurité administrateur](https://experienceleague.adobe.com/fr/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
4. Modifiez l’URL d’administration (voir [Utilisation d’une URL d’administration personnalisée](https://experienceleague.adobe.com/fr/docs/commerce-admin/stores-sales/site-store/store-urls#use-a-custom-admin-url){target="_blank"}).
5. Supprimez tous les utilisateurs qui ne font plus partie du projet (voir [Création et gestion des utilisateurs](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/project/user-access){target="_blank"}).
6. Les mots de passe des administrateurs sont configurés (voir [Exigences de mot de passe administrateur](https://experienceleague.adobe.com/fr/docs/commerce-admin/systems/security/security-admin){target="_blank"}).
7. Configurez l’authentification à deux facteurs (voir [&#x200B; Authentification à deux facteurs &#x200B;](https://developer.adobe.com/commerce/testing/functional-testing-framework/two-factor-authentication/){target="_blank"}).

## &#x200B;10. Activation

Au moment du basculement, procédez comme suit (pour plus d’informations, voir [Configurations DNS](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}) :

1. Accédez à votre service DNS et mettez à jour les enregistrements A et CNAME pour chacun de vos domaines et noms d’hôte :
   1. Ajoutez un enregistrement CNAME pour _&lt;&lt;www.yourdomain.com>>_, pointant vers **prod.magentocloud.map.fastly.net**
   2. Définissez quatre enregistrements A pour _&lt;&lt;yourdomain.com>>_, pointant vers :\
      151.101.1.124\
      151.101.65.124\
      151.101.129.124\
      151.101.193.124
2. Modifiez l’URL de base d’Adobe Commerce en _&lt;&lt;www.yourdomain.com>>_
3. Patientez jusqu’à ce que le temps de TTL soit écoulé, puis redémarrez le navigateur web.
4. Testez le site web.

### Si vous rencontrez un problème qui bloque la mise en production :

Si vous rencontrez des problèmes ou des problèmes qui vous empêchent de lancer pendant la transition, la méthode la plus rapide pour obtenir une assistance en temps voulu consiste à utiliser le Help Desk et à ouvrir un ticket avec le motif « Impossible de lancer ma boutique » et à appeler un numéro d’assistance téléphonique (voir [la liste des numéros d’assistance d’Adobe Commerce P1 (priorité 1)](https://support.magento.com/hc/en-us/articles/360042536151){target="_blank"}) :

- Numéro gratuit pour les États-Unis : (+1) 877 282 7436 (direct vers la ligne d’assistance P1 d’Adobe Commerce)
- Numéro gratuit : (+1) 800 685 3620 (Dans le premier menu, appuyez sur 7 pour la ligne d&#39;assistance Adobe Commerce P1)
- US Local : (+1) 408 537 8777

## &#x200B;11. Post-activation

Une fois le site en ligne, envoyez un e-mail au CTA (service consultatif technique du client), au CSE (ingénieur du succès client) et à AEM (gestionnaire de compte) désignés. Cependant, si aucun gestionnaire de compte n’est affecté au projet, vous pouvez créer un ticket d’assistance demandant que la surveillance High SLA soit activée une fois le site activé. Le CTA/CSE effectue les tâches suivantes dès que le lancement du site est vérifié avec Fastly activé et mis en cache :

- Marquez le cluster comme actif et créez un ticket d’assistance pour activer la surveillance High SLA (Service Level Agreements).
- Activez New Relic Synthetics pour la surveillance de la disponibilité.

>[!MORELIKETHIS]
> 
> - [Présentation de la préparation à Launch - Manuel d’implémentation](https://experienceleague.adobe.com/fr/docs/commerce-operations/implementation-playbook/best-practices/launch/overview){target="_blank"}
> - [Liste de contrôle Launch - Guide de l’utilisateur](https://experienceleague.adobe.com/fr/docs/commerce-cloud-service/user-guide/launch/checklist){target="_blank"}
> - [Liste de contrôle de prélancement - Guide de l’administrateur de Site Manager/Commerce](https://experienceleague.adobe.com/fr/docs/commerce-admin/start/setup/prelaunch-checklist){target="_blank"}
> - [Modèle de sécurité de responsabilité partagée](https://experienceleague.adobe.com/fr/docs/commerce-operations/security-and-compliance/shared-responsibility){target="_blank"}
