---
title: Création d’un module
description: Créez une page qui renvoie json avec un paramètre.
topic: Development
kt: 5614
doc-type: video
activity: use
last-substantial-update: 2023-6-2
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: fb2cb5b844c4156802e8627e71fc18f8e7fa981d
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# Création d’un module

Le module est un élément structurel de [!DNL Commerce] - l’ensemble du système repose sur des modules. En règle générale, la première étape de la création d’une personnalisation est la création d’un module.

## Pour qui est cette vidéo ?

- Développeurs

## Étapes pour ajouter un module

- Créez le dossier du module.
- Créez le fichier etc/module.xml .
- Créez le fichier registration.php .
- Exécutez la configuration bin/magento.
- Mettre à niveau le script pour installer le nouveau module.
- Vérifiez que le module fonctionne.

>[!VIDEO](https://video.tv.adobe.com/v/35792?learn=on)

### module.xml

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Training_Sales">
        <sequence>
            <module name="Magento_Sales"/>
        </sequence>
    </module>
</config>
```

### registration.php

```PHP
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

## Ressources utiles

- [Guide de référence des modules](https://developer.adobe.com/commerce/php/module-reference/)
