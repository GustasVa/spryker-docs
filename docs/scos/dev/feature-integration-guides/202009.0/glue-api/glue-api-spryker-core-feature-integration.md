---
title: Glue API - Spryker core feature integration
description: Use the guide to install the Spryker Core feature in your project.
last_updated: Apr 8, 2021
template: feature-integration-guide-template
originalLink: https://documentation.spryker.com/v6/docs/glue-api-spryker-core-feature-integration
originalArticleId: 99095f0f-09cd-4d48-8c4f-ed2c0fa742a7
redirect_from:
  - /v6/docs/glue-api-spryker-core-feature-integration
  - /v6/docs/en/glue-api-spryker-core-feature-integration
---

## Install Feature API
### Prerequisites
To start feature integration, overview and install the necessary features:

| Name | Type | Version |
| --- | --- | --- |
| Spryker Core | Feature | {{page.version}} |

### 1) Install the required modules using Composer
Run the following command(s) to install the required modules:

```bash
composer require spryker/glue-application:"^1.0.0" spryker/entity-tags-rest-api:"^1.0.0" spryker/stores-rest-api:"^1.0.0" spryker/urls-rest-api:"^1.0.0" --update-with-dependencies
```

{% info_block warningBox "Verification" %}
Make sure that the following modules have been installed:

|Module|Expected Directory|
|--- |--- |
|`GlueApplication`|`vendor/spryker/glue-application`|
|`EntityTagsRestApi`|`vendor/spryker/entity-tag-rest-api`|
|`StoresRestApi`|`vendor/spryker/stores-rest-api`|
|`UrlsRestApi`|`vendor/spryker/urls-rest-api`|
|`SecurityBlockerRestApi`|`vendor/spryker/security-blocker-rest-api`|

{% endinfo_block %}

### 2) Set Up Configuration
Add the necessary parameters to `config/Shared/config_default.php`:

```php
$config[GlueApplicationConstants::GLUE_APPLICATION_DOMAIN] = 'https://glue.mysprykershop.com';
$config[GlueApplicationConstants::GLUE_APPLICATION_CORS_ALLOW_ORIGIN] = 'https://glue.mysprykershop.com';
$config[GlueApplicationConstants::GLUE_APPLICATION_REST_DEBUG] = false;
```

#### Add Global CORS policy

{% info_block infoBox "Info" %}
`GLUE_APPLICATION_CORS_ALLOW_ORIGIN` should be configured for every domain used in the project. 
{% endinfo_block %}

Adjust `config/Shared/config_default.php`:

```php
$config[GlueApplicationConstants::GLUE_APPLICATION_CORS_ALLOW_ORIGIN] = 'https://glue.mysprykershop.com';
```

#### Allow CORS requests to any domain
Adjust `config/Shared/config_default.php`:

```php
$config[GlueApplicationConstants::GLUE_APPLICATION_CORS_ALLOW_ORIGIN] = '*';
```

{% info_block warningBox "Verification" %}

To make sure that the CORS headers are set up correctly, send the OPTIONS request to any valid GLUE resource with the **Origin** header `https://glue.mysprykershop.com/` and see the correct JSON response:
- Verify that the **access-control-allow-origin** header is present and is the same to the one set in `config`
- Verify that the **access-control-allow-methods** header is present and contains all available methods
- Send POST, PATCH or DELETE requests (can choose any of available ones and verify that the response headers are the same)

{% endinfo_block %}

#### Configure included section

{% info_block infoBox "Info" %}
- When the `GlueApplicationConfig::isEagerRelationshipsLoadingEnabled()` option is set to "false", no relationship will be loaded unless they are explicitly specified in the "included" query parameter (for example, `/abstract-products?include=abstract-product-prices`).
- When the `GlueApplicationConfig::isEagerRelationshipsLoadingEnabled()` option is set to "true", all resource relationships will be loaded by default unless you pass an empty "include" query parameter (for example, `/abstract-products?include=`). If you specify needed relationships in the "included" query parameter, only required relationships will be added to response data.)

{% endinfo_block %}
    
### 3) Set Up Transfer Objects
Run the following command to generate transfer objects:
    
```bash
console transfer:generate
```
    
{% info_block warningBox "Verification" %}

