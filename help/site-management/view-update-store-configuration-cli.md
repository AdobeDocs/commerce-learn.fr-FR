---
title: Afficher et définir les configurations d’administrateur à l’aide de la ligne de commande
description: Découvrez comment afficher et définir des configurations d’administrateur à l’aide de la ligne de commande.
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 462
last-substantial-update: 2024-01-31T00:00:00Z
jira: KT-14877
thumbnail: KT-14877.jpeg
exl-id: 6cecba51-8d39-46f5-9864-80126d8ca3da
source-git-commit: 48a98261a827741459e45f14f7463f4a989c49d2
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# Afficher et définir les configurations d’administrateur à l’aide de la ligne de commande

Une démonstration pour savoir comment afficher, définir et rechercher des valeurs de configuration avec l’interface de ligne de commande de Commerce. Déterminez d’où sont enregistrées les valeurs et d’où viennent les valeurs par défaut.

## Pour qui est cette vidéo ?

- Développeurs Adobe Commerce

## Contenu vidéo

>[!VIDEO](https://video.tv.adobe.com/v/3427123?&learn=on)

## Certaines commandes utilisées dans le tutoriel

Définissez le paramètre de protection de mot de passe sur recommandé :

`$ php bin/magento config:set admin/security/password_is_forced 0`

Afficher l’adresse électronique de la fonctionnalité de copie automatique de la commande client

`$ php bin/magento config:show sales_email/order/copy_to`

Afficher le résultat vide pour une configuration ayant une valeur dans l’administrateur

`php bin/magento config:show trans_email/ident_sales/email`

## Requêtes Mysql utilisées dans le tutoriel

```
SELECT * FROM core_config_data WHERE path = 'sales_email/order/copy_to';

SELECT * FROM core_config_data WHERE path = 'sales_email/order_comment/copy_to';

SELECT * FROM core_config_data WHERE path = 'trans_email/ident_sales/email';
```

## Où trouver l’email de vente par défaut

Comment trouver la valeur de configuration définie quelque part dans le code base ?
`grep -rnw vendor/magento/ -e 'sales@example.com'`

Pour afficher une page dans un terminal et afficher les numéros de ligne `cat -n vendor/magento/module-email/etc/config.xml`

## Ressources supplémentaires

- [Outil de ligne de commande](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html){target="_blank"}
- [Configuration de la sécurité d’administration](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html){target="_blank"}
