---
title: Afficher et définir les configurations d’administration à l’aide de la ligne de commande
description: Découvrez comment afficher et définir des configurations d’administration à l’aide de la ligne de commande.
feature: Configuration,Console,System
topic: Administration,Commerce
role: Developer
level: Beginner
doc-type: Technical Video
duration: 510
last-substantial-update: 2024-01-31T00:00:00.000Z
jira: KT-14877
exl-id: 6cecba51-8d39-46f5-9864-80126d8ca3da
TQID: https://experienceleague.adobe.com/W0kaFd-DJAPA1kFO0hWaBZXsSarojbt5Hnv3QkW85hA
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: ba9e5be9-7de1-4f71-a5d2-baead0e425eeid: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: d095671a-1355-40aa-8b5f-06c33c68080bid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: b599f79ad41b9552cea6ff41062eb4ef75f183bb
workflow-type: tm+mt
source-wordcount: 168
ht-degree: 0%

---

# Afficher et définir les configurations d’administration à l’aide de la ligne de commande

Une démonstration expliquant comment afficher, définir et rechercher des valeurs de configuration avec l’interface de ligne de commande Commerce. Identifiez l’emplacement d’enregistrement des valeurs et l’origine des valeurs par défaut.

## À qui s&#39;adresse cette vidéo ?

* Développeurs Adobe Commerce

## Contenu vidéo

>[!VIDEO](https://video.tv.adobe.com/v/3427123?learn=on)

## Certaines commandes utilisées dans le tutoriel

Modifiez le paramètre de sécurité du mot de passe sur recommandé :

`$ php bin/magento config:set admin/security/password_is_forced 0`

Afficher l&#39;adresse e-mail pour la fonctionnalité de copie automatique de la commande client

`$ php bin/magento config:show sales_email/order/copy_to`

Afficher le résultat vide pour une configuration qui a une valeur dans l’administrateur

`php bin/magento config:show trans_email/ident_sales/email`

## Requêtes Mysql utilisées dans le tutoriel

```
SELECT * FROM core_config_data WHERE path = 'sales_email/order/copy_to';

SELECT * FROM core_config_data WHERE path = 'sales_email/order_comment/copy_to';

SELECT * FROM core_config_data WHERE path = 'trans_email/ident_sales/email';
```

## Où trouver l’e-mail de vente par défaut

Comment trouver la valeur de configuration définie quelque part dans codebase ?
`grep -rnw vendor/magento/ -e 'sales@example.com'`

Pour afficher une page dans le terminal et afficher les numéros de ligne `cat -n vendor/magento/module-email/etc/config.xml`

## Ressources supplémentaires

* [Outil de ligne de commande](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/config-cli.html){target="_blank"}
* [Configuration de la sécurité de l’administrateur](https://experienceleague.adobe.com/docs/commerce-admin/systems/security/security-admin.html){target="_blank"}
