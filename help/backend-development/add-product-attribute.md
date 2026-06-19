---
title: Création d’un attribut de produit
description: Découvrez comment créer un attribut de produit par programmation dans Adobe Commerce, y compris des attributs de type liste déroulante avec des modèles personnalisés d’arrière-plan, frontaux et sources.
doc-type: Tutorial
duration: 491
last-substantial-update: 2023-02-10
feature: Configuration, System, Backend Development
topic: Commerce, Development
role: Admin, User
level: Beginner, Intermediate
jira: KT-14131
exl-id: 98257e62-b23d-4fa9-a0eb-42e045c53195
TQID: https://experienceleague.adobe.com/nK2d0LjWl88FCVRW6IRCa-euZ8cTawlY3YOzjMyNq-Y
product_v2: id: eadea719-cf89-469b-a6fd-a236a7138047
feature_v2: id: c1256247-af4b-46d8-9dca-0c654ecfa157id: dac87252-6066-4d6e-a9d2-f6d84c323de7
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: add3e29f8841ca4ca99f4c40afc656f00e93ec36
workflow-type: tm+mt
source-wordcount: 337
ht-degree: 0%

---

# Création d’un attribut de produit

L’ajout d’un attribut de produit est l’une des opérations les plus populaires dans [!DNL Commerce]. Les attributs sont un moyen efficace d’effectuer de nombreuses tâches pratiques liées à un produit. Il existe un processus simple pour ajouter un attribut de type liste déroulante à un produit.

Dans cette vidéo :

* Ajoutez un attribut appelé habillement_matériau avec les valeurs possibles : coton, cuir, soie, denim, fourrure et laine
* Rendre cet attribut visible sur la page de consultation du produit, en gras
* Attribuez-le au jeu d’attributs par défaut et ajoutez une restriction
* Ajouter le nouvel attribut

## À qui s&#39;adresse cette vidéo ?

* Développeurs peu familiers avec [!DNL Commerce] qui doivent apprendre à créer un attribut de produit par programmation

## Contenu vidéo

>[!VIDEO](https://video.tv.adobe.com/v/35789?learn=on)

## Exemple de code

Créez d&#39;abord les dossiers, les fichiers xml et PHP nécessaires :

* app/code/Learning/ClothingMaterial/registration.php
* app/code/Learning/ClothingMaterial/etc/module.xml
* app/code/Learning/ClothingMaterial/Model/Attribute/Backend/Material.php
* app/code/Learning/ClothingMaterial/Model/Attribute/Frontend/Material.php
* app/code/Learning/ClothingMaterial/Model/Attribute/Source/Material.php
* app/code/Learning/ClothingMaterial/Setup/InstallData.php

### app/code/Learning/ClothingMaterial/registration.php

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,
    'Learning_ClothingMaterial',
    __DIR__);
```

### app/code/Learning/ClothingMaterial/etc/module.xml

>[!NOTE]
>
>Si votre module utilise un schéma déclaratif, et que la plupart l’ont fait depuis la version 2.3.0, vous devez omettre setup_version. Toutefois, si vous disposez de projets hérités, il se peut que cette méthode soit utilisée.  Voir [developer.adobe.com](https://developer.adobe.com/commerce/php/development/build/component-name/#add-a-modulexml-file){target="_blank"} pour plus d’informations.\
>REMARQUE : pour que cet exemple de code fonctionne, vous devez inclure setup_version, sinon InstallData.php ne s&#39;exécute pas.



```xml
<?xml version="1.0"?>
<config xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Learning_ClothingMaterial" setup_version="0.0.1"/>
</config>
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Backend/Material.php

>[!NOTE]
>
>Veillez à utiliser l’identifiant de jeu d’attributs qui se trouve dans votre projet. Dans cet exemple, il s’agit du chiffre 9.

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Backend;

use Magento\Catalog\Model\Product;
use Magento\Eav\Model\Entity\Attribute\Backend\AbstractBackend;
use Magento\Framework\Exception\LocalizedException;