Make sure that the following changes have occurred:

|Transfer|Type|Event|Path|
|--- |--- |--- |--- |
|`RestPageOffsetsTransfer`|class|created|`src/Generated/Shared/Transfer/RestPageOffsetsTransfer.php`|
|`RestErrorMessageTransfer`|class|created|`src/Generated/Shared/Transfer/RestErrorMessageTransfer.php`|
|`RestErrorCollectionTransfer`|class|created|`src/Generated/Shared/Transfer/RestErrorCollectionTransfer.php`|
|`RestVersionTransfer`|class|created|`src/Generated/Shared/Transfer/RestVersionTransfer.php`|
|`RestUserTransfer`|class|created|`src/Generated/Shared/Transfer/RestUserTransfer.php`|
|`StoresRestAttributesTransfer`|class|created|`src/Generated/Shared/Transfer/StoresRestAttributesTransfer.php`|
|`StoreCountryRestAttributesTransfer`|class|created|`src/Generated/Shared/Transfer/StoreCountryRestAttributesTransfer.php`|
|`StoreRegionRestAttributesTransfer`|class|created|`src/Generated/Shared/Transfer/StoreRegionRestAttributesTransfer.php`|
|`StoreLocaleRestAttributesTransfer`|class|created|`src/Generated/Shared/Transfer/StoreLocaleRestAttributesTransfer.php`|
|`StoreCurrencyRestAttributesTransfer`|class|created|`src/Generated/Shared/Transfer/StoreCurrencyRestAttributesTransfer.php`|
|`RestUrlResolverAttributesTransfer`|class|created|`src/Generated/Shared/Transfer/RestUrlResolverAttributesTransfer.php`|

{% endinfo_block %}
    
### 4) Set Up Behavior
Activate the following plugins:

| Plugin | Specification | Prerequisites | Namespace |
| --- | --- | --- | --- |
| `GlueResourceBuilderService` | Registers the resource builder service in Glue Application. | None | `Spryker\Glue\GlueApplication\Plugin\Rest\ServiceProvider` |
| `GlueApplicationServiceProvider` | Registers the pimple plugin, the controller resolver and configures the debug mode in Glue Application. | None | `Spryker\Glue\GlueApplication\Plugin\Rest\ServiceProvider` |
| `SessionApplicationPlugin` | Registers the session services in Glue Application. | None | `Spryker\Glue\Session\Plugin\Application` |
| `GlueServiceProviderPlugin` | Registers the `onKernelController` event listeners in Glue Application. | None | `Spryker\Glue\GlueApplication\Plugin\Rest` |
| `GlueRoutingServiceProvider` | Registers the URL matcher and router services in Glue Application. | None | `Spryker\Glue\GlueApplication\Plugin\Rest\ServiceProvider` |
| `SetStoreCurrentLocaleBeforeActionPlugin` | Sets a locale for the whole current store. | None | `Spryker\Glue\GlueApplication\Plugin\Rest\SetStoreCurrentLocaleBeforeActionPlugin` |
| `EntityTagFormatResponseHeadersPlugin` | Adds the ETag header to the response if applicable. | None | `Spryker\Glue\EntityTagsRestApi\Plugin\GlueApplication\EntityTagFormatResponseHeadersPlugin` |
| `EntityTagRestRequestValidatorPlugin` | Verifies that the `If-Match` header is equal to the entity tag. | None | `Spryker\Glue\EntityTagsRestApi\Plugin\GlueApplication\EntityTagRestRequestValidatorPlugin` |
| `StoresResourceRoutePlugin` | Registers the stores resource. | None | `Spryker\Glue\StoresRestApi\Plugin` |
| `UrlResolverResourceRoutePlugin` | Registers the url-resolver resource. | None | `Spryker\Glue\UrlsRestApi\Plugin\GlueApplication\UrlResolverResourceRoutePlugin` |
| `ProductAbstractRestUrlResolverAttributesTransferProviderPlugin` | Provides the abstract-products resource from the `UrlStorageTransfer` object. | None | `Spryker\Glue\ProductsRestApi\Plugin\UrlsRestApi\ProductAbstractRestUrlResolverAttributesTransferProviderPlugin` |
| `CategoryNodeRestUrlResolverAttributesTransferProviderPlugin`| Provides the category-nodes resource from the `UrlStorageTransfer` object. | None | `Spryker\Glue\CategoriesRestApi\Plugin\UrlsRestApi\CategoryNodeRestUrlResolverAttributesTransferProviderPlugin`|
    
