---
title: Managing products
description: Use this guide to view product details, activate or update product attributes in the Back Office.
last_updated: Aug 10, 2021
template: back-office-user-guide-template
related:
  - title: Discontinuing Products
    link: docs/scos/user/back-office-user-guides/page.version/catalog/products/managing-products/discontinuing-products.html
  - title: Adding Product Alternatives
    link: docs/scos/user/back-office-user-guides/page.version/catalog/products/managing-products/adding-product-alternatives.html
---

This article describes how to manage abstract and concrete products.

To start managing products, go to **Catalog** > **Products**.

## Activating products

The abstract product is inactive until at least one product variant is activated. You should understand that there is no option to activate it.

To activate a product:
1. Navigate to the product variant of the product that you want to activate:
    **Edit** > **Variants** > **Edit product variant**
2.  In the top right corner of the *Edit Concrete Product* page, click **Activate**.
The product turns visible to the customers of your online store.

{% info_block infoBox "Note" %}

Each variant needs to be activated in order to be visible to your customers.

{% endinfo_block %}

**Tips & tricks**
<br>If at some point in time you want to hide the product variant from your customers, you just deactivate it using the same procedure described above. This deactivates only the product variant. The abstract product is active until at least one its variant is active.

## Viewing products

If you need to review the product details without actually editing them, do the following:
1. In the _Actions_ column of the abstract product you want to view, click **View**.
2. On the *View Product* page, you can navigate to the view product variant, initiate the editing flow for it, or manage its attributes.

**Tips & tricks**
<br>If you notice something you would like to change for your product, in the top right corner of the page. click **Edit**.

## Managing product attributes

When you see the **Manage Attributes** option, keep in mind that you manage the attributes like *brand* but not the super attributes. Such attributes like brand do not define the product variants differentiation, meaning they are not used while defining the concrete products of an abstract product. They rather go to the details section on the product details page in your online store. You can manage attributes for both abstract and concrete products.

{% info_block infoBox "Info" %}

The attributes that you add are taken from the **Products** > **Attributes** section of the Back Office. So the attribute you want to define should exist in that section.

{% endinfo_block %}

To manage the product attributes:
1.  In the _Actions_ column or in the top right corner of the *Edit* page, select the **Manage Attributes** option for the concrete or abstract product.
2. On the *Manage Attributes for Product* page, type the first three letters of the attribute key.
3. Select the suggested value and click **Add**.
4. In the _Attributes_ section, define the **Default** value for your attributes and specify the value for the **locales**.
    Repeat the procedure if needed.
5. Click **Save**.

See the _References_ section to see the examples of how the attributes look like.

## Approving and denying the new marketplace products

Once the product is created, it needs to be approved by the marketplace administrator.

Depending on the current status of the product, you can approve, deny or get the product in the *draft* status.
To update the approval status of the product, in the _Actions_ column of the abstract product do the following:

- For the product with status *Draft*:
  - click **Send for Approval** to initiate the approval process.
  - click **Approve** to approve the product.
  - click **Deny** to reject the product.

- For the product with status *Waiting for Approval*:
  - click **Back to Draft** to revert the product to *Draft* status.
  - click **Approve** to approve the product.
  - click **Deny** to reject the product.

- For the product with status *Approved*:
  - click **Back to Draft** to revert the product to *Draft* status.
  - click **Deny** to reject the product.

- For the product with status *Denied*:
  - click **Back to Draft** to revert the product to *Draft* status.
  - click **Approve** to approve the product.

**What's next?**
<br>Review the other articles in the _Products_ section to know more about product management. Also, review the _References_ section to learn more about the attributes you see, select, and enter on the product pages.
