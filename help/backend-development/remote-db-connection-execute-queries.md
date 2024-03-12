---
title: Connecter et exécuter des requêtes sur la base de données
description: Découvrez plusieurs méthodes pour vous connecter à un projet cloud Adobe Commerce. Découvrez comment extraire une base de données pour utiliser hors site. Découvrez certaines méthodes pour masquer les informations d’identification personnelles et les supprimer.
feature: Backend Development,Console,Cloud
topic: Commerce,Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
duration: 0
last-substantial-update: 2024-02-14T00:00:00Z
jira: KT-14910
thumbnail: KT-14910.jpeg
exl-id: e740bbd0-5ec7-4272-89cb-0bed776eb149
source-git-commit: a951f61ff71ad3777f8aebfa3c237b2ec1a4b1a5
workflow-type: tm+mt
source-wordcount: '1047'
ht-degree: 0%

---

# Connexion et exécution de requêtes sur la base de données Adobe Commerce

Dans ce tutoriel, vous découvrez comment vous connecter à un projet Adobe Commerce sur le cloud, vider une base de données pour l’utiliser hors site, masquer les informations d’identification personnelles et la supprimer.

Vous pouvez accéder aux données Adobe Commerce à partir de votre projet cloud en utilisant l’une des méthodes suivantes :

* Utilisation d’un vidage DB local
* Une connexion DB à votre environnement cloud distant à l’aide d’une application telle que Mysql Workbench ou Tables Plus
* Connectez-vous directement à l’environnement cloud à l’aide de l’outil de ligne de commande magento-cloud et exécutez les commandes sur le serveur distant

La méthode recommandée consiste à effectuer un vidage de base de données et à le faire défiler pour supprimer toutes les informations sur les clients. Supprimez entièrement les données du client si elles ne sont pas nécessaires.

## Utilisation de l’outil d’interface de ligne de commande Adobe Commerce Cloud

Pour créer un vidage de base de données, vous devez disposer de la variable [Interface de ligne de commande Adobe Commerce Cloud](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html) installé. Sur votre ordinateur portable local, accédez à un répertoire et exécutez la commande suivante. Veillez à remplacer `your-project-id` avec l’ID de projet, qui est similaire à `asasdasd45q`. Vous devez également remplacer `your-environment-name` avec le nom de votre environnement, par exemple `master` ou `staging`.

`magento-cloud db:dump -p your-project-id -e your-environment-name`

Si vous n’êtes pas sûr de l’ID de projet ou de l’environnement, vous pouvez les omettre dans la commande :

`magento-cloud db:dump`

L’interface en ligne de commande vous demande de spécifier le projet et l’environnement appropriés. L’exemple suivant illustre cette boîte de dialogue. Cet exemple présente plusieurs projets affectés à votre compte, mais il est probable qu’un seul projet soit disponible.

Changement dans un répertoire

```bash
cd ~/Downloads/db-tutorial 
```

Exécutez maintenant la commande pour créer le vidage de la base de données.

```bash
magento-cloud db:dump
```

Comme nous n’avons pas spécifié de projet ou d’environnement, l’interface de ligne de commande d’Adobe Commerce va poser quelques questions. Voici un exemple de boîte de dialogue.

```bash
Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Creating SQL dump file: /Users/<username>/Downloads/db-tutorial/abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
```

## Utilisation des outils ECID Adobe Commerce

Si vous ne disposez pas de l’outil d’interface de ligne de commande Adobe Commerce, vous pouvez `ssh` dans votre projet et exécutez le `ece` command `vendor/bin/ece-tools db-dump`: exemple de réponse :

```bash
ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud

 __  __                   _          ___ _             _ 
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/                                        

 Welcome to Magento Cloud.

 This is environment remote-db-ecpefky
 of project abasrpikfw4123.

web@mymagento.0:~$ vendor/bin/ece-tools db-dump
The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
Your site will not receive any traffic until the operation completes.
Do you wish to proceed with this process? (y/N)?y
[2024-02-13T19:01:45.130999+00:00] INFO: Starting backup.
[2024-02-13T19:01:45.155039+00:00] NOTICE: Enabling Maintenance mode
[2024-02-13T19:01:46.404427+00:00] INFO: Trying to kill running cron jobs and consumers processes
[2024-02-13T19:01:46.420149+00:00] INFO: Running Magento cron and consumers processes were not found.
[2024-02-13T19:01:46.420434+00:00] INFO: Waiting for lock on db dump.
[2024-02-13T19:01:46.420499+00:00] INFO: Start creation DB dump for main database...
[2024-02-13T19:01:50.697886+00:00] INFO: Finished DB dump for main database, it can be found here: /app/var/dump-main-1707850906.sql.gz
[2024-02-13T19:01:51.628328+00:00] NOTICE: Maintenance mode is disabled.
[2024-02-13T19:01:51.628419+00:00] INFO: Backup completed.
web@mymagento.0:~$ exit
logout
Connection to ssh.us-4.magento.cloud closed.
```

