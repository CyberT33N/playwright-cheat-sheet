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




# Playwright Test
- Playwright Test provides a test function to declare tests and expect function to write assertions.

```
import { test, expect } from '@playwright/test';

test('basic test', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  const name = await page.innerText('.navbar__title');
  expect(name).toBe('Playwright');
});
```









## Test Configuration

<details><summary>Click to expand..</summary>


## Grundlegende Konfiguration (`Basic Configuration`)

Einige der gebräuchlichsten Konfigurationsoptionen.

```typescript
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  // Verzeichnis für Testdateien, relativ zu dieser Konfigurationsdatei.
  testDir: 'tests',

  // Alle Tests parallel ausführen.
  fullyParallel: true,

  // Build auf CI fehlschlagen lassen, wenn versehentlich test.only im Quellcode vergessen wurde.
  forbidOnly: !!process.env.CI,

  // Wiederholungsversuche nur auf CI.
  retries: process.env.CI ? 2 : 0,

  // Parallele Tests auf CI deaktivieren (Anzahl der Worker begrenzen).
  workers: process.env.CI ? 1 : undefined,

  // Zu verwendender Reporter.
  reporter: 'html',

  use: {
    // Basis-URL für Aktionen wie `await page.goto('/')`.
    baseURL: 'http://localhost:3000',

    // Trace sammeln, wenn ein fehlgeschlagener Test wiederholt wird.
    trace: 'on-first-retry',
  },
  // Projekte für wichtige Browser konfigurieren.
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    // Weitere Browser wie Firefox, WebKit können hier hinzugefügt werden
    // {
    //   name: 'firefox',
    //   use: { ...devices['Desktop Firefox'] },
    // },
    // {
    //   name: 'webkit',
    //   use: { ...devices['Desktop Safari'] },
    // },
  ],
  // Lokalen Entwicklungsserver vor dem Start der Tests ausführen.
  webServer: {
    command: 'npm run start',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
  },
});
```

