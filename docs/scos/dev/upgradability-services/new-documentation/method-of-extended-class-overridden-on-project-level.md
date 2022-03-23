---
title: Method of extended class is overridden one the project level
description: Reference information for evaluator and upgrader tools.
last_updated: Mar 23, 2022
template: concept-topic-template
---

## Method of an extended class is overridden on the project level

Factory, Dependency Provider, Repository, and Entity Manager methods belong to the private API.
If you extend a core class and override one of its methods, minor releases can cause errors or unexpected changes in functionality.

#### Using custom methods on the project level

To avoid unexpected issues and achieve the same result, instead of overriding the core methods, introduce custom ones.

#### Example of code that can cause upgradability errors

For example, the extended class EvaluatorCategoryImageEntityManager overrides the core method CategoryImageEntityManager.

```php
namespace Pyz\Zed\Evaluator\Persistence;

use Generated\Shared\Transfer\CategoryImageSetTransfer;
use Spryker\Zed\CategoryImage\Persistence\CategoryImageEntityManager;

class EvaluatorCategoryImageEntityManager extends CategoryImageEntityManager
{
    /**
     * @param \Generated\Shared\Transfer\CategoryImageSetTransfer $categoryImageSetTransfer
     *
     * @return \Generated\Shared\Transfer\CategoryImageSetTransfer
     */
    public function saveCategoryImageSet(CategoryImageSetTransfer $categoryImageSetTransfer): CategoryImageSetTransfer
    {
        ...
    }
}
```

#### Example of related error in the Evaluator output

```bash
------------------------------------------------------------------------------------
************************************************************************************************************************
Evaluator\Business\Check\IsMethodOverridden\EntityManagerCheck
Introduce a new custom method without usage of existing one. Override usage of the current method in all usage of public API.
************************************************************************************************************************
------------------------------------------------------------------------------------
Pyz\Zed\EvaluatorSpryker\Persistence\EvaluatorSprykerCategoryImageEntityManager
{"name":"saveCategoryImageSet","class":"Pyz\\Zed\\EvaluatorSpryker\\Persistence\\EvaluatorSprykerCategoryImageEntityManager"}
{"parentClass":"Spryker\\Zed\\CategoryImage\\Persistence\\CategoryImageEntityManager"}
************************************************************************************************************************
```

{% info_block warningBox "Dependency Provider exception" %}

If you override a method and initialize a plugin, it does not break backward compatibility. For example, in `StorageRouterDependencyProvider`, `SprykerShopStorageRouterDependencyProvider` is overridden with a plugin introduced:

<details>
<summary markdown='span'>Example of an overridden method with a plugin</summary>

```php
<?php

namespace Pyz\Yves\StorageRouter;

use SprykerShop\Yves\CatalogPage\Plugin\StorageRouter\CatalogPageResourceCreatorPlugin;
use SprykerShop\Yves\CmsPage\Plugin\StorageRouter\PageResourceCreatorPlugin;
use SprykerShop\Yves\ProductDetailPage\Plugin\StorageRouter\ProductDetailPageResourceCreatorPlugin;
use SprykerShop\Yves\ProductSetDetailPage\Plugin\StorageRouter\ProductSetDetailPageResourceCreatorPlugin;
use SprykerShop\Yves\RedirectPage\Plugin\StorageRouter\RedirectResourceCreatorPlugin;
use SprykerShop\Yves\StorageRouter\StorageRouterDependencyProvider as SprykerShopStorageRouterDependencyProvider;

class StorageRouterDependencyProvider extends SprykerShopStorageRouterDependencyProvider
{
    /**
     * @return \SprykerShop\Yves\StorageRouterExtension\Dependency\Plugin\ResourceCreatorPluginInterface[]
     */
    protected function getResourceCreatorPlugins(): array
    {
        return [
            new PageResourceCreatorPlugin(),
            new CatalogPageResourceCreatorPlugin(),
            new ProductDetailPageResourceCreatorPlugin(),
            new ProductSetDetailPageResourceCreatorPlugin(),
            new RedirectResourceCreatorPlugin(),
        ];
    }
}
```

</details>

{% endinfo_block %}

#### Steps to solve potential problem in functionality

1. Introduce a new custom method.
2. Replace the core method with the custom one you've created in the previous step.

{% info_block infoBox "Unique method names" %}

The method name should be unique to the extent of making it impossible to accidentally match the name of a core method introduced in the future.

{% endinfo_block %}

When the core method is overridden with a custom one, re-evaluate the code. The same error shouldn't be returned.

#### Example of using a custom method on the project level

At this particular example was created new method that almost the same as the core one, but it has unique prefix that base on the default name of current shop (Pyz). 
This new method should be used instead core's one.

```php
namespace Pyz\Zed\Evaluator\Persistence;

use Generated\Shared\Transfer\CategoryImageSetTransfer;
use Spryker\Zed\CategoryImage\Persistence\CategoryImageEntityManager;

class EvaluatorCategoryImageEntityManager extends CategoryImageEntityManager
{
    /**
     * @param \Generated\Shared\Transfer\CategoryImageSetTransfer $categoryImageSetTransfer
     *
     * @return \Generated\Shared\Transfer\CategoryImageSetTransfer
     */
    public function savePyzCategoryImageSet(CategoryImageSetTransfer $categoryImageSetTransfer): CategoryImageSetTransfer
    {
        ...
    }
}
```
---

