# How to add or change toolbar buttons in Magento2 admin area ?

## Default way - (order create page example)

This technique will work on any block class extending `\Magento\Backend\Block\Widget\Container` where `addButton`, `removeButton` and `updateButton` methods are defined.

### Step 1
Declare the plugin, in `Your/Module/etc/adminhtml/di.xml`, for block class of admin page where the toolbar button customization should occur.
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <type name="Magento\Sales\Block\Adminhtml\Order\Create">
        <plugin name="create_order_customize_toolbar_buttons" type="\Your\Module\Plugin\Block\Adminhtml\OrderCreatePlugin" sortOrder="1" disabled="false" />
    </type>
</config>
```


### Step 2
Define customization code in `beforeSetLayout` method inside the plugin class `Your/Module/Plugin\Block\Adminhtml\OrderCreatePlugin.php`.
```php
<?php
namespace Your\Module\Plugin\Block\Adminhtml;

class OrderCreatePlugin
{    
    public function beforeSetLayout(\Magento\Sales\Block\Adminhtml\Order\Create $subject, \Magento\Framework\View\LayoutInterface $layout)
    {
      $subject->addButton(....);
      ...
      $subject->removeButton(....);
      ...
      $subject->updateButton(....);

      return null; 
    }
}
```

## UI Component way - (product edit page example)

This technique will work on pages implementing UI Form component and constructing the toolbar using the  `Magento\Framework\View\Element\UiComponent\Control\ButtonProviderInterface`.
Removing or updating buttons can be achieved with plugin for public `getButtonData` method.

### Step 1
Add custom button configuration in `Your/Module/view/adminhtml/ui_component/product_form.xml`
```xml
<?xml version="1.0"?>
<form xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:module:Magento_Ui:etc/ui_configuration.xsd">
    <argument name="data" xsi:type="array">
        <item name="buttons" xsi:type="array">
            <item name="custom_button" xsi:type="string">Your\Module\Block\Adminhtml\Product\Edit\Button\Custom</item>
        </item>
    </argument>
</form>
```

### Step 2
Add Block class for custom button in `Your\Module\Block\Adminhtml\Product\Edit\Button\Custom`
```php
<?php
namespace Your\Module\Block\Adminhtml\Product\Edit\Button;

use Magento\Catalog\Block\Adminhtml\Product\Edit\Button\Generic;

class Custom extends Generic
{    
    public function getButtonData()
    {
        return [
            'label' => __('Custom Button'),
            'on_click' => "alert('Can be left out and options can be provided for multiple actions under dropdown')",
            'class' => 'action-secondary',
            'sort_order' => 100,
            'data_attribute' => [
                'mage-init' => []
            ],
            'options' => []
        ];
    }
}
```