| Option                   | Beschreibung                                                                                                                                                              |
| :----------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `testConfig.testDir`     | Verzeichnis mit den Testdateien.                                                                                                                                          |
| `testConfig.fullyParallel`| Lässt alle Tests in allen Dateien parallel laufen. Siehe [Parallelism and Sharding](https://playwright.dev/docs/test-parallel) für mehr Details.                         |
| `testConfig.forbidOnly`  | Ob mit einem Fehler beendet werden soll, wenn Tests als `test.only` markiert sind. Nützlich auf CI.                                                                         |
| `testConfig.retries`     | Die maximale Anzahl von Wiederholungsversuchen pro Test. Siehe [Test Retries](https://playwright.dev/docs/test-retries) für mehr Details.                                  |
| `testConfig.workers`     | Die maximale Anzahl gleichzeitiger Worker-Prozesse für die Parallelisierung von Tests. Kann auch als Prozentsatz der logischen CPU-Kerne festgelegt werden, z.B. `'50%'`. Siehe [Parallelism and Sharding](https://playwright.dev/docs/test-parallel). |
| `testConfig.reporter`    | Zu verwendender Reporter. Siehe [Test Reporters](https://playwright.dev/docs/test-reporters) für verfügbare Reporter.                                                     |
| `testConfig.use`         | Globaler Satz von Optionen, die an `BrowserContext` und `Page` übergeben werden. Optionen in `use{}`.                                                                     |
| `testConfig.projects`    | Tests in mehreren Konfigurationen oder auf mehreren Browsern ausführen.                                                                                                   |
| `testConfig.webServer`   | Um einen Server während der Tests zu starten, verwende die `webServer`-Option.                                                                                           |

---

## Tests Filtern (`Filtering Tests`)

Filtere Tests nach Glob-Mustern oder regulären Ausdrücken.

```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  // Glob-Muster oder reguläre Ausdrücke, um Testdateien zu ignorieren.
  testIgnore: '*test-assets', // z.B. '*test-assets/**' oder /.*test-assets.*/

  // Glob-Muster oder reguläre Ausdrücke, die auf Testdateien passen.
  testMatch: '*todo-tests/*.spec.ts', // z.B. /.*todo-tests\/.*\.spec\.ts/
});
```

| Option                  | Beschreibung                                                                                                                                                           |
| :---------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `testConfig.testIgnore` | Glob-Muster oder reguläre Ausdrücke, die beim Suchen nach Testdateien ignoriert werden sollen. Zum Beispiel `'*test-assets'` oder `['**/test-assets/**', '**/data/**']`. |
| `testConfig.testMatch`  | Glob-Muster oder reguläre Ausdrücke, die auf Testdateien passen. Zum Beispiel `'*todo-tests/*.spec.ts'`. Standardmäßig führt Playwright `.*(test\|spec)\.(js\|ts\|mjs)` Dateien aus. |

---

## Erweiterte Konfiguration (`Advanced Configuration`)

```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  // Ordner für Testartefakte wie Screenshots, Videos, Traces usw.
  outputDir: 'test-results',

  // Pfad zu den globalen Setup-Dateien.
  globalSetup: require.resolve('./global-setup'), // z.B. './global-setup.ts'

  // Pfad zu den globalen Teardown-Dateien.
  globalTeardown: require.resolve('./global-teardown'), // z.B. './global-teardown.ts'

  // Jeder Test erhält 30 Sekunden Zeit.
  timeout: 30000, // 30 Sekunden
});
```

| Option                     | Beschreibung                                                                                                                                                                                                |
| :------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `testConfig.outputDir`     | Ordner für Testartefakte wie Screenshots, Videos, Traces usw. Standardmäßig `test-results`.                                                                                                               |
| `testConfig.globalSetup`   | Pfad zur globalen Setup-Datei. Diese Datei wird vor allen Tests `require`d und ausgeführt. Sie muss eine einzelne Funktion exportieren (kann `async` sein).                                                  |
| `testConfig.globalTeardown`| Pfad zur globalen Teardown-Datei. Diese Datei wird nach allen Tests `require`d und ausgeführt. Sie muss eine einzelne Funktion exportieren (kann `async` sein).                                                |
| `testConfig.timeout`       | Playwright erzwingt ein Timeout für jeden Test, standardmäßig 30 Sekunden. Die vom Test selbst, von Test-Fixtures und `beforeEach`-Hooks verbrachte Zeit ist im Test-Timeout enthalten. `0` deaktiviert das Timeout. |

---

## Expect-Optionen (`Expect Options`)

Konfiguration für die `expect`-Assertionsbibliothek.

```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test';

export default defineConfig({
  expect: {
    // Maximale Zeit, die expect() auf die Erfüllung der Bedingung warten soll.
    timeout: 5000, // 5 Sekunden

    toHaveScreenshot: {
      // Eine akzeptable Anzahl von Pixeln, die unterschiedlich sein dürfen, standardmäßig nicht gesetzt.
      maxDiffPixels: 10,
    },

    toMatchSnapshot: {
      // Ein akzeptables Verhältnis von unterschiedlichen Pixeln zur
      // Gesamtanzahl der Pixel, zwischen 0 und 1.
      maxDiffPixelRatio: 0.1,
    },
  },
});
```

| Option                                    | Beschreibung                                                                                                                                                                                                                       |
| :---------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `testConfig.expect.timeout`               | Web-First-Assertions wie `expect(locator).toHaveText()` haben standardmäßig ein separates Timeout von 5 Sekunden. Dies ist die maximale Zeit, die `expect()` auf die Erfüllung der Bedingung warten soll. [Mehr erfahren](https://playwright.dev/docs/test-timeouts). |
| `testConfig.expect.toHaveScreenshot`      | Konfiguration für die Methode `expect(locatorOrPage).toHaveScreenshot()`. Enthält Optionen wie `maxDiffPixels`, `maxDiffPixelRatio`, `threshold`.                                                                                 |
| `testConfig.expect.toHaveScreenshot.maxDiffPixels` | Eine akzeptable absolute Anzahl von Pixeln, die unterschiedlich sein dürfen.                                                                                                                                                   |
| `testConfig.expect.toMatchSnapshot`       | Konfiguration für die Methode `expect(value).toMatchSnapshot()`. Enthält Optionen wie `maxDiffPixels`, `maxDiffPixelRatio`, `threshold`.                                                                                           |
| `testConfig.expect.toMatchSnapshot.maxDiffPixelRatio` | Ein akzeptables Verhältnis (zwischen 0 und 1) von unterschiedlichen Pixeln zur Gesamtanzahl der Pixel.                                                                                                                   |


</details>





## API

<details><summary>Click to expand..</summary>

## Grundlagen

**Import:**
```javascript
import { test, expect } from '@playwright/test';
```

**Beispielhafter Test:**
```javascript
test('basic test', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  const name = await page.innerText('.navbar__title');
  expect(name).toBe('Playwright');
});
```

---

## `test` (Methode)
_Hinzugefügt in: v1.10_

Deklariert einen Test.

### Signaturen
```javascript
test(title, body)
test(title, details, body)
```

### Usage
```javascript
import { test, expect } from '@playwright/test';

test('basic test', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  // ...
});
```

### Tags
Tests können durch zusätzliche Testdetails getaggt werden. Alternativ können Tags in den Testtitel aufgenommen werden. Jeder Tag muss mit `@` beginnen.

```javascript
import { test, expect } from '@playwright/test';

test('basic test', {
  tag: '@smoke',
}, async ({ page }) => {
  await page.goto('https://playwright.dev/');
  // ...
});

test('another test @smoke', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  // ...
});
```
Test-Tags werden im Testbericht angezeigt und sind über `TestCase.tags` für benutzerdefinierte Reporter verfügbar.
Tests können über ihre Tags gefiltert werden:
*   In der Kommandozeile.
*   In der Konfiguration mit `testConfig.grep` und `testProject.grep`.
*   [Mehr über Tagging erfahren](https://playwright.dev/docs/test-annotations#tagging).

### Annotations
Tests können durch zusätzliche Testdetails annotiert werden.

```javascript
import { test, expect } from '@playwright/test';

test('basic test', {
  annotation: {
    type: 'issue',
    description: 'https://github.com/microsoft/playwright/issues/23180',
  },
}, async ({ page }) => {
  await page.goto('https://playwright.dev/');
  // ...
});
```
Test-Annotationen werden im Testbericht angezeigt und sind über `TestCase.annotations` für benutzerdefinierte Reporter verfügbar. Annotationen können auch zur Laufzeit durch Manipulation von `testInfo.annotations` hinzugefügt werden.
*   [Mehr über Test-Annotationen erfahren](https://playwright.dev/docs/test-annotations).

### Argumente
*   **`title`**: `string`
    *   Testtitel.
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_
    *   Zusätzliche Testdetails.
    *   **`tag`**: `string | Array<string>` (optional)
        *   Ein oder mehrere Tags.
    *   **`annotation`**: `Object | Array<Object>` (optional)
        *   Eine oder mehrere Annotationen.
        *   **`type`**: `string`
            *   Annotationstyp, z.B. 'issue'.
        *   **`description`**: `string` (optional)
            *   Optionale Annotationsbeschreibung, z.B. eine Issue-URL.
*   **`body`**: `function(Fixtures, TestInfo)`
    *   Testkörper, der ein oder zwei Argumente akzeptiert: ein Objekt mit Fixtures und optional `TestInfo`.

---

## `test.afterAll` (Hook)
_Hinzugefügt in: v1.10_

Deklariert einen `afterAll`-Hook, der einmal pro Worker nach allen Tests ausgeführt wird.
*   Wenn in einer Testdatei aufgerufen, läuft er nach allen Tests in dieser Datei.
*   Wenn innerhalb einer `test.describe()`-Gruppe aufgerufen, läuft er nach allen Tests in dieser Gruppe.

### Signaturen
```javascript
test.afterAll(hookFunction)
test.afterAll(title, hookFunction)
```

### Usage
```javascript
test.afterAll(async () => {
  console.log('Done with tests');
  // ...
});

// Alternativ mit Titel:
test.afterAll('Teardown', async () => {
  console.log('Done with tests');
  // ...
});
```

### Argumente
*   **`title`**: `string` (optional) _Hinzugefügt in: v1.38_
    *   Hook-Titel.
*   **`hookFunction`**: `function(Fixtures, TestInfo)`
    *   Hook-Funktion, die ein oder zwei Argumente akzeptiert: ein Objekt mit Worker-Fixtures und optional `TestInfo`.

### Details
*   Mehrere `afterAll`-Hooks werden in der Reihenfolge ihrer Registrierung ausgeführt.
*   Der Worker-Prozess wird bei Testfehlschlägen neu gestartet, und der `afterAll`-Hook läuft erneut im neuen Worker. [Mehr über Worker und Fehlschläge](https://playwright.dev/docs/test-parallel#worker-process-isolation).
*   Playwright führt weiterhin alle anwendbaren Hooks aus, auch wenn einige fehlgeschlagen sind.

---

## `test.afterEach` (Hook)
_Hinzugefügt in: v1.10_

Deklariert einen `afterEach`-Hook, der nach jedem Test ausgeführt wird.
*   Wenn in einer Testdatei aufgerufen, läuft er nach jedem Test in dieser Datei.
*   Wenn innerhalb einer `test.describe()`-Gruppe aufgerufen, läuft er nach jedem Test in dieser Gruppe.
*   Kann auf dieselben Fixtures wie der Testkörper und auf das `TestInfo`-Objekt zugreifen.

### Signaturen
```javascript
test.afterEach(hookFunction)
test.afterEach(title, hookFunction)
```

### Usage
```javascript
// example.spec.ts
import { test, expect } from '@playwright/test';

test.afterEach(async ({ page }, testInfo) => { // Korrigiert, um testInfo zu verwenden
  console.log(`Finished ${testInfo.title} with status ${testInfo.status}`);

  if (testInfo.status !== testInfo.expectedStatus)
    console.log(`Did not run as expected, ended up at ${page.url()}`);
});

test('my test', async ({ page }) => {
  // ...
});

// Alternativ mit Titel:
test.afterEach('Status check', async ({ page }, testInfo) => { // Korrigiert, um testInfo zu verwenden
  if (testInfo.status !== testInfo.expectedStatus)
    console.log(`Did not run as expected, ended up at ${page.url()}`);
});
```

### Argumente
*   **`title`**: `string` (optional) _Hinzugefügt in: v1.38_
    *   Hook-Titel.
*   **`hookFunction`**: `function(Fixtures, TestInfo)`
    *   Hook-Funktion, die ein oder zwei Argumente akzeptiert: ein Objekt mit Fixtures und optional `TestInfo`.

### Details
*   Mehrere `afterEach`-Hooks werden in der Reihenfolge ihrer Registrierung ausgeführt.
*   Playwright führt weiterhin alle anwendbaren Hooks aus, auch wenn einige fehlgeschlagen sind.

---

## `test.beforeAll` (Hook)
_Hinzugefügt in: v1.10_

Deklariert einen `beforeAll`-Hook, der einmal pro Worker-Prozess vor allen Tests ausgeführt wird.
*   Wenn in einer Testdatei aufgerufen, läuft er vor allen Tests in dieser Datei.
*   Wenn innerhalb einer `test.describe()`-Gruppe aufgerufen, läuft er vor allen Tests in dieser Gruppe.
*   `test.afterAll()` kann verwendet werden, um Ressourcen abzubauen, die in `beforeAll` eingerichtet wurden.

### Signaturen
```javascript
test.beforeAll(hookFunction)
test.beforeAll(title, hookFunction)
```

### Usage
```javascript
// example.spec.ts
import { test, expect } from '@playwright/test';

test.beforeAll(async () => {
  console.log('Before tests');
});

test.afterAll(async () => {
  console.log('After tests');
});

test('my test', async ({ page }) => {
  // ...
});

// Alternativ mit Titel:
test.beforeAll('Setup', async () => {
  console.log('Before tests');
});
```

### Argumente
*   **`title`**: `string` (optional) _Hinzugefügt in: v1.38_
    *   Hook-Titel.
*   **`hookFunction`**: `function(Fixtures, TestInfo)`
    *   Hook-Funktion, die ein oder zwei Argumente akzeptiert: ein Objekt mit Worker-Fixtures und optional `TestInfo`.

### Details
*   Mehrere `beforeAll`-Hooks werden in der Reihenfolge ihrer Registrierung ausgeführt.
*   Der Worker-Prozess wird bei Testfehlschlägen neu gestartet, und der `beforeAll`-Hook läuft erneut im neuen Worker. [Mehr über Worker und Fehlschläge](https://playwright.dev/docs/test-parallel#worker-process-isolation).
*   Playwright führt weiterhin alle anwendbaren Hooks aus, auch wenn einige fehlgeschlagen sind.

---

## `test.beforeEach` (Hook)
_Hinzugefügt in: v1.10_

Deklariert einen `beforeEach`-Hook, der vor jedem Test ausgeführt wird.
*   Wenn in einer Testdatei aufgerufen, läuft er vor jedem Test in dieser Datei.
*   Wenn innerhalb einer `test.describe()`-Gruppe aufgerufen, läuft er vor jedem Test in dieser Gruppe.
*   Kann auf dieselben Fixtures wie der Testkörper und auf das `TestInfo`-Objekt zugreifen.
*   `test.afterEach()` kann verwendet werden, um Ressourcen abzubauen, die in `beforeEach` eingerichtet wurden.

### Signaturen
```javascript
test.beforeEach(hookFunction)
test.beforeEach(title, hookFunction)
```

### Usage
```javascript
// example.spec.ts
import { test, expect } from '@playwright/test';

test.beforeEach(async ({ page }, testInfo) => { // Korrigiert, um testInfo zu verwenden
  console.log(`Running ${testInfo.title}`);
  await page.goto('https://my.start.url/');
});

test('my test', async ({ page }) => {
  expect(page.url()).toBe('https://my.start.url/');
});

// Alternativ mit Titel:
test.beforeEach('Open start URL', async ({ page }, testInfo) => { // Korrigiert, um testInfo zu verwenden
  console.log(`Running ${testInfo.title}`);
  await page.goto('https://my.start.url/');
});
```

### Argumente
*   **`title`**: `string` (optional) _Hinzugefügt in: v1.38_
    *   Hook-Titel.
*   **`hookFunction`**: `function(Fixtures, TestInfo)`
    *   Hook-Funktion, die ein oder zwei Argumente akzeptiert: ein Objekt mit Fixtures und optional `TestInfo`.

### Details
*   Mehrere `beforeEach`-Hooks werden in der Reihenfolge ihrer Registrierung ausgeführt.
*   Playwright führt weiterhin alle anwendbaren Hooks aus, auch wenn einige fehlgeschlagen sind.

---

## `test.describe` (Methode)
_Hinzugefügt in: v1.10_

Deklariert eine Gruppe von Tests.

### Signaturen
```javascript
test.describe(title, callback)
test.describe(callback)
test.describe(title, details, callback)
```

### Usage
**Gruppe mit Titel:**
```javascript
test.describe('two tests', () => {
  test('one', async ({ page }) => {
    // ...
  });

  test('two', async ({ page }) => {
    // ...
  });
});
```

**Anonyme Gruppe:**
Nützlich, um einer Gruppe von Tests eine gemeinsame Option mit `test.use()` zu geben.
```javascript
test.describe(() => {
  test.use({ colorScheme: 'dark' });

  test('one', async ({ page }) => {
    // ...
  });

  test('two', async ({ page }) => {
    // ...
  });
});
```

### Tags
Alle Tests in einer Gruppe können durch zusätzliche Details getaggt werden. Jeder Tag muss mit `@` beginnen.
```javascript
import { test, expect } from '@playwright/test';

test.describe('two tagged tests', {
  tag: '@smoke',
}, () => {
  test('one', async ({ page }) => {
    // ...
  });

  test('two', async ({ page }) => {
    // ...
  });
});
```
*   [Mehr über Tagging erfahren](https://playwright.dev/docs/test-annotations#tagging).

### Annotations
Alle Tests in einer Gruppe können durch zusätzliche Details annotiert werden.
```javascript
import { test, expect } from '@playwright/test';

test.describe('two annotated tests', {
  annotation: {
    type: 'issue',
    description: 'https://github.com/microsoft/playwright/issues/23180',
  },
}, () => {
  test('one', async ({ page }) => {
    // ...
  });

  test('two', async ({ page }) => {
    // ...
  });
});
```
*   [Mehr über Test-Annotationen erfahren](https://playwright.dev/docs/test-annotations).

### Argumente
*   **`title`**: `string` (optional)
    *   Gruppentitel.
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_
    *   Zusätzliche Details für alle Tests in der Gruppe.
    *   **`tag`**: `string | Array<string>` (optional)
    *   **`annotation`**: `Object | Array<Object>` (optional)
        *   **`type`**: `string`
        *   **`description`**: `string` (optional)
*   **`callback`**: `function`
    *   Ein Callback, der sofort beim Aufruf von `test.describe()` ausgeführt wird. Alle in diesem Callback deklarierten Tests gehören zur Gruppe.

---

## `test.describe.configure` (Methode)
_Hinzugefügt in: v1.10_

Konfiguriert den umschließenden Geltungsbereich. Kann auf oberster Ebene oder innerhalb eines `describe` ausgeführt werden. Die Konfiguration gilt für den gesamten Geltungsbereich, unabhängig davon, ob sie vor oder nach der Testdeklaration ausgeführt wird.
*   [Mehr über Ausführungsmodi](https://playwright.dev/docs/test-parallel#execution-modes).

### Usage
**Tests parallel ausführen:**
```javascript
// Alle Tests in der Datei gleichzeitig mit parallelen Workern ausführen.
test.describe.configure({ mode: 'parallel' });
test('runs in parallel 1', async ({ page }) => {});
test('runs in parallel 2', async ({ page }) => {});
```

**Tests in Reihenfolge ausführen, jeden fehlgeschlagenen Test unabhängig wiederholen (Standardmodus):**
```javascript
// Tests in dieser Datei laufen nacheinander. Wiederholungen, falls vorhanden, laufen unabhängig.
test.describe.configure({ mode: 'default' });
test('runs first', async ({ page }) => {});
test('runs second', async ({ page }) => {});
```

**Tests seriell ausführen, Wiederholung von Anfang an. Wenn ein serieller Test fehlschlägt, werden alle nachfolgenden Tests übersprungen:**
> **Hinweis:** Die serielle Ausführung wird nicht empfohlen. Es ist normalerweise besser, Tests isoliert zu gestalten, damit sie unabhängig ausgeführt werden können.
```javascript
// Tests als voneinander abhängig annotieren.
test.describe.configure({ mode: 'serial' });
test('runs first', async ({ page }) => {});
test('runs second', async ({ page }) => {});
```

**Wiederholungen und Timeout für jeden Test konfigurieren:**
```javascript
// Jeder Test in der Datei wird zweimal wiederholt und hat ein Timeout von 20 Sekunden.
test.describe.configure({ retries: 2, timeout: 20000 }); // 20_000 ist 20000
test('runs first', async ({ page }) => {});
test('runs second', async ({ page }) => {});
```

**Mehrere `describe`-Blöcke parallel ausführen, aber Tests innerhalb jedes `describe`-Blocks in Reihenfolge:**
```javascript
test.describe.configure({ mode: 'parallel' });

test.describe('A, runs in parallel with B', () => {
  test.describe.configure({ mode: 'default' });
  test('in order A1', async ({ page }) => {});
  test('in order A2', async ({ page }) => {});
});

test.describe('B, runs in parallel with A', () => {
  test.describe.configure({ mode: 'default' });
  test('in order B1', async ({ page }) => {});
  test('in order B2', async ({ page }) => {});
});
```

### Argumente
*   **`options`**: `Object` (optional)
    *   **`mode`**: `"default" | "parallel" | "serial"` (optional)
        *   Ausführungsmodus. [Mehr erfahren](https://playwright.dev/docs/test-parallel#execution-modes).
    *   **`retries`**: `number` (optional) _Hinzugefügt in: v1.28_
        *   Die Anzahl der Wiederholungen für jeden Test.
    *   **`timeout`**: `number` (optional) _Hinzugefügt in: v1.28_
        *   Timeout für jeden Test in Millisekunden. Überschreibt `testProject.timeout` und `testConfig.timeout`.

---

## `test.describe.fixme` (Methode)
_Hinzugefügt in: v1.25_

Deklariert eine Testgruppe ähnlich wie `test.describe()`. Tests in dieser Gruppe werden als "fixme" markiert und nicht ausgeführt.

### Signaturen
```javascript
test.describe.fixme(title, callback)
test.describe.fixme(callback)
test.describe.fixme(title, details, callback)
```

### Usage
```javascript
test.describe.fixme('broken tests that should be fixed', () => {
  test('example', async ({ page }) => {
    // Dieser Test wird nicht ausgeführt
  });
});

// Ohne Titel:
test.describe.fixme(() => {
  // ...
});
```

### Argumente
*   **`title`**: `string` (optional)
    *   Gruppentitel.
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_
    *   Siehe `test.describe()` für Detailbeschreibung (tag, annotation).
    *   **`tag`**: `string | Array<string>` (optional)
    *   **`annotation`**: `Object | Array<Object>` (optional)
        *   **`type`**: `string`
        *   **`description`**: `string` (optional)
*   **`callback`**: `function`
    *   Ein Callback, der sofort beim Aufruf von `test.describe.fixme()` ausgeführt wird. Alle in diesem Callback hinzugefügten Tests gehören zur Gruppe und werden nicht ausgeführt.

---

## `test.describe.only` (Methode)
_Hinzugefügt in: v1.10_

Deklariert eine fokussierte Testgruppe. Wenn es fokussierte Tests oder Suiten gibt, werden nur diese ausgeführt.

### Signaturen
```javascript
test.describe.only(title, callback)
test.describe.only(callback)
test.describe.only(title, details, callback)
```

### Usage
```javascript
test.describe.only('focused group', () => {
  test('in the focused group', async ({ page }) => {
    // Dieser Test wird ausgeführt
  });
});
test('not in the focused group', async ({ page }) => {
  // Dieser Test wird nicht ausgeführt
});

// Ohne Titel:
test.describe.only(() => {
  // ...
});
```

### Argumente
*   **`title`**: `string` (optional)
    *   Gruppentitel.
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_
    *   Siehe `test.describe()` für Detailbeschreibung (tag, annotation).
    *   **`tag`**: `string | Array<string>` (optional)
    *   **`annotation`**: `Object | Array<Object>` (optional)
        *   **`type`**: `string`
        *   **`description`**: `string` (optional)
*   **`callback`**: `function`
    *   Ein Callback, der sofort beim Aufruf von `test.describe.only()` ausgeführt wird. Alle in diesem Callback hinzugefügten Tests gehören zur Gruppe.

---

## `test.describe.skip` (Methode)
_Hinzugefügt in: v1.10_

Deklariert eine übersprungene Testgruppe, ähnlich wie `test.describe()`. Tests in der übersprungenen Gruppe werden niemals ausgeführt.

### Signaturen
```javascript
test.describe.skip(title, callback) // Der Text sagt: test.describe.skip(title) - aber das macht ohne callback wenig Sinn für eine Gruppe. Ich nehme an, es ist ein Tippfehler und sollte title, callback sein oder title, details, callback, wenn details optional ist. Das callback Argument fehlt auch in der Beschreibung der Argumente für diese Signatur.
test.describe.skip(title) // Diese Signatur ist im Text vorhanden, aber das callback Argument fehlt in der Argumentbeschreibung.
test.describe.skip(title, details, callback)
```
> Anmerkung: Die offizielle Doku listet `test.describe.skip(title)` und `test.describe.skip(title, callback)` und `test.describe.skip(title, details, callback)`. Die Variante `test.describe.skip(callback)` (ohne Titel) ist auch implizit möglich, wie bei den anderen `describe` Varianten.

Ich werde die vom Text bereitgestellten und die logisch sinnvollen Signaturen auflisten:
```javascript
test.describe.skip(title, callback)
test.describe.skip(callback) // Implizit durch Analogie
test.describe.skip(title, details, callback)
test.describe.skip(title) // Deklariert eine leere übersprungene Gruppe, kann aber auch als bedingtes Überspringen interpretiert werden. Besser explizit sein.
```


### Usage
```javascript
test.describe.skip('skipped group', () => {
  test('example', async ({ page }) => {
    // Dieser Test wird nicht ausgeführt
  });
});

// Ohne Titel (implizit):
test.describe.skip(() => {
  // ...
});
```

### Argumente
*   **`title`**: `string` (optional, aber für die Variante `test.describe.skip(title)` erforderlich)
    *   Gruppentitel.
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_
    *   Siehe `test.describe()` für Detailbeschreibung (tag, annotation).
    *   **`tag`**: `string | Array<string>` (optional)
    *   **`annotation`**: `Object | Array<Object>` (optional)
        *   **`type`**: `string`
        *   **`description`**: `string` (optional)
*   **`callback`**: `function`
    *   Ein Callback, der sofort beim Aufruf von `test.describe.skip()` ausgeführt wird. Alle in diesem Callback hinzugefügten Tests gehören zur Gruppe und werden nicht ausgeführt.

---

## `test.extend` (Methode)
_Hinzugefügt in: v1.10_

Erweitert das `test`-Objekt durch Definition von Fixtures und/oder Optionen, die in den Tests verwendet werden können.

### Usage
**1. Fixture und/oder Option definieren:**
```typescript
// my-test.ts
import { test as base } from '@playwright/test';
import { TodoPage } from './todo-page'; // Angenommene Hilfsklasse

export type Options = { defaultItem: string };

// Erweitere den Basistest um eine "defaultItem"-Option und eine "todoPage"-Fixture.
export const test = base.extend<Options & { todoPage: TodoPage }>({
  // Definiere eine Option und gib einen Standardwert an.
  defaultItem: ['Do stuff', { option: true }],

  // Definiere eine Fixture. Beachte, dass sie die eingebaute "page"-Fixture
  // und die neue Option "defaultItem" verwenden kann.
  todoPage: async ({ page, defaultItem }, use) => {
    const todoPage = new TodoPage(page);
    await todoPage.goto();
    await todoPage.addToDo(defaultItem);
    await use(todoPage);
    await todoPage.removeAll(); // Beispielhafter Aufräumschritt
  },
});
```

**2. Fixture im Test verwenden:**
```typescript
// example.spec.ts
import { test } from './my-test';

test('test 1', async ({ todoPage }) => {
  await todoPage.addToDo('my todo');
  // ...
});
```

**3. Option in der Konfigurationsdatei konfigurieren:**
```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test';
import type { Options } from './my-test'; // Pfad zu deiner Test-Erweiterung

export default defineConfig<Options>({ // Typisiere defineConfig mit deinen Optionen
  projects: [
    {
      name: 'shopping',
      use: { defaultItem: 'Buy milk' },
    },
    {
      name: 'wellbeing',
      use: { defaultItem: 'Exercise!' },
    },
  ]
});
```
*   [Mehr über Fixtures und Parametrisierung von Tests erfahren](https://playwright.dev/docs/test-fixtures).

### Argumente
*   **`fixtures`**: `Object`
    *   Ein Objekt, das Fixtures und/oder Optionen enthält. [Mehr über das Fixture-Format](https://playwright.dev/docs/test-fixtures#introduction).

### Returns
*   `Test`

---

## `test.fail` (Methode)
_Hinzugefügt in: v1.10_

Markiert einen Test als "sollte fehlschlagen". Playwright führt diesen Test aus und stellt sicher, dass er tatsächlich fehlschlägt. Nützlich zu Dokumentationszwecken, um anzuerkennen, dass eine Funktionalität defekt ist, bis sie behoben wird.

### Signaturen
**Um einen "fehlgeschlagenen" Test zu deklarieren:**
```javascript
test.fail(title, body)
test.fail(title, details, body)
```
**Um einen Test zur Laufzeit als "fehlgeschlagen" zu annotieren:**
```javascript
test.fail(condition, description)
test.fail(callback, description)
test.fail()
```

### Usage
**Deklarieren eines Tests als fehlschlagend:**
```javascript
import { test, expect } from '@playwright/test';

test.fail('not yet ready', async ({ page }) => {
  // ...
});
```

**Bedingtes Markieren als fehlschlagend im Testkörper:**
```javascript
import { test, expect } from '@playwright/test';

test('fail in WebKit', async ({ page, browserName }) => {
  test.fail(browserName === 'webkit', 'This feature is not implemented for Mac yet');
  // ...
});
```

**Bedingtes Markieren aller Tests in einer Datei oder `test.describe()`-Gruppe:**
```javascript
import { test, expect } from '@playwright/test';

test.fail(({ browserName }) => browserName === 'webkit', 'not implemented yet');

test('fail in WebKit 1', async ({ page }) => {
  // ...
});
test('fail in WebKit 2', async ({ page }) => {
  // ...
});
```

**`test.fail()` ohne Argumente (weniger empfohlen):**
```javascript
import { test, expect } from '@playwright/test';

test('less readable', async ({ page }) => {
  test.fail();
  // ...
});
```

### Argumente
*   **`title`**: `string` (optional) _Hinzugefügt in: v1.42_
    *   Testtitel.
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_
    *   Siehe `test()` für Detailbeschreibung (tag, annotation).
    *   **`tag`**: `string | Array<string>` (optional)
    *   **`annotation`**: `Object | Array<Object>` (optional)
        *   **`type`**: `string`
        *   **`description`**: `string` (optional)
*   **`body`**: `function(Fixtures, TestInfo)` (optional) _Hinzugefügt in: v1.42_
    *   Testkörper.
*   **`condition`**: `boolean` (optional)
    *   Test wird als "sollte fehlschlagen" markiert, wenn die Bedingung `true` ist.
*   **`callback`**: `function(Fixtures):boolean` (optional)
    *   Eine Funktion, die basierend auf Test-Fixtures zurückgibt, ob der Test als "sollte fehlschlagen" markiert werden soll.
*   **`description`**: `string` (optional)
    *   Optionale Beschreibung, die im Testbericht angezeigt wird.

---

## `test.fail.only` (Methode)
_Hinzugefügt in: v1.49_

Ermöglicht das Fokussieren auf einen bestimmten Test, von dem erwartet wird, dass er fehlschlägt. Besonders nützlich beim Debuggen eines fehlschlagenden Tests oder bei der Arbeit an einem bestimmten Problem.

### Signaturen
**Um einen fokussierten "fehlgeschlagenen" Test zu deklarieren:**
```javascript
test.fail.only(title, body)
test.fail.only(title, details, body)
```

### Usage
```javascript
import { test, expect } from '@playwright/test';

test.fail.only('focused failing test', async ({ page }) => {
  // Von diesem Test wird erwartet, dass er fehlschlägt
});
test('not in the focused group', async ({ page }) => {
  // Dieser Test wird nicht ausgeführt
});
```

### Argumente
*   **`title`**: `string` (optional, aber empfohlen)
    *   Testtitel.
*   **`details`**: `Object` (optional)
    *   Siehe `test.describe()` für Detailbeschreibung (tag, annotation).
    *   **`tag`**: `string | Array<string>` (optional)
    *   **`annotation`**: `Object | Array<Object>` (optional)
        *   **`type`**: `string`
        *   **`description`**: `string` (optional)
*   **`body`**: `function(Fixtures, TestInfo)` (optional, aber für eine Deklaration erforderlich)
    *   Testkörper.

---

## `test.fixme` (Methode)
_Hinzugefügt in: v1.10_

Markiert einen Test als "fixme", mit der Absicht, ihn zu beheben. Playwright führt den Test nach dem Aufruf von `test.fixme()` nicht weiter aus.

### Signaturen
**Um einen "fixme"-Test zu deklarieren:**
```javascript
test.fixme(title, body)
test.fixme(title, details, body)
```
**Um einen Test zur Laufzeit als "fixme" zu annotieren:**
```javascript
test.fixme(condition, description)
test.fixme(callback, description)
test.fixme()
```

### Usage
**Deklarieren eines Tests als "fixme":**
```javascript
import { test, expect } from '@playwright/test';

test.fixme('to be fixed', async ({ page }) => {
  // ...
});
```

**Bedingtes Markieren als "fixme" im Testkörper:**
Playwright führt den Test aus, bricht ihn aber sofort nach dem `test.fixme`-Aufruf ab.
```javascript
import { test, expect } from '@playwright/test';

test('to be fixed in Safari', async ({ page, browserName }) => {
  test.fixme(browserName === 'webkit', 'This feature breaks in Safari for some reason');
  // ...
});
```

**Bedingtes Markieren aller Tests in einer Datei oder `test.describe()`-Gruppe:**
```javascript
import { test, expect } from '@playwright/test';

test.fixme(({ browserName }) => browserName === 'webkit', 'Should figure out the issue');

test('to be fixed in Safari 1', async ({ page }) => {
  // ...
});
test('to be fixed in Safari 2', async ({ page }) => {
  // ...
});
```

**`test.fixme()` ohne Argumente (weniger empfohlen):**
```javascript
import { test, expect } from '@playwright/test';

test('less readable', async ({ page }) => {
  test.fixme();
  // ...
});
```

### Argumente
*   **`title`**: `string` (optional)
    *   Testtitel.
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_
    *   Siehe `test()` für Detailbeschreibung (tag, annotation).
    *   **`tag`**: `string | Array<string>` (optional)
    *   **`annotation`**: `Object | Array<Object>` (optional)
        *   **`type`**: `string`
        *   **`description`**: `string` (optional)
*   **`body`**: `function(Fixtures, TestInfo)` (optional)
    *   Testkörper.
*   **`condition`**: `boolean` (optional)
    *   Test wird als "fixme" markiert, wenn die Bedingung `true` ist.
*   **`callback`**: `function(Fixtures):boolean` (optional)
    *   Eine Funktion, die basierend auf Test-Fixtures zurückgibt, ob der Test als "fixme" markiert werden soll.
*   **`description`**: `string` (optional)
    *   Optionale Beschreibung, die im Testbericht angezeigt wird.

---

## `test.info` (Methode)
_Hinzugefügt in: v1.10_

Gibt Informationen über den aktuell laufenden Test zurück. Diese Methode kann nur während der Testausführung aufgerufen werden, andernfalls wird ein Fehler ausgelöst.

### Usage
```javascript
test('example test', async ({ page }, testInfo) => { // testInfo ist hier verfügbar
  // ...
  await testInfo.attach('screenshot', { // test.info() ist eine Alternative
    body: await page.screenshot(),
    contentType: 'image/png',
  });
});

// Alternative, wenn testInfo nicht direkt als Parameter übergeben wurde:
test('another example', async ({ page }) => {
  const currentTestInfo = test.info();
  await currentTestInfo.attach('log', { body: 'Some log data', contentType: 'text/plain' });
});
```

### Returns
*   `TestInfo`

---

## `test.only` (Methode)
_Hinzugefügt in: v1.10_

Deklariert einen fokussierten Test. Wenn es fokussierte Tests oder Suiten gibt, werden nur diese ausgeführt.

### Signaturen
```javascript
test.only(title, body)
test.only(title, details, body)
```

### Usage
```javascript
test.only('focus this test', async ({ page }) => {
  // Nur fokussierte Tests im gesamten Projekt ausführen.
});
```

### Argumente
*   **`title`**: `string`
    *   Testtitel.
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_
    *   Siehe `test()` für Detailbeschreibung (tag, annotation).
    *   **`tag`**: `string | Array<string>` (optional)
    *   **`annotation`**: `Object | Array<Object>` (optional)
        *   **`type`**: `string`
        *   **`description`**: `string` (optional)
*   **`body`**: `function(Fixtures, TestInfo)`
    *   Testkörper.

---

## `test.setTimeout` (Methode)
_Hinzugefügt in: v1.10_

Ändert das Timeout für den Test. Null bedeutet kein Timeout. [Mehr über verschiedene Timeouts](https://playwright.dev/docs/test-timeouts).
Das Timeout für den aktuell laufenden Test ist über `testInfo.timeout` verfügbar.

### Usage
**Timeout eines Tests ändern:**
```javascript
test('very slow test', async ({ page }) => {
  test.setTimeout(120000); // 120 Sekunden
  // ...
});
```

**Timeout von einem langsamen `beforeEach`-Hook ändern:**
Beachte, dass dies das Test-Timeout beeinflusst, das mit `beforeEach`-Hooks geteilt wird.
```javascript
test.beforeEach(async ({ page }, testInfo) => {
  // Timeout für alle Tests, die diesen Hook ausführen, um 30 Sekunden verlängern.
  test.setTimeout(testInfo.timeout + 30000);
});
```

**Timeout für einen `beforeAll`- oder `afterAll`-Hook ändern:**
Beachte, dass dies das Timeout des Hooks beeinflusst, nicht das Test-Timeout.
```javascript
test.beforeAll(async () => {
  // Timeout für diesen Hook setzen.
  test.setTimeout(60000);
});
```

**Timeout für alle Tests in einer `test.describe()`-Gruppe ändern:**
```javascript
test.describe('group', () => {
  // Gilt für alle Tests in dieser Gruppe.
  test.describe.configure({ timeout: 60000 });

  test('test one', async () => { /* ... */ });
  test('test two', async () => { /* ... */ });
  test('test three', async () => { /* ... */ });
});
```

### Argumente
*   **`timeout`**: `number`
    *   Timeout in Millisekunden.

---

## `test.skip` (Methode)
_Hinzugefügt in: v1.10_

Überspringt einen Test. Playwright führt den Test nach dem Aufruf von `test.skip()` nicht weiter aus.
Übersprungene Tests sollen niemals ausgeführt werden. Wenn die Absicht besteht, den Test zu beheben, verwende stattdessen `test.fixme()`.

### Signaturen
**Um einen übersprungenen Test zu deklarieren:**
```javascript
test.skip(title, body)
test.skip(title, details, body)
```
**Um einen Test zur Laufzeit zu überspringen:**
```javascript
test.skip(condition, description)
test.skip(callback, description)
test.skip()
```

### Usage
**Deklarieren eines übersprungenen Tests:**
```javascript
import { test, expect } from '@playwright/test';

test.skip('never run', async ({ page }) => {
  // ...
});
```

**Bedingtes Überspringen im Testkörper:**
Playwright führt den Test aus, bricht ihn aber sofort nach dem `test.skip`-Aufruf ab.
```javascript
import { test, expect } from '@playwright/test';

test('Safari-only test', async ({ page, browserName }) => {
  test.skip(browserName !== 'webkit', 'This feature is Safari-only');
  // ...
});
```

**Bedingtes Überspringen aller Tests in einer Datei oder `test.describe()`-Gruppe:**
```javascript
import { test, expect } from '@playwright/test';

test.skip(({ browserName }) => browserName !== 'webkit', 'Safari-only');

test('Safari-only test 1', async ({ page }) => {
  // ...
});
test('Safari-only test 2', async ({ page }) => {
  // ...
});
```

**`test.skip()` ohne Argumente (weniger empfohlen):**
```javascript
import { test, expect } from '@playwright/test';

test('less readable', async ({ page }) => {
  test.skip();
  // ...
});
```

### Argumente
*   **`title`**: `string` (optional)
    *   Testtitel.
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_
    *   Siehe `test()` für Detailbeschreibung (tag, annotation).
    *   **`tag`**: `string | Array<string>` (optional)
    *   **`annotation`**: `Object | Array<Object>` (optional)
        *   **`type`**: `string`
        *   **`description`**: `string` (optional)
*   **`body`**: `function(Fixtures, TestInfo)` (optional)
    *   Testkörper.
*   **`condition`**: `boolean` (optional)
    *   Test wird übersprungen, wenn die Bedingung `true` ist. (Hinweis: Text sagt "should fail", ist aber für `skip`)
*   **`callback`**: `function(Fixtures):boolean` (optional)
    *   Eine Funktion, die basierend auf Test-Fixtures zurückgibt, ob der Test übersprungen werden soll. (Hinweis: Text sagt "should fail", ist aber für `skip`)
*   **`description`**: `string` (optional)
    *   Optionale Beschreibung, die im Testbericht angezeigt wird.

---

## `test.slow` (Methode)
_Hinzugefügt in: v1.10_

Markiert einen Test als "langsam". Langsame Tests erhalten das dreifache des Standard-Timeouts.
> **Hinweis:** `test.slow()` kann nicht in einem `beforeAll`- oder `afterAll`-Hook verwendet werden. Verwende stattdessen `test.setTimeout()`.

### Signaturen
```javascript
test.slow()
test.slow(condition, description)
test.slow(callback, description)
```

### Usage
**Einen Test als langsam markieren:**
```javascript
import { test, expect } from '@playwright/test';

test('slow test', async ({ page }) => {
  test.slow();
  // ...
});
```

**Bedingtes Markieren als langsam:**
```javascript
import { test, expect } from '@playwright/test';

test('slow in Safari', async ({ page, browserName }) => {
  test.slow(browserName === 'webkit', 'This feature is slow in Safari');
  // ...
});
```

**Alle Tests in einer Datei oder `test.describe()`-Gruppe bedingt als langsam markieren:**
```javascript
import { test, expect } from '@playwright/test';

test.slow(({ browserName }) => browserName === 'webkit', 'all tests are slow in Safari');

test('slow in Safari 1', async ({ page }) => {
  // ...
});
test('slow in Safari 2', async ({ page }) => { // Der Text sagt "fail in Safari 2", sollte aber "slow in Safari 2" sein, analog zum Beispiel
  // ...
});
```

### Argumente
*   **`condition`**: `boolean` (optional)
    *   Test wird als "langsam" markiert, wenn die Bedingung `true` ist.
*   **`callback`**: `function(Fixtures):boolean` (optional)
    *   Eine Funktion, die basierend auf Test-Fixtures zurückgibt, ob der Test als "langsam" markiert werden soll.
*   **`description`**: `string` (optional)
    *   Optionale Beschreibung, die im Testbericht angezeigt wird.

---

## `test.step` (Methode)
_Hinzugefügt in: v1.10_

Deklariert einen Testschritt, der im Bericht angezeigt wird.

### Usage
```javascript
import { test, expect } from '@playwright/test';

test('test', async ({ page }) => {
  await test.step('Log in', async () => {
    // ...
  });

  await test.step('Outer step', async () => {
    // ...
    // Schritte können verschachtelt werden.
    await test.step('Inner step', async () => {
      // ...
    });
  });
});
```

### Argumente
*   **`title`**: `string`
    *   Schrittname.
*   **`body`**: `function(TestStepInfo):Promise<Object>` (Der Typ im Text ist `Promise<Object>`, sollte aber eher `Promise<T>` oder `Promise<any>` sein, da der Rückgabewert vom Body abhängt. `TestStepInfo` wird dem Body übergeben.)
    *   Schrittkörper.
*   **`options`**: `Object` (optional)
    *   **`box`**: `boolean` (optional) _Hinzugefügt in: v1.39_
        *   Ob der Schritt im Bericht eingerahmt werden soll. Standard: `false`. Wenn eingerahmt, zeigen Fehler aus dem Inneren des Schritts auf die Aufrufstelle des Schritts.
    *   **`location`**: `Location` (optional) _Hinzugefügt in: v1.48_
        *   Gibt einen benutzerdefinierten Ort für den Schritt an, der in Testberichten und im Trace Viewer angezeigt wird. Standardmäßig wird der Ort des `test.step()`-Aufrufs angezeigt.
    *   **`timeout`**: `number` (optional) _Hinzugefügt in: v1.50_
        *   Die maximale Zeit in Millisekunden, die der Schritt zur Fertigstellung hat. Wenn der Schritt nicht innerhalb des angegebenen Timeouts abgeschlossen wird, löst die Methode `test.step()` einen `TimeoutError` aus. Standard: `0` (kein Timeout).

### Returns
*   `Promise<Object>` (Der Rückgabewert des Schritt-Callbacks)

### Details
Die Methode gibt den Wert zurück, den der Schritt-Callback zurückgibt.
```javascript
import { test, expect } from '@playwright/test';

test('test', async ({ page }) => {
  const user = await test.step('Log in', async () => {
    // ...
    return 'john';
  });
  expect(user).toBe('john');
});
```

### Decorator
TypeScript-Methodendekoratoren können verwendet werden, um eine Methode in einen Schritt umzuwandeln.
```typescript
import { test, expect, Page } from '@playwright/test'; // Page importiert

function step(target: Function, context: ClassMethodDecoratorContext) {
  return function replacementMethod(this: any, ...args: any) { // this explizit typisiert
    const name = this.constructor.name + '.' + (context.name as string);
    return test.step(name, async () => {
      return await target.apply(this, args); // target.call(this, ...args) ist auch ok
    });
  };
}

class LoginPage {
  constructor(readonly page: Page) {}

  @step
  async login() {
    const account = { username: 'Alice', password: 's3cr3t' };
    await this.page.getByLabel('Username or email address').fill(account.username);
    await this.page.getByLabel('Password').fill(account.password);
    await this.page.getByRole('button', { name: 'Sign in' }).click();
    await expect(this.page.getByRole('button', { name: 'View profile and more' })).toBeVisible();
  }
}

test('example', async ({ page }) => {
  const loginPage = new LoginPage(page);
  await loginPage.login();
});
```

### Boxing
Wenn etwas innerhalb eines Schritts fehlschlägt, wird der Fehler normalerweise auf die genaue fehlgeschlagene Aktion zeigen. Mit der Option `box: true` zeigt der Fehler stattdessen auf die Aufrufstelle des Schritts.

**Ohne `box: true` (Fehler im Schritt):**
```
Error: Timed out 5000ms waiting for expect(locator).toBeVisible()
   8 |     await page.getByRole('button', { name: 'Sign in' }).click();
>  9 |     await expect(page.getByRole('button', { name: 'View profile and more' })).toBeVisible();
     |                                                                               ^
  10 |   });
```

**Mit `box: true` (Fehler an der Aufrufstelle):**
```javascript
async function login(page: Page) { // page typisiert
  await test.step('login', async () => {
    // ... (wie oben)
  }, { box: true }); // "box"-Option hier
}
```
```
Error: Timed out 5000ms waiting for expect(locator).toBeVisible()
  14 |   await page.goto('https://github.com/login');
> 15 |   await login(page);
     |         ^
  16 | });
```

**Decorator für einen `boxedStep`:**
```typescript
import { test, expect, Page } from '@playwright/test'; // Page importiert

function boxedStep(target: Function, context: ClassMethodDecoratorContext) {
  return function replacementMethod(this: any, ...args: any) { // this explizit typisiert
    const name = this.constructor.name + '.' + (context.name as string);
    return test.step(name, async () => {
      return await target.apply(this, args);
    }, { box: true }); // "box"-Option hier
  };
}

class LoginPage {
  constructor(readonly page: Page) {}

  @boxedStep
  async login() {
    // ....
  }
}

test('example', async ({ page }) => {
  const loginPage = new LoginPage(page);
  await loginPage.login();  // <-- Fehler wird auf dieser Zeile gemeldet.
});
```

---

## `test.step.skip` (Methode)
_Hinzugefügt in: v1.50_

Markiert einen Testschritt als "übersprungen", um seine Ausführung vorübergehend zu deaktivieren. Nützlich für Schritte, die derzeit fehlschlagen und für eine kurzfristige Korrektur geplant sind. Playwright wird den Schritt nicht ausführen. Siehe auch `testStepInfo.skip()`.
> **Empfehlung:** `testStepInfo.skip()` wird stattdessen empfohlen.

### Usage
```javascript
import { test, expect } from '@playwright/test';

test('my test', async ({ page }) => {
  // ...
  await test.step.skip('not yet ready', async () => {
    // ...
  });
});
```

### Argumente
*   **`title`**: `string`
    *   Schrittname.
*   **`body`**: `function():Promise<Object>` (Der Text sagt `Promise<Object>`, aber da der Schritt übersprungen wird, ist der Rückgabetyp des `body` irrelevant, und die äußere Funktion gibt `Promise<void>` zurück.)
    *   Schrittkörper (wird nicht ausgeführt).
*   **`options`**: `Object` (optional)
    *   **`box`**: `boolean` (optional)
        *   Ob der Schritt im Bericht eingerahmt werden soll. Standard: `false`.
    *   **`location`**: `Location` (optional)
        *   Benutzerdefinierter Ort für den Schritt.
    *   **`timeout`**: `number` (optional)
        *   Maximale Zeit in Millisekunden für die Fertigstellung des Schritts. Standard: `0` (kein Timeout). (Irrelevant, da der Schritt übersprungen wird).

### Returns
*   `Promise<void>`

---

## `test.use` (Methode)
_Hinzugefügt in: v1.10_

Spezifiziert Optionen oder Fixtures, die in einer einzelnen Testdatei oder einer `test.describe()`-Gruppe verwendet werden sollen. Am nützlichsten, um eine Option zu setzen, z.B. `locale`, um die Kontext-Fixture zu konfigurieren.

### Usage
**Option setzen:**
```javascript
import { test, expect } from '@playwright/test';

test.use({ locale: 'en-US' });

test('test with locale', async ({ page }) => {
  // Standardkontext und Seite haben das angegebene Gebietsschema
});
```

**Fixture überschreiben:**
```javascript
import { test, expect } from '@playwright/test';
import fs from 'fs'; // Für das Beispiel benötigt

test.use({
  locale: async ({}, use) => {
    // Gebietsschema aus einer Konfigurationsdatei lesen.
    const locale = await fs.promises.readFile('test-locale.txt', 'utf-8'); // Pfad korrigiert
    await use(locale.trim()); // trim() hinzugefügt, um Whitespace zu entfernen
  },
});

test('test with locale', async ({ page }) => {
  // Standardkontext und Seite haben das angegebene Gebietsschema
});
```

### Argumente
*   **`options`**: `TestOptions`
    *   Ein Objekt mit lokalen Optionen oder Fixture-Überschreibungen.

### Details
*   `test.use` kann entweder im globalen Geltungsbereich oder innerhalb von `test.describe` aufgerufen werden.
*   Es ist ein Fehler, es innerhalb von `beforeEach` oder `beforeAll` aufzurufen.

---

## Eigenschaften

### `test.expect`
_Hinzugefügt in: v1.10_

Die `expect`-Funktion kann verwendet werden, um Test-Assertions zu erstellen. [Mehr über Test-Assertions](https://playwright.dev/docs/test-assertions).

#### Usage
```javascript
test('example', async ({ page }) => {
  await test.expect(page).toHaveTitle('Title');
});
```
> **Hinweis:** `test.expect` ist identisch mit dem direkten Import von `expect`.

#### Type
*   `Object` (eigentlich eine Funktion, die ein Assertion-Objekt zurückgibt)

---

## Veraltete Methoden (Deprecated)

Die folgenden Methoden sind veraltet. Verwende stattdessen `test.describe.configure()` für die Konfiguration des Ausführungsmodus.

### `test.describe.parallel`
_Hinzugefügt in: v1.10_
> **Veraltet:** Siehe `test.describe.configure({ mode: 'parallel' })`.

Deklariert eine Gruppe von Tests, die parallel ausgeführt werden könnten.

#### Signaturen
```javascript
test.describe.parallel(title, callback)
test.describe.parallel(callback)
test.describe.parallel(title, details, callback)
```

#### Usage
```javascript
test.describe.parallel('group', () => {
  test('runs in parallel 1', async ({ page }) => {});
  test('runs in parallel 2', async ({ page }) => {});
});
```
Parallele Tests werden in separaten Prozessen ausgeführt und können keinen Zustand oder globale Variablen teilen.

#### Argumente
*   **`title`**: `string` (optional)
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_ (tag, annotation)
*   **`callback`**: `function`

---

### `test.describe.parallel.only`
_Hinzugefügt in: v1.10_
> **Veraltet:** Kombiniere `test.describe.only()` mit `test.describe.configure({ mode: 'parallel' })`.

Deklariert eine fokussierte Gruppe von Tests, die parallel ausgeführt werden könnten.

#### Signaturen
```javascript
test.describe.parallel.only(title, callback)
test.describe.parallel.only(callback)
test.describe.parallel.only(title, details, callback)
```

#### Usage
```javascript
test.describe.parallel.only('group', () => {
  test('runs in parallel 1', async ({ page }) => {});
  test('runs in parallel 2', async ({ page }) => {});
});
```

#### Argumente
*   **`title`**: `string` (optional)
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_ (tag, annotation)
*   **`callback`**: `function`

---

### `test.describe.serial`
_Hinzugefügt in: v1.10_
> **Veraltet:** Siehe `test.describe.configure({ mode: 'serial' })`.

Deklariert eine Gruppe von Tests, die immer seriell ausgeführt werden sollten. Wenn einer der Tests fehlschlägt, werden alle nachfolgenden Tests übersprungen. Alle Tests in einer Gruppe werden zusammen wiederholt.
> **Hinweis:** Die serielle Ausführung wird nicht empfohlen.

#### Signaturen
```javascript
test.describe.serial(title, callback)
test.describe.serial(title) // Wahrscheinlich ein Tippfehler in der Doku, sollte callback haben oder Details.
test.describe.serial(title, details, callback)
// Implizit auch test.describe.serial(callback)
```
Ich nehme die sinnvollen Signaturen:
```javascript
test.describe.serial(title, callback)
test.describe.serial(callback)
test.describe.serial(title, details, callback)
```

#### Usage
```javascript
test.describe.serial('group', () => {
  test('runs first', async ({ page }) => {});
  test('runs second', async ({ page }) => {});
});
```

#### Argumente
*   **`title`**: `string` (optional)
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_ (tag, annotation)
*   **`callback`**: `function`

---

### `test.describe.serial.only`
_Hinzugefügt in: v1.10_
> **Veraltet:** Kombiniere `test.describe.only()` mit `test.describe.configure({ mode: 'serial' })`.

Deklariert eine fokussierte Gruppe von Tests, die immer seriell ausgeführt werden sollten.
> **Hinweis:** Die serielle Ausführung wird nicht empfohlen.

#### Signaturen
```javascript
test.describe.serial.only(title, callback)
test.describe.serial.only(title) // Wahrscheinlich ein Tippfehler in der Doku.
test.describe.serial.only(title, details, callback)
// Implizit auch test.describe.serial.only(callback)
```
Ich nehme die sinnvollen Signaturen:
```javascript
test.describe.serial.only(title, callback)
test.describe.serial.only(callback)
test.describe.serial.only(title, details, callback)
```

#### Usage
```javascript
test.describe.serial.only('group', () => {
  test('runs first', async ({ page }) => {});
  test('runs second', async ({ page }) => {});
});
```

#### Argumente
*   **`title`**: `string` (optional, aber für die `(title)` Variante erforderlich)
*   **`details`**: `Object` (optional) _Hinzugefügt in: v1.42_ (tag, annotation)
*   **`callback`**: `function`



</details>
