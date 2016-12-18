# How to add or change toolbar buttons in Magento2 admin area (order create page example) ?

This technique will work on any block class extending `\Magento\Backend\Block\Widget\Container` where `addButton`, `removeButton` and `updateButton` methods are defined.

## Step 1
Declare the plugin, in `Your/Module/etc/adminhtml/di.xml`, for block class of admin page where the toolbar button customization should occur.
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <type name="Magento\Sales\Block\Adminhtml\Order\Create">
        <plugin name="create_order_customize_toolbar_buttons" type="\Your\Module\Plugin\Block\Adminhtml\OrderCreatePlugin" sortOrder="1" disabled="false" />
    </type>
</config>
```


## Step 2
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
