---
title: Création d’un module
description: Découvrez comment créer un module dans Adobe Commerce qui envoie des informations au journal PSR. Cette opération ajoute la fonctionnalité à votre premier module dans Adobe Commerce.
kt: 5614
doc-type: video
activity: use
last-substantial-update: 2023-6-2
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, Developer
level: Beginner, Intermediate
exl-id: 941c04ee-54b8-4b81-b77d-fff5875927f0
source-git-commit: 4f6c8abec90663f80233b94456ad1e58edb86d51
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---

# Création d’un module

Le module est un élément structurel de [!DNL Commerce] : l’ensemble du système est construit sur des modules. En règle générale, la première étape de la création d’une personnalisation est la création d’un module.

## Pour qui est cette vidéo ?

- Développeurs

## Étapes pour ajouter un module

- Créez le dossier du module.
- Créez le fichier etc/module.xml .
- Créez le fichier registration.php .
- Exécutez la configuration bin/magento.
- Mettre à niveau le script pour installer le nouveau module.
- Vérifiez que le module fonctionne.

>[!VIDEO](https://video.tv.adobe.com/v/3412452?learn=on&captions=fre_fr)

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

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Training_Sales',
    __DIR__);
```

### Ajouter un module externe et fournir certaines fonctionnalités

L’étape suivante consiste à ajouter des fonctionnalités à notre module de base. Un module externe est un outil essentiel que tous les développeurs Adobe Commerce utilisent. Cette vidéo et ce tutoriel vous aident à créer un module externe.

>[!VIDEO](https://video.tv.adobe.com/v/3420255?learn=on)

### Informations à retenir pour les modules externes

- Tous les modules externes sont déclarés dans `di.xml`.
- Le module externe nécessite un nom unique.
- disabled et sortOrder sont facultatifs
- La portée du module externe est définie par le dossier dans lequel il se trouve.
- Les modules externes peuvent être exécutés avant, après ou après l’appel de la méthode.
- Évitez d’utiliser des modules externes `around`. Ils sont tentants d’utiliser, mais sont souvent le mauvais choix et entraîneront des problèmes de performances.

### Exemples de code de module externe

Voici les classes XML et PHP utilisées dans le tutoriel pour ajouter un module externe au premier module.

### app/code/Training/Sales/etc/adminhtml/di.xml

```xml
<?xml version="1.0" ?>
<!--
/**
 * Copyright &copy; Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- A Plugin that executes when the admin user places an order -->
    <type name="Magento\Sales\Model\Order">
        <plugin name="admin-training-sales-add-logging" type="Training\Sales\Plugin\AdminAddLoggingAfterOrderPlacePlugin" disabled="false" sortOrder="0"/>
    </type>
</config>
```

### app/code/Training/Sales/etc/frontend/di.xml

```xml
<?xml version="1.0" ?>
<!--
/**
 * Copyright &copy; Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- A plugin that executes when a customer uses the LoginPost controller from the Luma frontend -->
    <type name="Magento\Customer\Controller\Account\LoginPost">
        <plugin name="training-customer-loginpost-plugin"
                type="Training\Sales\Plugin\CustomerLoginPostPlugin" sortOrder="20"/>
    </type>
</config>
```

### app/code/Training/Sales/etc/webapi_rest/di.xml

```xml
<?xml version="1.0" ?>
<!--
/**
 * Copyright &copy; Magento, Inc. All rights reserved.
 * See COPYING.txt for license details.
 */
-->
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <!-- A plugin that executes when the REST API is used OR when the Luma frontend places an order -->
    <type name="Magento\Sales\Model\Order">
        <plugin name="rest-training-sales-add-logging" type="Training\Sales\Plugin\RestAddLoggingAfterOrderPlacePlugin"/>
    </type>
</config>
```

### app/code/Training/Sales/Plugin/AdminAddLoggingAfterOrderPlacePlugin.php

```php
<?php

declare(strict_types=1);

namespace Training\Sales\Plugin;

use Magento\Sales\Model\Order;
use Psr\Log\LoggerInterface;

/**
 *
 */
class AdminAddLoggingAfterOrderPlacePlugin
{

    /**
     * @var LoggerInterface
     */
    private LoggerInterface $logger;

    /**
     * @param LoggerInterface $logger
     */
    public function __construct(
        LoggerInterface $logger
    ) {
        $this->logger = $logger;
    }

    /**
     * Add log after an order is placed
     *
     * @param Order $subject
     * @param Order $result
     * @return Order
     */
    public function afterPlace(Order $subject, Order $result): Order
    {
        // Log this admin area order
        $this->logger->notice('An ADMIN User placed an Order - $' . $subject->getBaseTotalDue());
        return $result;
    }
}
```

### app/code/Training/Sales/Plugin/CustomerLoginPostPlugin.php

```php
<?php

declare(strict_types=1);

namespace Training\Sales\Plugin;

use Psr\Log\LoggerInterface;
use Magento\Framework\App\RequestInterface;

/**
 * Introduces Context information for ActionInterface of Customer Action
 */
class CustomerLoginPostPlugin
{

    /**
     * @var LoggerInterface
     */
    private LoggerInterface $logger;

    /**
     * @var RequestInterface
     */
    private RequestInterface $request;

    /**
     * @param LoggerInterface $logger
     * @param RequestInterface $request
     */
    public function __construct(LoggerInterface $logger, RequestInterface $request)
    {
        $this->logger = $logger;
        $this->request = $request;
    }

    /**
     * Simple example of a before Plugin on a public method in a Controller
     */
    public function beforeExecute()
    {
        $login = $this->request->getParam('login');
        $username = $login['username'];
        $this->logger->notice( "Login Post controller was used for " . $username );
    }
}
```

### app/code/Training/Sales/Plugin/RestAddLoggingAfterOrderPlacePlugin.php

```php
<?php

declare(strict_types=1);

namespace Training\Sales\Plugin;

use Magento\Sales\Model\Order;
use Psr\Log\LoggerInterface;

/**
 *
 */
class RestAddLoggingAfterOrderPlacePlugin
{

    /**
     * @var LoggerInterface
     */
    private LoggerInterface $logger;

    /**
     * @param LoggerInterface $logger
     */
    public function __construct(
        LoggerInterface $logger
    ) {
        $this->logger = $logger;
    }

    /**
     * Add log after an order is placed
     *
     * @param Order $subject
     * @param Order $result
     * @return Order
     */
    public function afterPlace(Order $subject, Order $result): Order
    {
        // Log this customer driven order
        $this->logger->notice('A Customer placed an Order for $' . $subject->getBaseTotalDue());
        return $result;
    }
}
```

## Ressources utiles

- [Guide de référence de module](https://developer.adobe.com/commerce/php/module-reference/){target="_blank"}
- [Plugins](https://developer.adobe.com/commerce/php/development/components/plugins/){target="_blank"}
