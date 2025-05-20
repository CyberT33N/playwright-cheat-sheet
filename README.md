# Playwright Cheat Sheet
Playwright Cheat Sheet with the most needed stuff..


<br><br>

# DOCS
- https://playwright.dev/docs/intro

<br><br>

# API
- https://playwright.dev/docs/api/class-playwright


















<br><br>
_________________________________________________
_________________________________________________
<br><br>


# Launch
```javascript
const { chromium } = require('playwright');

(async () => {
  const browser = await chromium.launch();
  // Create pages, interact with UI elements, assert values
  await browser.close();
})();
```



















<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



# Browser

<br><br>

## .isConnected()
- https://playwright.dev/docs/api/class-browser?_highlight=isconne#browserisconnected
- Indicates that the browser is connected.
```javascript
const res = await browser.isConnected() // true or false
```  





























<br><br>
 _____________________________________________________
 _____________________________________________________
<br><br>



# Event Listener

<br><br>

## disconnected (https://playwright.dev/docs/api/class-browser?_highlight=discon#browserondisconnected)
- Emitted when Browser gets disconnected from the browser application. This might happen because of one of the following:
  - Browser application is closed or crashed.
  - The browser.close() method was called.
```bash
browser.on('disconnected', doSomething())
```








































<br><br>
_________________________________________________
_________________________________________________
<br><br>


# Page


<br><br>


## New Page
```javascript
 const page = await browser.newPage();
```

<br><br>

## handle multiple pages
```javascript
(async () => {
  const browser = await playwright.chromium.launch();
  const context = await browser.newContext();
  const page = await context.newPage();
  context.on("page", async newPage => {
    console.log("newPage", await newPage.title())
  })

  // emulate some opening in a new tab or popup
  await page.evaluate(() => window.open('https://google.com', '_blank'))
  // Keep in mind to have some blocking action there so that the browser won't be closed. In this case we are just waiting 2 seconds.
  await page.waitForTimeout(2000)
  await browser.close();
})();
```









































<br><br>
_________________________________________________
_________________________________________________
<br><br>


# Context


<br><br>


## Disable Javascript
```javascript
const browser = await playwright.chromium.launch();
const context = await browser.newContext({
  javaScriptEnabled: false
});
const page = await context.newPage();
```



















<br><br>
_________________________________________________
_________________________________________________
<br><br>


# Code Generation
- Create automated tests
- https://playwright.dev/docs/codegen






















<br><br>
_________________________________________________
_________________________________________________
<br><br>



Playwright Test
- Playwright Test provides a test function to declare tests and expect function to write assertions.

```
import { test, expect } from '@playwright/test';

test('basic test', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  const name = await page.innerText('.navbar__title');
  expect(name).toBe('Playwright');
});
```

<details><summary>Click to expand..</summary>



</details>