Utilisation `SFTP` ou `rsync` pour extraire la mémoire de base de données dans votre environnement local.

L’exemple suivant utilise `rsync` pour extraire le fichier dans la variable `~/Downloads/db-tutorial` dossier.

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
```

La fenêtre de terminal génère certaines informations, voici quelques exemples de sortie.

```bash
rsync -avrp -e ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud:/app/var/dump-main-1707850906.sql.gz ~/Downloads/db-tutorial
receiving file list ... done
dump-main-1707850906.sql.gz

sent 38 bytes  received 2691041 bytes  358810.53 bytes/sec
total size is 2690241  speedup is 1.00
```

Affichez le contenu du fichier pour vérifier qu’il a bien été téléchargé.

```bash
ls -lah
total 29840
drwxr-xr-x    4 <ussername>  staff   128B Feb 13 13:02 .
drwx------@ 103 <ussername>   staff   3.2K Feb 13 12:52 ..
-rw-r--r--    1 <ussername>   staff    11M Feb 13 12:53 abasrpikfw4123--remote-db-ecpefky--mysql--main--dump.sql
-rw-r--r--    1 <ussername>   staff   2.6M Feb 13 13:01 dump-main-1707850906.sql.gz
```

Une fois que vous disposez des données, veillez à les nettoyer en supprimant ou en masquant les données du client. L’exemple de script suivant vous aidera à démarrer.

Cet exemple transforme les données client en chaînes aléatoires, mais conserve tous les éléments. Cet exemple contient quelques tables supplémentaires pour démontrer que les informations d’identification personnelle des clients se trouvent dans les tables tierces ainsi que dans les tables principales. Examinez attentivement les données de chaque tableau et masquez ou supprimez les données du client.

En règle générale, l’architecte ou le développeur principal est le seul responsable du masquage et de l’assainissement des vidages de base de données. Disposer d’un assainisseur dédié réduit l’exposition des données brutes, ce qui réduit les possibilités de violation des règles et réglementations de conformité.

```sql
SET FOREIGN_KEY_CHECKS=0;
UPDATE customer_entity SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE email_contact SET email = REPLACE(email, SUBSTRING(email, LOCATE('@', email) +1), CONCAT(UUID(), '.com'));
UPDATE sales_invoice_grid SET customer_email = 'customer@example.com', customer_name  = 'Jack Smith';
UPDATE sales_order SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Smith', remote_ip = '127.0.0.1';
UPDATE sales_order_address SET region = 'Ohio', postcode = '12345-1234', lastname = 'Smith', street = '123 Main street', region_id = 44, city = 'Phoenix', telephone = NULL, firstname = 'Jane', company = NULL;
UPDATE sales_order_grid SET customer_email = 'customer@example.com', shipping_name = 'Jack', billing_name = 'Jack Smith', billing_address = '123 Main Street', shipping_address = '321 Pine Street', customer_name = 'Jane Smith';
UPDATE sales_shipment_grid SET customer_email = 'customer@example.com', customer_name = 'Jane Smith', billing_address = '123 Main street', billing_name = 'Jack Doe', shipping_name = 'Susie Smith';
UPDATE quote SET customer_email = 'customer@example.com', customer_firstname = 'Sally', customer_lastname = 'Jones', customer_dob = NULL, remote_ip = '127.0.0.1';
UPDATE quote_address SET email = 'customer@example.com', firstname = 'Jack', lastname = 'Smith', company = NULL, street = '123 Main st', city = 'AnyCity', region = 'Some State', region_id = 44, postcode = '12345-1234', telephone = NULL;
UPDATE magento_rma SET customer_custom_email = 'customer@example.com' WHERE customer_custom_email IS NOT NULL;
UPDATE customer_address_entity SET firstname = 'Jack', lastname = 'Smith', telephone = '909-555-1212', postcode = NULL,  region = NULL, street = '123 Main street', city = 'Anycity', company = NULL;
UPDATE customer_grid_flat SET name = 'Jane Doe', email = 'customer@example.com', dob = NULL, gender = NULL, taxvat = NULL, shipping_full = '', billing_full = '', billing_firstname = 'Jack', billing_lastname = 'Smith', billing_telephone = NULL, billing_postcode = NULL, billing_country_id = NULL, billing_region = NULL, billing_street = '123 Main street', billing_city = 'Anycity', billing_fax = NULL, billing_vat_id = NULL, billing_company = NULL;
UPDATE sales_creditmemo_grid SET billing_name = 'Sally', billing_address = '123 Main Street', customer_name = 'Jack Smith', customer_email = 'customer@example.com';
UPDATE magento_rma_grid SET customer_name = 'Jack Smith';
UPDATE newsletter_subscriber SET subscriber_email = 'customer@example.com';
UPDATE core_config_data SET value = '' WHERE path = 'orderexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'productexport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'trackingimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'stockimport/general/serial';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'remarketing/onescript/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/application_id';
UPDATE core_config_data SET value = '' WHERE path = 'algoliasearch_credentials/credentials/search_only_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_account_number';
UPDATE core_config_data SET value = '' WHERE path = 'tax/avatax/production_license_key';
UPDATE core_config_data SET value = '' WHERE path = 'design/head/includes';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/merchant_id';
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/public_key';     
UPDATE core_config_data SET value = '' WHERE path = 'payment/braintree/private_key';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_service_id';
UPDATE core_config_data SET value = '' WHERE path = 'system/full_page_cache/fastly/fastly_api_key';
UPDATE core_config_data SET value = '' WHERE path = 'google/analytics/container_id';  
UPDATE core_config_data SET value = '' WHERE path = 'analytics/general/token';
UPDATE vault_payment_token SET public_hash = UUID(), details = '{"type":"VI","maskedCC":"1111","expirationDate":"01\/2019"}';
TRUNCATE customer_log; 
TRUNCATE customer_visitor; 
TRUNCATE magento_logging_event;
TRUNCATE oauth_consumer;
TRUNCATE oauth_nonce;
TRUNCATE oauth_token;
TRUNCATE password_reset_request_event;
TRUNCATE acknowledgement;
TRUNCATE acknowledgement_report;
TRUNCATE avatax_log;
TRUNCATE avatax_queue;
TRUNCATE cron_schedule;
SET FOREIGN_KEY_CHECKS=1;
```

Vous pouvez également supprimer les enregistrements au lieu de masquer les informations, ce qui réduit également la nouvelle base de données. Une fois les informations d’identification personnelles masquées ou supprimées, les données peuvent être fournies en toute sécurité à un membre de l’équipe en vue de leur utilisation dans son environnement local.

## Connexion à une base de données distante à un projet Adobe Commerce Cloud

Cette méthode permet la modification et la suppression accidentelles de données réelles. Cette approche doit être utilisée avec précaution. Il est préférable d’utiliser une sauvegarde de base de données et de passer en revue les données hors ligne. Il est parfois nécessaire d’accéder aux données directement sur Adobe Commerce Cloud, mais cela comporte toutefois des risques. Il n&#39;y a pas de &quot;êtes-vous sûr ?&quot; questions posées, il est donc possible de modifier ou de supprimer des données par inadvertance.

Super important ! L’utilisation d’une connexion à base de données distante est pratique et de données réelles en direct, mais présente des risques. Personnellement, et en tant qu’architecte technique principal d’Adobe Commerce, je ne le recommande pas. Il est trop facile d’oublier que vous êtes sur la base de données distante et de supprimer ou de modifier accidentellement des données. Il existe une option pour se connecter à la réplication en lecture seule, mais cela a un impact sur le site en fonction de la charge des activités SQL. Cependant, comme c&#39;est possible, voici les étapes à suivre.

Installez un tunnel SSH :

```bash
magento-cloud tunnel:open
```

Une fois le projet sélectionné et l&#39;environnement sélectionné, une sortie de la commande est utilisée dans les paramètres de l&#39;interface graphique mysql.

```bash
magento-cloud tunnel:open

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
SSH tunnel opened to redis at: redis://127.0.0.1:30001
SSH tunnel opened to opensearch at: http://127.0.0.1:30002
SSH tunnel opened to rabbitmq at: amqp://guest:guest@127.0.0.1:30003

