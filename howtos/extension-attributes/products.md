# Custom extension attribute for catalog product entity.

1. Database table schema for custom extension attribute in `Your/Module/Setup/InstallSchema.php`. **[sample code](#InstallSchema)**
2. ACL resource for custom extension attribute in `Your/Module/etc/acl.xml`. **[sample code](#acl)**
3. Extension attribute definition in `Your/Module/etc/extension_attributes.xml`. **[sample code](#extension_attributes)**
4. Extension attribute intefrace in `Your\Module\Api\Data\CustomItemInterface.php`. **[sample code](#CustomItemInterface)**


<a name="InstallSchema"></a>
```php
<?php

namespace Your\Module\Setup;

use Magento\Framework\Setup\InstallSchemaInterface;
use Magento\Framework\Setup\ModuleContextInterface;
use Magento\Framework\Setup\SchemaSetupInterface;
use Magento\Framework\DB\Ddl\Table;

class InstallSchema implements InstallSchemaInterface
{
    public function install(SchemaSetupInterface $setup, ModuleContextInterface $context)
    {
        $setup->startSetup();

        $table = $setup->getConnection()
            ->newTable($setup->getTable('custom_item'))
            ->addColumn(
                'item_id',
                Table::TYPE_SMALLINT,
                null,
                ['identity' => true, 'nullable' => false, 'primary' => true],
                'Identifier'
            )
            ->addColumn(
                'product_id',
                Table::TYPE_INTEGER,
                null,
                ['unsigned' => true, 'nullable' => false, 'default' => '0'],
                'Product Id'
            )
            ->addColumn(
                'custom_attr',
                Table::TYPE_SMALLINT,
                5,
                ['unsigned' => true, 'nullable' => false, 'default' => '0'],
                'Some Custom Attribute Needed For Product Entity Functionality'
            )
            ->addIndex($setup->getIdxName('custom_item', ['product_id']), ['product_id'])
            ->addIndex($setup->getIdxName('custom_item', ['custom_attr']), ['custom_attr'])
            ->addIndex(
                $setup->getIdxName(
                    'custom_item',
                    ['product_id'],
                    \Magento\Framework\DB\Adapter\AdapterInterface::INDEX_TYPE_UNIQUE
                ),
                ['product_id'],
                ['type' => \Magento\Framework\DB\Adapter\AdapterInterface::INDEX_TYPE_UNIQUE]
            )->addForeignKey(
                $setup->getFkName(
                    'custom_item',
                    'product_id',
                    'catalog_product_entity',
                    'entity_id'
                ),
                'product_id',
                $setup->getTable('catalog_product_entity'),
                'entity_id',
                \Magento\Framework\DB\Ddl\Table::ACTION_CASCADE
            )->setComment('Table Holding Custom Product Attributes Grouped Under Custom Item Extension Atttribute');
      
        $setup->getConnection()->createTable($table);
     
        $setup->endSetup();
    }
}
```

<a name="acl"></a>
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Acl/etc/acl.xsd">
    <acl>
        <resources>
            <resource id="Magento_Backend::admin">
                <resource id="Magento_Backend::stores">
                    <resource id="Magento_Backend::stores_settings">
                        <resource id="Magento_Config::config">
                            <resource id="Your_Module::custom" title="Custom Product Extension Attribute" translate="title" />
                        </resource>
                    </resource>
                </resource>
            </resource>
        </resources>
    </acl>
</config>
```

<a name="extension_attributes"></a>
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Api/etc/extension_attributes.xsd">
    <extension_attributes for="Magento\Catalog\Api\Data\ProductInterface">
        <attribute code="custom_item" type="Your\Module\Api\Data\CustomItemInterface">
            <resources>
                <resource ref="Your_Module::custom"/>
            </resources>
        </attribute>
    </extension_attributes>
</config>
```

<a name="CustomItemInterface"></a>
```php
<?php

namespace Your\Module\Api\Data;

use Magento\Framework\Api\ExtensibleDataInterface;

interface CustomItemInterface extends ExtensibleDataInterface
{
    const ITEM_ID = 'item_id';
    const PRODUCT_ID = 'product_id';
    const CUSTOM_ATTR = 'custom_attr';
  
    /**
     * @return int|null
     */
    public function getItemId();

    /**
     * @param int $itemId
     * @return $this
     */
    public function setItemId($itemId);

    /**
     * @return int|null
     */
    public function getProductId();

    /**
     * @param int $productId
     * @return $this
     */
    public function setProductId($productId);
  
    /**
     * @return int|null
     */
    public function getCustomAttr();

    /**
     * @param int $customAttr
     * @return $this
     */
    public function setCustomAttr($customAttr);
  
    /**
     * Retrieve existing extension attributes object or create a new one.
     *
     * @return \Your\Module\Api\Data\CustomItemInterface|null
     */
    public function getExtensionAttributes();

    /**
     * Set an extension attributes object.
     *
     * @param \Your\Module\Api\Data\CustomItemInterface $extensionAttributes
     * @return $this
     */
    public function setExtensionAttributes(
        \Your\Module\Api\Data\CustomItemInterface $extensionAttributes
    );
}
```


