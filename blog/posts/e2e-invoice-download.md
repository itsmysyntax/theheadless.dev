---
title: E2E Invoice Download
date: 2020-08-31
author: Giovanni Rago
githubUser: ragog
tags: 
  - e2e
  - file download
---

Websites might expose files for users to download and then access from their local machine. Common cases are downloading tickets, receipts and itineraries.

## Steps

This example runs against our [test webshop](https://danube-webshop.herokuapp.com/) and proceeds to download a receipt for a previous purchase. It includes the following steps:

1. Logging in to the website
2. Navigating to the account page
3. Downloading a linked file

We will check that the downloaded file is as expected by comparing it to a [fixture file](test-data-intro.md) in our final assertion.

We can approach this scenario in different ways. One possibility is to perform the first two steps, then [extract](basics-scraping.md) the `href` value and use it to retrieve the file with a `GET` request (performed with [axios](https://github.com/axios/axios), for example).

:::: tabs :options="{ useUrlFragment: false }"
::: tab Puppeteer 
<<< @/blog/snippets/puppeteer/file-download.js{29-34}
:::
::: tab Playwright
<<< @/blog/snippets/playwright/file-download.js{22-27}
:::
::::

We could also click the link directly and wait for the download event, then proceed with the comparison. Waiting for the download event is currently not supported by Puppeteer.

Note that in this case, we need to enable downloads in the browser context before proceeding.

:::: tabs :options="{ useUrlFragment: false }"
::: tab Playwright
<<< @/blog/snippets/playwright/file-download-alt.js{7}
:::
::::

Both examples can be run as follows:
:::: tabs :options="{ useUrlFragment: false }"
::: tab MacOS
```sh
USER_EMAIL=user@email.com USER_PASSWORD=supersecure1 node file-download.js
```
:::
::: tab Windows
```sh
SET USER_EMAIL=user@email.com
SET USER_PASSWORD=supersecure1
node file-download.js
```
:::
::::

## Takeaways

1. Use environment variables to inject secrets.
2. Compare the expected file with the newly downloaded one.
3. There is more than one way to download a file within our script.

## Further reading

1. [Playwright's](https://playwright.dev/#version=v1.3.0&path=docs%2Fapi.md&q=class-download) documentation on downloading files.