Logs are written to: /Users/<user>/.magento-cloud/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close

Save encoded tunnel details to the MAGENTO_CLOUD_RELATIONSHIPS variable using:
  export MAGENTO_CLOUD_RELATIONSHIPS="$(magento-cloud tunnel:info --encode)"
```

établir une connexion à l’aide d’une interface graphique MySQL à l’aide de la fonction `SSH tunnel opened to database at` de la commande.

```bash
SSH tunnel opened to database at: mysql://user:@127.0.0.1:30000/main
```

Maintenant que vous disposez des informations appropriées, continuez à insérer ces valeurs dans la console Cloud.

Vous trouverez le nom d’hôte SSH et le nom d’utilisateur à partir des informations d’identification du cloud dans la console cloud.

![logo - Console Adobe Commerce Cloud](./assets/cloud-ui-screenshot.png "Console Adobe Commerce Cloud")

Voici un exemple : `ssh abasrpikfw4123-remote-db-ecpefky--mymagento@ssh.us-4.magento.cloud`
Le nom d’hôte SSH est tout ce qui suit le signe @ : `ssh.us-4.magento.cloud` dans cet exemple.
Le nom d’utilisateur SSH est tout ce qui précède le signe @ :  `abasrpikfw4123-remote-db-ecpefky—mymagento`

## Recherche de valeurs pour la connexion à la base

L’accès direct à la base de données MariaDB nécessite l’utilisation du SSH pour se connecter à l’environnement cloud distant et se connecter à la base de données.

1. Utilisez SSH pour vous connecter à l’environnement distant.

   ```bash
   magento-cloud ssh
   ```

1. Récupérez les informations d’identification de connexion MySQL à partir du `database` et `type` dans le [$MAGENTO_CLOUD_RELATIONSHIPS](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/properties/properties.html?lang=en#relationships) Variable .

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   ou

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Dans la réponse, recherchez les informations MySQL. Par exemple :

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

Utilisez ensuite les valeurs de configuration dans votre interface utilisateur graphique MySQL. L’exemple suivant utilise MySQL Workbench, mais toute application prenant en charge les connexions MySQL aura des champs similaires.

![Logo - Exemple d’interface utilisateur graphique Mysql Workbench](./assets/mysql-workbench-after-connecting.png " Exemple d’interface utilisateur graphique Mysql Workbench")

![logo : exemple d’interface utilisateur graphique Mysql avec TablePlus](./assets/tablesPlus-db-connection.png " Exemple d’interface utilisateur graphique Mysql avec TablePlus")

Une fois tout configuré, il est possible d’utiliser une interface utilisateur graphique MySQL pour exécuter des requêtes sur un projet Adobe Commerce Cloud distant.

## Connexion directe à la base de données de projet cloud pour exécuter SQL

La méthode suivante utilise la méthode `magento-cloud` cli pour se connecter directement à la base de données mysql et exécuter SQL, ce qui permet une interrogation plus rapide de la base de données. Si vous devez copier cette base de données, reportez-vous à l’une des méthodes alternatives pour [créer un vidage de base de données](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html).

```bash
magento-cloud db:sql    

