---
title: REST API B2C Demo Shop reference
description: This page provides an exhaustive reference for the REST API endpoints present in the Spryker B2C demo Shop by default with the corresponding parameters and data formats.
last_updated: May 10, 2022
template: glue-api-storefront-guide-template
originalLink: https://documentation.spryker.com/2021080/docs/rest-api-reference
originalArticleId: 4f1c673e-8917-40ed-a88e-8710a00aec8a
redirect_from:
  - /2021080/docs/rest-api-reference
  - /2021080/docs/en/rest-api-reference
  - /docs/rest-api-reference
  - /docs/en/rest-api-reference
  - /docs/scos/dev/glue-api-guides/202204.0/rest-api-reference.html
---

This document provides an overview of REST API endpoints provided by the Spryker B2C Demo Shop by default. For each endpoint, you will find its URL relative to the server, REST request parameters, as well as the appropriate request and response data formats.

<div id="swagger-ui"></div>

{% raw %}
<link rel="stylesheet" type="text/css" href="https://cdnjs.cloudflare.com/ajax/libs/swagger-ui/3.22.1/swagger-ui.css" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/swagger-ui/3.22.1/swagger-ui-standalone-preset.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/swagger-ui/3.22.1/swagger-ui-bundle.js"></script>
<script>
const swaggerContainer = document.getElementById('swagger-ui');
if(swaggerContainer) { 
    console.log('start'); const ui = SwaggerUIBundle({
        url: 'https://spryker.s3.eu-central-1.amazonaws.com/docs/scos/dev/glue-api-guides/202204.0/b2c_spryker_rest_api.schema.json',
        dom_id: '#swagger-ui', deepLinking: true, presets: [
            SwaggerUIBundle.presets.apis, SwaggerUIStandalonePreset
        ],
        enableCORS: false, layout: 'BaseLayout', supportedSubmitMethods: []
    });
    console.log(ui); window.ui = ui 
}
</script>
{% endraw %}