Create a new entry point for Glue Application:

**public/Glue/index.php**

```php
<?php
 
use Pyz\Glue\GlueApplication\Bootstrap\GlueBootstrap;
use Spryker\Shared\Config\Application\Environment;
use Spryker\Shared\ErrorHandler\ErrorHandlerEnvironment;
 
define('APPLICATION', 'GLUE');
defined('APPLICATION_ROOT_DIR') || define('APPLICATION_ROOT_DIR', realpath(__DIR__ . '/../..'));
 
require_once APPLICATION_ROOT_DIR . '/vendor/autoload.php';
 
Environment::initialize();
 
$errorHandlerEnvironment = new ErrorHandlerEnvironment();
$errorHandlerEnvironment->initialize();
 
$bootstrap = new GlueBootstrap();
$bootstrap
    ->boot()
    ->run();
```

#### Configure web server
Create Nginx VHOST configuration:

**/etc/nginx/sites-enabled/DE_development_glue**

```
server {
    # Listener for production/staging - requires external LoadBalancer directing traffic to this port
    listen 10001;
 
    # Listener for testing/development - one host only, doesn't require external LoadBalancer
    listen 80;
 
    server_name ~^glue\\..+\\.com$;
 
    keepalive_timeout 0;
    access_log  /data/logs/development/glue-access.log extended;
 
    # entry point for Glue Application
    root /data/shop/development/current/public/Glue;
 
    set $application_env development;
    # Binding store
    set $application_store DE;
    include "spryker/zed.conf";
}
```

