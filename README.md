# Playwright Cheat Sheet
Playwright Cheat Sheet with the most needed stuff..


<br><br>

# DOCS
- https://playwright.dev/docs/intro

<br><br>

# API
- https://playwright.dev/docs/api/












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
_________________________________________________
_________________________________________________
<br><br>


# Page


<br><br>


## New Page
```javascript
 const page = await browser.newPage();
```