class Material extends AbstractBackend
{
    /**
     * @param Product @object
     * @throws LocalizedException
     */
    public function validate($object)
    {
        $value =$object->getData($this->getAttribute()->getAttributeCode());
        // Be sure to validate that your ID number it is likely to be different

        if (($object->getAttributeSetId() == 9) && ($value == 'fur')) {
            throw new LocalizedException(__('Bottoms cannot be fur'));
        }
    }
}
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Frontend/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Frontend;

use Magento\Eav\Model\Entity\Attribute\Frontend\AbstractFrontend;
use Magento\Framework\DataObject;

class Material extends AbstractFrontend
{

    public function getValue(DataObject $object): string
    {
        $value = $object->getData($this->getAttribute()->getAttributeCode());
        return "<b>$value</b>";
    }

}
```

### app/code/Learning/ClothingMaterial/Model/Attribute/Source/Material.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Model\Attribute\Source;

use Magento\Eav\Model\Entity\Attribute\Source\AbstractSource;

class Material extends AbstractSource
{
    /**
     * Get all options
     *
     * @retrun array
     */
    public function getAllOptions(): array
    {
       if(!$this->_options){
           $this->_options = [
               ['label'    => __('Cotton'),     'value' => 'cotton'],
               ['label'    => __('Leather'),    'value' => 'leather'],
               ['label'    => __('Silk'),       'value' => 'silk'],
               ['label'    => __('Fur'),        'value' => 'fur'],
               ['label'    => __('Wool'),       'value' => 'wool'],
           ];
       }
       return $this->_options;
    }
}
```

### app/code/Learning/ClothingMaterial/Setup/InstallData.php

```php
<?php
declare(strict_types=1);

namespace Learning\ClothingMaterial\Setup;

use Learning\ClothingMaterial\Model\Attribute\Frontend\Material as Frontend;
use Learning\ClothingMaterial\Model\Attribute\Source\Material as Source;
use Learning\ClothingMaterial\Model\Attribute\Backend\Material as Backend;
use Magento\Catalog\Model\Product;
use Magento\Eav\Model\Entity\Attribute\ScopedAttributeInterface;
use Magento\Framework\Setup\InstallDataInterface;
use Magento\Framework\Setup\ModuleContextInterface;
use Magento\Framework\Setup\ModuleDataSetupInterface;

class InstallData implements InstallDataInterface
{
    protected $eavSetupFactory;

    public function __construct(
        \Magento\Eav\Setup\EavSetupFactory $eavSetupFactory
    )
    {
        $this->eavSetupFactory = $eavSetupFactory;
    }

    /**
     * @param ModuleDataSetupInterface $setup
     * @param ModuleContextInterface $context
     * {@inheritDoc}
     *
     * @SuppressWarnings(PHPMD.CyclomaticComplexity)
     * @suppressWarnings(PHPMD.ExessiveMethodLength)
     * @SuppressWarnings(PHPMD.NPathComplexity)
     */
    public function install(ModuleDataSetupInterface $setup, ModuleContextInterface $context)
    {
        $eavSetup = $this->eavSetupFactory->create();
        $eavSetup->addAttribute(
            Product::ENTITY,
            'clothing_material',
            [
                'group'         => 'Product Details',
                'type'          => 'varchar',
                'label'         => 'Clothing Material',
                'input'         => 'select',
                'source'        => Source::class,
                'frontend'      => Frontend::class,
                'backend'       => Backend::class,
                'required'      => false,
                'sort_order'    => 50,
                'global'        => ScopedAttributeInterface::SCOPE_GLOBAL,
                'is_used_in_grid'               => false,
                'is_visible_in_grid'            => false,
                'is_filterable_in_grid'         => false,
                'visible'                       => true,
                'is_html_allowed_on_frontend'   => true,
                'visible_on_front'              => true,
            ]
        );
    }
}
```

## Ressources utiles

[Ajout d’un attribut de champ de texte personnalisé](https://developer.adobe.com/commerce/php/tutorials/admin/custom-text-field-attribute/)
