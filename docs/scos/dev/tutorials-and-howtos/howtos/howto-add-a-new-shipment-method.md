---
title: HowTo - Add a new shipment method
last_updated: Jun 16, 2021
template: howto-guide-template
originalLink: https://documentation.spryker.com/2021080/docs/ht-add-new-shipment-method
originalArticleId: acc1e4a3-0d2e-459e-b57b-8a2eb20e4fd8
redirect_from:
  - /2021080/docs/ht-add-new-shipment-method
  - /2021080/docs/en/ht-add-new-shipment-method
  - /docs/ht-add-new-shipment-method
  - /docs/en/ht-add-new-shipment-method
  - /v6/docs/ht-add-new-shipment-method
  - /v6/docs/en/ht-add-new-shipment-method
  - /v5/docs/ht-add-new-shipment-method
  - /v5/docs/en/ht-add-new-shipment-method
  - /v4/docs/ht-add-new-shipment-method
  - /v4/docs/en/ht-add-new-shipment-method
  - /v3/docs/ht-add-new-shipment-method
  - /v3/docs/en/ht-add-new-shipment-method
  - /v2/docs/ht-add-new-shipment-method
  - /v2/docs/en/ht-add-new-shipment-method
  - /v1/docs/ht-add-new-shipment-method
  - /v1/docs/en/ht-add-new-shipment-method
related:
  - title: "Reference information: Shipment method plugins"
    link: docs/scos/dev/feature-walkthroughs/page.version/shipment-feature-walkthrough/reference-information-shipment-method-plugins.html
---

{% info_block infoBox %}

This article describes the steps to add a new shipment method, without integrating with the shipment provider.

{% endinfo_block %}

In this tutorial we’ll consider the case when you need to add a new shipment method, without the need to integrate it with the shipment providers system.

What’s important for this situation is to have a price attached to the shipment method and also to have the correct tax set linked to it. Also, the ship event should be manually triggerable from the Zed Admin UI.

## Set up the state machine

The state machine that handles orders that use this shipment method needs to use a manual event for shipping, so that it can be triggered from the Zed Admin UI.

<!--../../Resources/Images/ship_event.png -->

The corresponding XML for this transition would be:

```xml
<states>
   <state name="exported" reserved="true"/>
   <state name="shipped" reserved="true"/>
//..
</states>
<transitions>
    <transition happy="true">
        <source>exported</source>
        <target>shipped</target>
        <event>ship</event>
    </transition>
//..
</transitions>
<events>
    <event name="ship" manual="true"/>
//..
</events>
```

## Add a new shipment method

To add a new shipment method:

1. In the Zed Admin UI, navigate to the Shipment section and click **Add new Carrier Company**.
2. Specify a name for the carrier company and the corresponding glossary key for having a localized name.
3. To use this carrier company in the shop, select **Enabled** in the check-box.
<!-- ../../Resources/Images/ui_add_carrier_cmpany.png-->

Now that we have a new shipment carrier, we can add a new shipment method to it.

To add a new shipment method to a carrier:

1. Click **Add new Shipment Method**.
   The **Add a new shipment method** page opens.
2. Select the carrier you created in the previous step.
3. Add the name and default price.
4. Mark it as `Active`.
5. Select the corresponding tax set.
6. Click **Add Shipment Method**.
<!-- ../../Resources/Images/ui_shipment_method_6.png -->

Now the new shipment method is available in the shop.
<!-- ../../Resources/Images/ui_shipment_selection.png -->