Update hosts configuration by adding the following line (replace **ip** with your server's IP address):

**/etc/hosts**

```
ip glue.mysprykershop.com
```

{% info_block warningBox "Verification" %}
If everything is set up correctly, you should be able to access `https://glue.mysprykershop.com` and get a correct JSON response as follows:
{% endinfo_block %}

**Default JSON Response**

```json
{
    "errors": [
        {
            "status": 404,
            "detail": "Not Found"
        }
    ]
}
```

\Pyz\Glue\GlueApplication\GlueApplicationDependencyProvider.php

```php
<?php
 
namespace Pyz\Glue\GlueApplication;
 
use Spryker\Glue\EventDispatcher\Plugin\Application\EventDispatcherApplicationPlugin;
use Spryker\Glue\GlueApplication\GlueApplicationDependencyProvider as SprykerGlueApplicationDependencyProvider;
use Spryker\Glue\GlueApplication\Plugin\Application\GlueApplicationApplicationPlugin;
use Spryker\Glue\GlueApplication\Plugin\Rest\SetStoreCurrentLocaleBeforeActionPlugin;
use Spryker\Glue\Router\Plugin\Application\RouterApplicationPlugin;
use Spryker\Glue\Session\Plugin\Application\SessionApplicationPlugin;
use Spryker\Glue\StoresRestApi\Plugin\StoresResourceRoutePlugin;
use Spryker\Glue\UrlsRestApi\Plugin\GlueApplication\UrlsResourceRoutePlugin;

class GlueApplicationDependencyProvider extends SprykerGlueApplicationDependencyProvider
{
    /**
     * @return \Spryker\Glue\GlueApplicationExtension\Dependency\Plugin\ResourceRoutePluginInterface[]
     */
    protected function getResourceRoutePlugins(): array
    {
        return [
            new StoresResourceRoutePlugin(),
            new UrlResolverResourceRoutePlugin(),
        ];
    }
 
    /**
     * @return \Spryker\Glue\GlueApplicationExtension\Dependency\Plugin\ControllerBeforeActionPluginInterface[]
     */
    protected function getControllerBeforeActionPlugins(): array
    {
        return [
            new SetStoreCurrentLocaleBeforeActionPlugin(),
        ];
    }
     
    /**
     * @return \Spryker\Glue\GlueApplicationExtension\Dependency\Plugin\FormatResponseHeadersPluginInterface[]
     */
    protected function getFormatResponseHeadersPlugins(): array
    {
        return [
            new EntityTagFormatResponseHeadersPlugin(),
        ];
    }
 
    /**
     * @return \Spryker\Glue\GlueApplicationExtension\Dependency\Plugin\RestRequestValidatorPluginInterface[]
     */
    protected function getRestRequestValidatorPlugins(): array
    {
        return [
            new EntityTagRestRequestValidatorPlugin(),
        ];
    }
    
    /**
     * @return \Spryker\Shared\ApplicationExtension\Dependency\Plugin\ApplicationPluginInterface[]
     */
    protected function getApplicationPlugins(): array
    {
        return [
            new SessionApplicationPlugin(),
            new EventDispatcherApplicationPlugin(),
            new GlueApplicationApplicationPlugin(),
            new RouterApplicationPlugin(),
        ];
    }
}
```

\Pyz\Glue\UrlsRestApi\UrlsRestApiDependencyProvider.php

```php
<?php
 
namespace Pyz\Glue\UrlsRestApi;
 
use Spryker\Glue\CategoriesRestApi\Plugin\UrlsRestApi\CategoryNodeResourceIdentifierProviderPlugin;
use Spryker\Glue\ProductsRestApi\Plugin\UrlsRestApi\ProductAbstractResourceIdentifierProviderPlugin;
use Spryker\Glue\UrlsRestApi\UrlsRestApiDependencyProvider as SprykerUrlsRestApiDependencyProvider;
 
class UrlsRestApiDependencyProvider extends SprykerUrlsRestApiDependencyProvider
{
    /**
     * @return \Spryker\Glue\UrlsRestApiExtension\Dependency\Plugin\ResourceIdentifierProviderPluginInterface[]
     */
    protected function getResourceIdentifierProviderPlugins(): array
    {
        return [
            new ProductAbstractRestUrlResolverAttributesTransferProviderPlugin(),
            new CategoryNodeRestUrlResolverAttributesTransferProviderPlugin(),
        ];
    }
}
```

src/Pyz/Glue/EntityTagsRestApi/EntityTagsRestApiConfig.php

```php
<?php
 
namespace Pyz\Glue\EntityTagsRestApi;
 
use Spryker\Glue\CartsRestApi\CartsRestApiConfig;
use Spryker\Glue\EntityTagsRestApi\EntityTagsRestApiConfig as SprykerEntityTagsRestApiConfig;
 
 class EntityTagsRestApiConfig extends SprykerEntityTagsRestApiConfig
{
    /**
     * @return string[]
     */
    public function getEntityTagRequiredResources(): array
    {
        return array_merge(
            parent::getEntityTagRequiredResources(),
            [PyzRestApiConfig::RESOURCE_NAME]
        );
    }
}
```

{% info_block warningBox "Verification" %}
If everything is set up correctly, a request to `https://glue.mysprykershop.com` with the header `[{"key":"Accept-Language","value":"de_DE, de;q=0.9"}]` should result in a response that contains the **content-language** header set to **de_DE**.
{% endinfo_block %}

{% info_block warningBox "Verification" %}
Send a GET request to `https://glue.mysprykershop.com/{% raw %}{{{% endraw %}RESOURCE_NAME{% raw %}}}{% endraw %}/{% raw %}{{{% endraw %}identifier{% raw %}}}{% endraw %} `.<br>Make sure that the response contains the 'ETag' header.<br>Prepare a PATCH request to  `https://glue.mysprykershop.com/{% raw %}{{{% endraw %}RESOURCE_NAME{% raw %}}}{% endraw %}/{% raw %}{{{% endraw %}identitifer{% raw %}}}{% endraw %}`<br>Add the 'If-Match' header with the value of ETag from a GET response header.<br>Add a request body.
{% endinfo_block %}

**Request body**

```json
{
    "data": {
        "type": "RESOURCE_NAME",
        "attributes": {
            "name": "{% raw %}{{{% endraw %}new_name{% raw %}}}{% endraw %}"
        }
    }
}
```

{% info_block warningBox "Verification" %}
Send a request with the specified header and body.<br>Make sure that the returned resource contains the updated 'ETag'.
{% endinfo_block %}

{% info_block warningBox "Verification" %}
Send a GET request to `https://glue.mysprykershop.com/{% raw %}{{{% endraw %}RESOURCE_NAME{% raw %}}}{% endraw %}/{% raw %}{{{% endraw %}identifier{% raw %}}}{% endraw %} `.<br>Make sure that the response contains the 'ETag' header.<br>Prepare a PATCH request to  `https://glue.mysprykershop.com/{% raw %}{{{% endraw %}RESOURCE_NAME{% raw %}}}{% endraw %}/{% raw %}{{{% endraw %}identifier{% raw %}}}{% endraw %}`<br>Add the 'If-Match' header with some random value.<br>Add a request body.
{% endinfo_block %}

**Request body**

```json
{
    "data": {
        "type": "RESOURCE_NAME",
        "attributes": {
            "name": "{% raw %}{{{% endraw %}new_name{% raw %}}}{% endraw %}"
        }
    }
}
```
{% info_block warningBox "Verification" %}
Send a request with the specified header and body.<br>Make sure that the response contains the ETag validation error.
{% endinfo_block %}

{% info_block warningBox "Verification" %}
Make sure that the following endpoint is available:<ul><li>`https://glue.mysprykershop.com/stores`</li></ul>
{% endinfo_block %}

{% info_block warningBox "Verification" %}
To make sure that the `ProductAbstractRestUrlResolverAttributesTransferProviderPlugin` plugin is set up correctly, request the `abstract-products` URL via the `/urls` API endpoint and make sure that you receive the correct resource identifier in the response.

**Request body**

```json
https://glue.mysprykershop.com/url-resolver/?url=/product-abstract-url
{
    "data": [
        {
            "type": "url-resolver",
            "id": null,
            "attributes": {
                "entityType": "abstract-products",
                "entityId": "134"
            },
            "links": {
                "self": "https://glue.mysprykershop.com/url-resolver?url=/de/acer-aspire-s7-134"
            }
        }
    ],
    "links": {
        "self": "https://glue.mysprykershop.com/url-resolver?url=/de/acer-aspire-s7-134"
    }
}
```
{% endinfo_block %}

{% info_block warningBox "Verification" %}
To make sure that the `CategoryNodeRestUrlResolverAttributesTransferProviderPlugin` plugin is set up correctly, request the `category` URL via the `/urls` API endpoint and make sure that you receive the correct resource identifier in the response.

**Request body**

```json
https://glue.mysprykershop.com/url-resolver/?url=/category-url
{
    "data": [
        {
            "type": "url-resolver",
            "id": null,
            "attributes": {
                "entityType": "category-nodes",
                "entityId": "5"
            },
            "links": {
                "self": "https://glue.mysprykershop.com/url-resolver?url=/de/computer"
            }
        }
    ],
    "links": {
        "self": "https://glue.mysprykershop.com/url-resolver?url=/de/computer"
    }
}
```

{% endinfo_block %}

{% info_block warningBox "Verification" %}

Make sure `SecurityBlockerCustomerControllerAfterActionPlugin` and `SecurityBlockerCustomerRestRequestValidatorPlugin` are activated correctly by attempting to get an access token (see [Authenticating as a customer](/docs/scos/dev/glue-api-guides/{{page.version}}/managing-customers/authenticating-as-a-customer.html)) with the wrong credentials as a customer. After making the number of attempts you specified in `SecurityBlockerConstants::SECURITY_BLOCKER_BLOCKING_NUMBER_OF_ATTEMPTS`, the account should be blocked for `SecurityBlockerConstants::SECURITY_BLOCKER_BLOCK_FOR` seconds. Check that with the consequent login attempts, you get the `429 Too many requests` error.

Repeat the same actions for the agent sign-in to check `SecurityBlockerAgentRestRequestValidatorPlugin` and `SecurityBlockerAgentControllerAfterActionPlugin`. The agent should get the blocking configuration specific for agents if you specified the agent-specific settings in step #3 of the integration of the feature core. See [Authenticating as an agent assist](/docs/scos/dev/glue-api-guides/{{page.version}}/managing-agent-assists/authenticating-as-an-agent-assist.html) for agent access tokens manual.

{% endinfo_block %}
