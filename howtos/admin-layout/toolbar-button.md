# Admin Page Toolbar Buttons customization (order create page example).


1. Declare Plugin for any Block class extending `\Magento\Backend\Block\Widget\Container` in `Your/Module/etc/adminhtml/di.xml`. **[sample code](#di1)**
2. Define customization code in `beforeSetLayout` method inside the Plugin class `Your/Module/Plugin\Block\Adminhtml\OrderCreatePlugin.php`. **[sample code](#OrderCreatePlugin)**


<a name="di1"></a>
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:ObjectManager/etc/config.xsd">
    <type name="Magento\Sales\Block\Adminhtml\Order\Create">
        <plugin name="create_order_customize_toolbar_buttons" type="\Your\Module\Plugin\Block\Adminhtml\OrderCreatePlugin" sortOrder="1" disabled="false" />
    </type>
</config>
```

<a name="OrderCreatePlugin"></a>
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