---
title: Managing glossary
description: he guide provides instructions for shop owners to learn how to create and handle information in different languages in the Back Office.
last_updated: Jun 16, 2021
template: back-office-user-guide-template
originalLink: https://documentation.spryker.com/2021080/docs/managing-glossary
originalArticleId: a485b9af-b90b-4847-904c-3f87891de5c9
redirect_from:
  - /2021080/docs/managing-glossary
  - /2021080/docs/en/managing-glossary
  - /docs/managing-glossary
  - /docs/en/managing-glossary
  - /docs/scos/user/back-office-user-guides/202200.0/administration/glossary/managing-glossary.html
---

This article describes how to manage translations.

## Prerequisites

To start managing translations, go to the **Administration&nbsp;<span aria-label="and then">></span> Glossary** section.

Review the reference information before you start, or look up the necessary information as you go through the process.

## Creating a new translation

To create a new translation, do the following:

1. In the top-right corner of the **Overview of Translations** page click **Create Translation**.
2. On the **Create Translation** page, do the following:
3. In the **Name** field, enter the glossary key.

{% info_block errorBox %}

You can not just add/update a glossary key without using it in code, otherwise the translation will not be applied. Therefore, if you need to add a new translation, ask a developer to apply the glossary key and its translation values in code as well.

{% endinfo_block %}

4. Fill in the glossary values per the locales.
5. Click **Save**.

This is the example of how the translations can look like:
![Example of translations](https://spryker.s3.eu-central-1.amazonaws.com/docs/User+Guides/Back+Office+User+Guides/Glossary/Managing+Glossary/managing-glossary.png)

## Editing an existing translation

To edit an existing translation, do the following:

1. On the **Overview of Translations** page, for a specific glossary key, in the **Actions** column, click **Edit**.
2. On the **Edit Translation** page, change the translation values in the locales—the **Name** field is greyed out, and thus the glossary key itself is not available for modifications.
3. Click **Save**.