Enter a number to choose a project:
  [0] demo-ralbin (ral32nryq4123)
  [1] adobe-commerce-demo (abc123zzkipexnqo)
  [2] DX Tutorials - Commerce (abasrpikfw4123)
 > 2

Enter a number to choose an environment:
Default: master
  [0] master (type: production)
  [1] remote-db (type: development)
 > 1

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 273454
Server version: 10.6.15-MariaDB-1:10.6.15+maria~deb10-log mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

Par exemple, vous pouvez trouver tous les enregistrements de la `core_config_data` tableau contenant le mot `secure` dans la colonne `path`:

```sql
MariaDB [main]> SELECT * FROM core_config_data WHERE path LIKE '%secure%' \G;
*************************** 1. row ***************************
 config_id: 5
     scope: default
  scope_id: 0
      path: web/unsecure/base_url
     value: http://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 2. row ***************************
 config_id: 6
     scope: default
  scope_id: 0
      path: web/secure/base_url
     value: https://remote-db-ecpefky-abasrpikfw4123.us-4.magentosite.cloud/
updated_at: 2024-02-02 18:03:17
*************************** 3. row ***************************
 config_id: 8
     scope: default
  scope_id: 0
      path: web/secure/use_in_adminhtml
     value: 1
updated_at: 2023-04-26 19:43:58
3 rows in set (0.001 sec)

ERROR: No query specified

MariaDB [main]> 
```

## Ressources supplémentaires

[Interface de ligne de commande Adobe Commerce Cloud](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/dev-tools/cloud-cli/cloud-cli-overview.html)
[Configuration du service MySQL](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/mysql.html)
[Configuration d’une connexion de base de données MySQL distante](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/database-server/mysql-remote.html)
[Création d’un vidage de base de données sur Adobe Commerce sur l’infrastructure cloud](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
