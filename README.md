# Playwright Cheat Sheet
Playwright Cheat Sheet with the most needed stuff..


<br><br>

# DOCS
- https://playwright.dev/docs/intro

<br><br>









































<br><br>
_________________________________________________
_________________________________________________
<br><br>






# API
- https://playwright.dev/docs/api/class-playwright


<details><summary>Click to expand..</summary>


<br><br>

## Locator


<details><summary>Click to expand..</summary>


Locators sind das Herzstück von Playwrights Auto-Waiting und Retry-Fähigkeiten. Sie stellen eine Methode dar, um Elemente auf der Seite jederzeit zu finden. Ein Locator wird mit `page.locator()` erstellt.

---

## Methoden

### `all()`
*(Hinzugefügt in v1.29)*
Gibt ein Array von Locators zurück, die auf die jeweiligen Elemente einer Liste zeigen.
**Wichtig:** Wartet nicht, bis Elemente dem Locator entsprechen, sondern gibt sofort zurück, was vorhanden ist. Kann bei dynamischen Listen unvorhersehbar sein.

```javascript
for (const li of await page.getByRole('listitem').all()) {
  await li.click();
}
// Gibt zurück: Promise<Array<Locator>>
```

---

### `allInnerTexts()`
*(Hinzugefügt in v1.14)*
Gibt ein Array der `node.innerText`-Werte für alle übereinstimmenden Elemente zurück.
**Hinweis:** Für Assertions `expect(locator).toHaveText()` mit `useInnerText` verwenden.

```javascript
const texts = await page.getByRole('link').allInnerTexts();
// Gibt zurück: Promise<Array<string>>
```

---

### `allTextContents()`
*(Hinzugefügt in v1.14)*
Gibt ein Array der `node.textContent`-Werte für alle übereinstimmenden Elemente zurück.
**Hinweis:** Für Assertions `expect(locator).toHaveText()` verwenden.

```javascript
const texts = await page.getByRole('link').allTextContents();
// Gibt zurück: Promise<Array<string>>
```

---

### `and(locator)`
*(Hinzugefügt in v1.34)*
Erstellt einen Locator, der sowohl diesem Locator als auch dem Argument-Locator entspricht.

```javascript
const button = page.getByRole('button').and(page.getByTitle('Subscribe'));
// Gibt zurück: Locator
```

---

### `ariaSnapshot([options])`
*(Hinzugefügt in v1.49)*
Erfasst den ARIA-Snapshot des Elements (im YAML-Format).
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await page.getByRole('link').ariaSnapshot();
// Gibt zurück: Promise<string>
```

---

### `blur([options])`
*(Hinzugefügt in v1.28)*
Ruft `blur` auf dem Element auf.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await locator.blur();
// Gibt zurück: Promise<void>
```

---

### `boundingBox([options])`
*(Hinzugefügt in v1.14)*
Gibt die Bounding Box (Position und Größe) des Elements zurück, oder `null`, wenn nicht sichtbar. Relativ zum Viewport des Hauptframes.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
const box = await page.getByRole('button').boundingBox();
// if (box) { await page.mouse.click(box.x + box.width / 2, box.y + box.height / 2); }
// Gibt zurück: Promise<null | { x: number, y: number, width: number, height: number }>
```

---

### `check([options])`
*(Hinzugefügt in v1.14)*
Stellt sicher, dass ein Checkbox- oder Radio-Element ausgewählt ist.
- `options.force`: Umgeht Actionability-Checks.
- `options.position`: `{ x: number, y: number }` Klickposition relativ zur oberen linken Ecke.
- `options.timeout`: Maximale Wartezeit in ms.
- `options.trial`: Nur Actionability-Checks durchführen, keine Aktion.

```javascript
await page.getByRole('checkbox').check();
// Gibt zurück: Promise<void>
```

---

### `clear([options])`
*(Hinzugefügt in v1.28)*
Löscht den Inhalt eines Eingabefeldes.
- `options.force`: Umgeht Actionability-Checks.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await page.getByRole('textbox').clear();
// Gibt zurück: Promise<void>
```

---

### `click([options])`
*(Hinzugefügt in v1.14)*
Klickt auf ein Element.
- `options.button`: `"left"` (Standard), `"right"`, `"middle"`.
- `options.clickCount`: Anzahl der Klicks (Standard: 1).
- `options.delay`: Zeit in ms zwischen `mousedown` und `mouseup` (Standard: 0).
- `options.force`: Umgeht Actionability-Checks.
- `options.modifiers`: Array von Tasten wie `["Shift", "Control"]`.
- `options.noWaitAfter`: **VERALTET** (wird zukünftig `true`). Nicht auf Navigationen warten.
- `options.position`: `{ x: number, y: number }` Klickposition.
- `options.timeout`: Maximale Wartezeit in ms.
- `options.trial`: Nur Actionability-Checks.

```javascript
await page.getByRole('button').click();
await page.locator('canvas').click({ button: 'right', modifiers: ['Shift'] });
// Gibt zurück: Promise<void>
```

---

### `contentFrame()`
*(Hinzugefügt in v1.43)*
Gibt einen `FrameLocator` zurück, der auf denselben iFrame zeigt wie dieser Locator.

```javascript
const frameLocator = page.locator('iframe[name="embedded"]').contentFrame();
await frameLocator.getByRole('button').click();
// Gibt zurück: FrameLocator
```

---

### `count()`
*(Hinzugefügt in v1.14)*
Gibt die Anzahl der Elemente zurück, die dem Locator entsprechen.
**Hinweis:** Für Assertions `expect(locator).toHaveCount()` verwenden.

```javascript
const count = await page.getByRole('listitem').count();
// Gibt zurück: Promise<number>
```

---

### `dblclick([options])`
*(Hinzugefügt in v1.14)*
Doppelklickt auf ein Element. Optionen ähnlich wie `click()`.

```javascript
await locator.dblclick();
// Gibt zurück: Promise<void>
```

---

### `describe(description)`
*(Hinzugefügt in v1.53)*
Beschreibt den Locator für Trace Viewer und Reports. Gibt den Locator selbst zurück.

```javascript
locator.describe('Login Button');
// Gibt zurück: Locator
```

---

### `dispatchEvent(type[, eventInit, options])`
*(Hinzugefügt in v1.14)*
Löst ein DOM-Event programmatisch auf dem Element aus.
- `type`: Event-Typ (z.B. `"click"`, `"dragstart"`).
- `eventInit`: Event-spezifische Initialisierungseigenschaften.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await locator.dispatchEvent('click');
// Gibt zurück: Promise<void>
```

---

### `dragTo(targetLocator[, options])`
*(Hinzugefügt in v1.18)*
Zieht das Quell-Element zum Ziel-Element und lässt es dort fallen.
- `targetLocator`: Locator des Zielelements.
- `options.force`: Umgeht Actionability-Checks.
- `options.sourcePosition`: `{ x, y }` Startpunkt auf Quellelement.
- `options.targetPosition`: `{ x, y }` Endpunkt auf Zielelement.
- `options.timeout`: Maximale Wartezeit in ms.
- `options.trial`: Nur Actionability-Checks.

```javascript
await sourceLocator.dragTo(targetLocator);
// Gibt zurück: Promise<void>
```

---

### `evaluate(pageFunction[, arg, options])`
*(Hinzugefügt in v1.14)*
Führt JavaScript-Code im Seitenkontext aus, wobei das übereinstimmende Element als Argument übergeben wird.
- `pageFunction`: Funktion, die im Browser ausgeführt wird `(element, arg) => { ... }`.
- `arg`: Optionales Argument für `pageFunction`.
- `options.timeout`: Maximale Wartezeit auf den Locator in ms.

```javascript
const text = await locator.evaluate(element => element.textContent);
// Gibt zurück: Promise<Serializable>
```

---

### `evaluateAll(pageFunction[, arg])`
*(Hinzugefügt in v1.14)*
Führt JavaScript-Code im Seitenkontext aus, wobei ein Array aller übereinstimmenden Elemente als Argument übergeben wird.
- `pageFunction`: Funktion `(elements, arg) => { ... }`.
- `arg`: Optionales Argument für `pageFunction`.

```javascript
const count = await locator.evaluateAll(elements => elements.length);
// Gibt zurück: Promise<Serializable>
```

---

### `evaluateHandle(pageFunction[, arg, options])`
*(Hinzugefügt in v1.14)*
Wie `evaluate`, gibt aber ein `JSHandle` mit dem Ergebnis zurück.
- `options.timeout`: Maximale Wartezeit auf den Locator in ms.

```javascript
const elementHandle = await locator.evaluateHandle(element => element);
// Gibt zurück: Promise<JSHandle>
```

---

### `fill(value[, options])`
*(Hinzugefügt in v1.14)*
Setzt einen Wert in ein Eingabefeld (`<input>`, `<textarea>`, `[contenteditable]`).
- `value`: Einzugebender Text.
- `options.force`: Umgeht Actionability-Checks.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await page.getByRole('textbox').fill('Hallo Welt');
// Gibt zurück: Promise<void>
```

---

### `filter([options])`
*(Hinzugefügt in v1.22)*
Schränkt einen bestehenden Locator basierend auf Optionen ein.
- `options.has`: Locator, der innerhalb des aktuellen Elements gefunden werden muss.
- `options.hasNot`: Locator, der NICHT innerhalb des aktuellen Elements gefunden werden darf. *(v1.33)*
- `options.hasText`: Text (string|RegExp), der im Element enthalten sein muss.
- `options.hasNotText`: Text (string|RegExp), der NICHT im Element enthalten sein darf. *(v1.33)*
- `options.visible`: `boolean` *(v1.51)* Nur sichtbare/unsichtbare Elemente.

```javascript
locator.filter({ hasText: 'Login' }).filter({ has: page.getByRole('button') });
// Gibt zurück: Locator
```

---

### `first()`
*(Hinzugefügt in v1.14)*
Gibt einen Locator für das erste übereinstimmende Element zurück.

```javascript
const firstItem = page.getByRole('listitem').first();
// Gibt zurück: Locator
```

---

### `focus([options])`
*(Hinzugefügt in v1.14)*
Setzt den Fokus auf das übereinstimmende Element.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await locator.focus();
// Gibt zurück: Promise<void>
```

---

### `frameLocator(selector)`
*(Hinzugefügt in v1.17)*
Erstellt einen `FrameLocator`, um Elemente innerhalb eines iFrames zu lokalisieren. (Dies ist eine Methode von `Page` oder `FrameLocator`, nicht von `Locator` selbst, aber wichtig im Kontext).

```javascript
const frame = page.frameLocator('iframe#myFrame');
await frame.getByText('Submit').click();
// Gibt zurück: FrameLocator
```

---

### `getAttribute(name[, options])`
*(Hinzugefügt in v1.14)*
Gibt den Attributwert des übereinstimmenden Elements zurück.
**Hinweis:** Für Assertions `expect(locator).toHaveAttribute()` verwenden.
- `name`: Attributname.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
const href = await locator.getAttribute('href');
// Gibt zurück: Promise<null | string>
```

---

### `getByAltText(text[, options])`
*(Hinzugefügt in v1.27)*
Findet Elemente anhand ihres `alt`-Textes (z.B. bei `<img>`).
- `text`: `string | RegExp`.
- `options.exact`: `boolean` (Standard: `false`) für exakte Übereinstimmung (Groß-/Kleinschreibung, ganzer String).

```javascript
await page.getByAltText('Playwright logo').click();
// Gibt zurück: Locator
```

---

### `getByLabel(text[, options])`
*(Hinzugefügt in v1.27)*
Findet Input-Elemente anhand des Textes des zugehörigen `<label>` oder `aria-labelledby` / `aria-label`.
- `text`: `string | RegExp`.
- `options.exact`: `boolean` (Standard: `false`).

```javascript
await page.getByLabel('Username').fill('john_doe');
// Gibt zurück: Locator
```

---

### `getByPlaceholder(text[, options])`
*(Hinzugefügt in v1.27)*
Findet Input-Elemente anhand ihres `placeholder`-Textes.
- `text`: `string | RegExp`.
- `options.exact`: `boolean` (Standard: `false`).

```javascript
await page.getByPlaceholder('name@example.com').fill('test@example.com');
// Gibt zurück: Locator
```

---

### `getByRole(role[, options])`
*(Hinzugefügt in v1.27)*
Findet Elemente anhand ihrer ARIA-Rolle, ARIA-Attribute und ihres zugänglichen Namens.
- `role`: ARIA-Rolle (z.B. `"button"`, `"checkbox"`, `"heading"`).
- `options.name`: `string | RegExp` (zugänglicher Name).
- `options.checked`: `boolean` (Status für Checkbox/Radio).
- `options.disabled`: `boolean`.
- `options.exact`: `boolean` (für `name`, Standard: `false`). *(v1.28)*
- `options.expanded`: `boolean`.
- `options.includeHidden`: `boolean` (ob versteckte Elemente berücksichtigt werden).
- `options.level`: `number` (z.B. für Überschriften `<h1>`-`<h6>`).
- `options.pressed`: `boolean`.
- `options.selected`: `boolean`.

```javascript
await page.getByRole('button', { name: /Submit/i }).click();
// Gibt zurück: Locator
```

---

### `getByTestId(testId)`
*(Hinzugefügt in v1.27)*
Findet Elemente anhand ihrer Test-ID (Standardattribut: `data-testid`).
- `testId`: `string | RegExp`.
**Hinweis:** Test-ID-Attribut kann via `selectors.setTestIdAttribute()` konfiguriert werden.

```javascript
await page.getByTestId('submit-button').click();
// Gibt zurück: Locator
```

---

### `getByText(text[, options])`
*(Hinzugefügt in v1.27)*
Findet Elemente, die den angegebenen Text enthalten. Whitespace wird normalisiert.
- `text`: `string | RegExp`.
- `options.exact`: `boolean` (Standard: `false`).

```javascript
await page.getByText('Hello world').click();
await page.getByText('Hello', { exact: true }); // findet nur "Hello"
// Gibt zurück: Locator
```

---

### `getByTitle(text[, options])`
*(Hinzugefügt in v1.27)*
Findet Elemente anhand ihres `title`-Attributs.
- `text`: `string | RegExp`.
- `options.exact`: `boolean` (Standard: `false`).

```javascript
await expect(page.getByTitle('Issues count')).toHaveText('25 issues');
// Gibt zurück: Locator
```

---

### `highlight()`
*(Hinzugefügt in v1.20)*
Hebt das/die entsprechende(n) Element(e) auf dem Bildschirm hervor. Nützlich zum Debuggen.

```javascript
await locator.highlight();
// Gibt zurück: Promise<void>
```

---

### `hover([options])`
*(Hinzugefügt in v1.14)*
Bewegt den Mauszeiger über das übereinstimmende Element.
- `options.force`: Umgeht Actionability-Checks.
- `options.modifiers`: Array von Tasten.
- `options.position`: `{ x, y }` Hover-Position.
- `options.timeout`: Maximale Wartezeit in ms.
- `options.trial`: Nur Actionability-Checks.

```javascript
await page.getByRole('link').hover();
// Gibt zurück: Promise<void>
```

---

### `innerHTML([options])`
*(Hinzugefügt in v1.14)*
Gibt `element.innerHTML` zurück.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
const html = await locator.innerHTML();
// Gibt zurück: Promise<string>
```

---

### `innerText([options])`
*(Hinzugefügt in v1.14)*
Gibt `element.innerText` zurück.
**Hinweis:** Für Assertions `expect(locator).toHaveText({ useInnerText: true })` verwenden.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
const text = await locator.innerText();
// Gibt zurück: Promise<string>
```

---

### `inputValue([options])`
*(Hinzugefügt in v1.14)*
Gibt den Wert eines `<input>`, `<textarea>` oder `<select>` Elements zurück.
**Hinweis:** Für Assertions `expect(locator).toHaveValue()` verwenden.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
const value = await page.getByRole('textbox').inputValue();
// Gibt zurück: Promise<string>
```

---

### `isChecked([options])`
*(Hinzugefügt in v1.14)*
Gibt zurück, ob das Element (Checkbox/Radio) ausgewählt ist.
**Hinweis:** Für Assertions `expect(locator).toBeChecked()` verwenden.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
const checked = await page.getByRole('checkbox').isChecked();
// Gibt zurück: Promise<boolean>
```

---

### `isDisabled([options])`
*(Hinzugefügt in v1.14)*
Gibt zurück, ob das Element deaktiviert ist.
**Hinweis:** Für Assertions `expect(locator).toBeDisabled()` verwenden.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
const disabled = await page.getByRole('button').isDisabled();
// Gibt zurück: Promise<boolean>
```

---

### `isEditable([options])`
*(Hinzugefügt in v1.14)*
Gibt zurück, ob das Element editierbar ist.
**Hinweis:** Für Assertions `expect(locator).toBeEditable()` verwenden.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
const editable = await page.getByRole('textbox').isEditable();
// Gibt zurück: Promise<boolean>
```

---

### `isEnabled([options])`
*(Hinzugefügt in v1.14)*
Gibt zurück, ob das Element aktiviert ist.
**Hinweis:** Für Assertions `expect(locator).toBeEnabled()` verwenden.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
const enabled = await page.getByRole('button').isEnabled();
// Gibt zurück: Promise<boolean>
```

---

### `isHidden([options])`
*(Hinzugefügt in v1.14)*
Gibt zurück, ob das Element versteckt ist. Wartet nicht.
**Hinweis:** Für Assertions `expect(locator).toBeHidden()` verwenden.
- `options.timeout`: **VERALTET** / Ignoriert.

```javascript
const hidden = await page.getByRole('button').isHidden();
// Gibt zurück: Promise<boolean>
```

---

### `isVisible([options])`
*(Hinzugefügt in v1.14)*
Gibt zurück, ob das Element sichtbar ist. Wartet nicht.
**Hinweis:** Für Assertions `expect(locator).toBeVisible()` verwenden.
- `options.timeout`: **VERALTET** / Ignoriert.

```javascript
const visible = await page.getByRole('button').isVisible();
// Gibt zurück: Promise<boolean>
```

---

### `last()`
*(Hinzugefügt in v1.14)*
Gibt einen Locator für das letzte übereinstimmende Element zurück.

```javascript
const lastItem = page.getByRole('listitem').last();
// Gibt zurück: Locator
```

---

### `locator(selectorOrLocator[, options])`
*(Hinzugefügt in v1.14)*
Findet ein Element, das dem angegebenen Selektor/Locator im Unterbaum des aktuellen Locators entspricht. Akzeptiert Filteroptionen wie `filter()`.
- `selectorOrLocator`: `string | Locator`.
- `options.has`, `options.hasNot`, `options.hasText`, `options.hasNotText`: Siehe `filter()`.

```javascript
const row = page.locator('tr:has-text("Produkt A")');
const cellInRow = row.locator('td').nth(2);
// Gibt zurück: Locator
```

---

### `nth(index)`
*(Hinzugefügt in v1.14)*
Gibt einen Locator für das n-te übereinstimmende Element zurück (0-basiert).

```javascript
const thirdItem = page.getByRole('listitem').nth(2);
// Gibt zurück: Locator
```

---

### `or(locator)`
*(Hinzugefügt in v1.33)*
Erstellt einen Locator, der Elemente findet, die entweder diesem oder dem alternativen Locator entsprechen.
**Achtung:** Kann zu Strict-Mode-Verletzungen führen, wenn beide Locators Elemente finden. Oft mit `.first()` kombinieren.

```javascript
const newEmail = page.getByRole('button', { name: 'New' });
const dialog = page.getByText('Confirm security settings');
await expect(newEmail.or(dialog).first()).toBeVisible();
// Gibt zurück: Locator
```

---

### `page()`
*(Hinzugefügt in v1.19)*
Gibt die Seite zurück, zu der dieser Locator gehört.

```javascript
const currentPage = locator.page();
// Gibt zurück: Page
```

---

### `press(key[, options])`
*(Hinzugefügt in v1.14)*
Fokussiert das Element und drückt eine Tastenkombination.
- `key`: Taste (z.B. `"ArrowLeft"`, `"a"`, `"Control+Shift+T"`).
- `options.delay`: Verzögerung in ms zwischen `keydown` und `keyup`.
- `options.noWaitAfter`: **VERALTET**.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await page.getByRole('textbox').press('Enter');
// Gibt zurück: Promise<void>
```

---

### `pressSequentially(text[, options])`
*(Hinzugefügt in v1.38)*
Fokussiert das Element und sendet Events für jedes Zeichen im Text.
**Tipp:** Meist `locator.fill()` verwenden. Dies nur bei spezieller Tastaturbehandlung nötig.
- `text`: Zu tippender Text.
- `options.delay`: Verzögerung in ms zwischen Tastendrücken.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await locator.pressSequentially('Hallo');
// Gibt zurück: Promise<void>
```

---

### `screenshot([options])`
*(Hinzugefügt in v1.14)*
Macht einen Screenshot des Elements.
- `options.animations`: `"disabled"` | `"allow"` (Standard).
- `options.caret`: `"hide"` (Standard) | `"initial"`.
- `options.mask`: `Array<Locator>` zum Maskieren von Elementen.
- `options.maskColor`: Farbe für Maskierung (Standard: `#FF00FF`). *(v1.35)*
- `options.omitBackground`: `boolean` für transparente Screenshots (nicht für JPEG).
- `options.path`: Dateipfad zum Speichern.
- `options.quality`: Bildqualität 0-100 (für JPEG).
- `options.scale`: `"css"` | `"device"` (Standard).
- `options.style`: CSS-Stylesheet, das während des Screenshots angewendet wird. *(v1.41)*
- `options.timeout`: Maximale Wartezeit in ms.
- `options.type`: `"png"` (Standard) | `"jpeg"`.

```javascript
await page.getByRole('link').screenshot({ path: 'link.png' });
// Gibt zurück: Promise<Buffer>
```

---

### `scrollIntoViewIfNeeded([options])`
*(Hinzugefügt in v1.14)*
Scrollt das Element in den sichtbaren Bereich, falls es nicht vollständig sichtbar ist.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await locator.scrollIntoViewIfNeeded();
// Gibt zurück: Promise<void>
```

---

### `selectOption(values[, options])`
*(Hinzugefügt in v1.14)*
Wählt eine oder mehrere Optionen in einem `<select>`-Element aus.
- `values`: `string | Array<string> | Object | Array<Object> | null`. Kann `{ value: 'val' }`, `{ label: 'lbl' }`, `{ index: idx }` sein.
- `options.force`: Umgeht Actionability-Checks.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await locator.selectOption('blue'); // Nach Wert oder Label
await locator.selectOption({ label: 'Green' });
await locator.selectOption(['red', { value: 'blue' }]);
// Gibt zurück: Promise<Array<string>> (Array der ausgewählten Werte)
```

---

### `selectText([options])`
*(Hinzugefügt in v1.14)*
Fokussiert das Element und wählt dessen gesamten Textinhalt aus.
- `options.force`: Umgeht Actionability-Checks.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await locator.selectText();
// Gibt zurück: Promise<void>
```

---

### `setChecked(checked[, options])`
*(Hinzugefügt in v1.15)*
Setzt den Zustand eines Checkbox- oder Radio-Elements.
- `checked`: `boolean`.
- `options`: Ähnlich wie `check()`.

```javascript
await page.getByRole('checkbox').setChecked(true);
// Gibt zurück: Promise<void>
```

---

### `setInputFiles(files[, options])`
*(Hinzugefügt in v1.14)*
Lädt eine oder mehrere Dateien in ein `<input type=file>` hoch.
- `files`: `string | Array<string> | Object | Array<Object>`.
  - Objektformat: `{ name: string, mimeType: string, buffer: Buffer }`.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await locator.setInputFiles('pfad/zur/datei.txt');
await locator.setInputFiles([{ name: 'file.txt', mimeType: 'text/plain', buffer: Buffer.from('...') }]);
// Gibt zurück: Promise<void>
```

---

### `tap([options])`
*(Hinzugefügt in v1.14)*
Führt eine Tipp-Geste auf dem Element aus (für Touch-Emulation).
**Hinweis:** Benötigt `hasTouch: true` im Browserkontext.
- `options`: Ähnlich wie `click()`.

```javascript
await locator.tap();
// Gibt zurück: Promise<void>
```

---

### `textContent([options])`
*(Hinzugefügt in v1.14)*
Gibt `node.textContent` zurück.
**Hinweis:** Für Assertions `expect(locator).toHaveText()` verwenden.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
const text = await locator.textContent();
// Gibt zurück: Promise<null | string>
```

---

### `uncheck([options])`
*(Hinzugefügt in v1.14)*
Stellt sicher, dass ein Checkbox- oder Radio-Element nicht ausgewählt ist.
- `options`: Ähnlich wie `check()`.

```javascript
await page.getByRole('checkbox').uncheck();
// Gibt zurück: Promise<void>
```

---

### `waitFor([options])`
*(Hinzugefügt in v1.16)*
Wartet, bis das Element den angegebenen Zustand erfüllt.
- `options.state`: `"attached"` | `"detached"` | `"visible"` (Standard) | `"hidden"`.
- `options.timeout`: Maximale Wartezeit in ms.

```javascript
await locator.waitFor({ state: 'visible' });
// Gibt zurück: Promise<void>
```

---

## Veraltete Methoden

### `elementHandle([options])`
*(Hinzugefügt in v1.14)*
**VERALTET!** Bevorzuge Locators und Web-Assertions.
Löst den Locator zum ersten übereinstimmenden DOM-Element auf.
- `options.timeout`: Maximale Wartezeit in ms.
```javascript
// Veraltet: const handle = await locator.elementHandle();
// Gibt zurück: Promise<ElementHandle>
```

---

### `elementHandles()`
*(Hinzugefügt in v1.14)*
**VERALTET!** Bevorzuge Locators und Web-Assertions.
Löst den Locator zu allen übereinstimmenden DOM-Elementen auf.
```javascript
// Veraltet: const handles = await locator.elementHandles();
// Gibt zurück: Promise<Array<ElementHandle>>
```

---

### `type(text[, options])`
*(Hinzugefügt in v1.14)*
**VERALTET!** In den meisten Fällen `locator.fill()` verwenden. Für spezielle Tastatureingaben `locator.pressSequentially()`.
Fokussiert das Element und sendet Events für jedes Zeichen.
- `text`: Zu tippender Text.
- `options.delay`: Verzögerung in ms zwischen Tastendrücken.
- `options.timeout`: Maximale Wartezeit in ms.
```javascript
// Veraltet: await locator.type('Hallo');
// Gibt zurück: Promise<void>
```

</details>









</details>
























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






## Test generator
- https://playwright.dev/docs/codegen

<details><summary>Click to expand..</summary>


# Option 1 - VS Code Extension 
- https://marketplace.visualstudio.com/items?itemName=ms-playwright.playwright

1. To record a test click on the Record new button from the Testing sidebar. This will create a test-1.spec.ts file as well as open up a browser window.

2. In the browser go to the URL you wish to test and start clicking around to record your user actions.


<br><br>

# Option 2 - Playwright inspector
- Usefully for e.g. electron apps

1. Create record.spec.ts
```
// In a test file, e.g., test/e2e/record-my-feature.e2e.ts
import { test } from '@playwright/test'
import { AppSetup } from './app.base.setup.ts'

test.describe('Recording Session', () => {
    let appSetup: AppSetup

    test.beforeAll(async() => {
        appSetup = new AppSetup(/* pass pvs if needed */)
        await appSetup.init() // This launches Electron, and it loads your UI
    })

    test.afterAll(async() => {
        await appSetup.close()
    })

    test.only('Record interactions here', async() => {
        // The webServer from playwright.config.ts has started your Vite server.
        // appSetup.init() has launched Electron, and Electron has loaded the UI.
        
        // Now, pause to open the Playwright Inspector connected to the Electron window
        await appSetup.page.pause() 

        // After you're done recording with the inspector, you can stop the test.
        // The recorded steps will appear in the Inspector window.
    })
})
```

2. The Playwright Inspector window will also appear due to await appSetup.page.pause();
   
3. Use the "Record" button in the Playwright Inspector (not the browser extension).
  
</details>









<br><br>
<br><br>

## Test CLI
- https://playwright.dev/docs/next/test-cli

<details><summary>Click to expand..</summary>

# Command line

Playwright provides a powerful command line interface for running tests, generating code, debugging, and more. The most up to date list of commands and arguments available on the CLI can always be retrieved via `npx playwright --help`.

## Essential Commands

### Run Tests
Run your Playwright tests. Read more about running tests.

#### Syntax
```bash
npx playwright test [options] [test-filter...]
```

#### Examples
```bash
# Run all tests
npx playwright test

# Run a single test file
npx playwright test tests/todo-page.spec.ts

# Run a set of test files
npx playwright test tests/todo-page/ tests/landing-page/

# Run tests at a specific line
npx playwright test my-spec.ts:42

# Run tests by title
npx playwright test -g "add a todo item"

# Run tests in headed browsers
npx playwright test --headed

# Run tests for a specific project
npx playwright test --project=chromium

# Get help
npx playwright test --help

# Disable parallelization
npx playwright test --workers=1

# Run in debug mode with Playwright Inspector
npx playwright test --debug

# Run tests in interactive UI mode
npx playwright test --ui
```

#### Common Options
| Option                       | Description                                                                                                                               |
|------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| `--debug`                    | Run tests with Playwright Inspector. Shortcut for `PWDEBUG=1` environment variable and `--timeout=0 --max-failures=1 --headed --workers=1` options. |
| `--headed`                   | Run tests in headed browsers (default: headless).                                                                                         |
| `-g <grep>` or `--grep <grep>` | Only run tests matching this regular expression (default: ".*").                                                                          |
| `--project <project-name...>`| Only run tests from the specified list of projects, supports '*' wildcard (default: run all projects).                                    |
| `--ui`                       | Run tests in interactive UI mode.                                                                                                         |
| `-j <workers>` or `--workers <workers>` | Number of concurrent workers or percentage of logical CPU cores, use 1 to run in a single worker (default: 50%).                 |

#### All Options
| Option                                | Description                                                                                                                                                                |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Non-option arguments                  | Each argument is treated as a regular expression matched against the full test file path. Only tests from files matching the pattern will be executed. Special symbols like `$` or `*` should be escaped with `\`. In many shells/terminals you may need to quote the arguments. |
| `-c <file>` or `--config <file>`      | Configuration file, or a test directory with optional "playwright.config.{m,c}?{js,ts}". Defaults to `playwright.config.ts` or `playwright.config.js` in the current directory. |
| `--debug`                             | Run tests with Playwright Inspector. Shortcut for `PWDEBUG=1` environment variable and `--timeout=0 --max-failures=1 --headed --workers=1` options.                          |
| `--fail-on-flaky-tests`               | Fail if any test is flagged as flaky (default: false).                                                                                                                     |
| `--forbid-only`                       | Fail if `test.only` is called (default: false). Useful on CI.                                                                                                              |
| `--fully-parallel`                    | Run all tests in parallel (default: false).                                                                                                                                |
| `--global-timeout <timeout>`          | Maximum time this test suite can run in milliseconds (default: unlimited).                                                                                                 |
| `-g <grep>` or `--grep <grep>`        | Only run tests matching this regular expression (default: ".*").                                                                                                           |
| `-gv <grep>` or `--grep-invert <grep>`| Only run tests that do not match this regular expression.                                                                                                                  |
| `--headed`                            | Run tests in headed browsers (default: headless).                                                                                                                          |
| `--ignore-snapshots`                  | Ignore screenshot and snapshot expectations.                                                                                                                               |
| `-j <workers>` or `--workers <workers>` | Number of concurrent workers or percentage of logical CPU cores, use 1 to run in a single worker (default: 50%).                                                         |
| `--last-failed`                       | Only re-run the failures.                                                                                                                                                  |
| `--list`                              | Collect all the tests and report them, but do not run.                                                                                                                     |
| `--max-failures <N>` or `-x`          | Stop after the first N failures. Passing `-x` stops after the first failure.                                                                                             |
| `--no-deps`                           | Do not run project dependencies.                                                                                                                                           |
| `--output <dir>`                      | Folder for output artifacts (default: "test-results").                                                                                                                     |
| `--only-changed [ref]`                | Only run test files that have been changed between 'HEAD' and 'ref'. Defaults to running all uncommitted changes. Only supports Git.                                     |
| `--pass-with-no-tests`                | Makes test run succeed even if no tests were found.                                                                                                                        |
| `--project <project-name...>`         | Only run tests from the specified list of projects, supports '*' wildcard (default: run all projects).                                                                     |
| `--quiet`                             | Suppress stdio.                                                                                                                                                            |
| `--repeat-each <N>`                   | Run each test N times (default: 1).                                                                                                                                        |
| `--reporter <reporter>`               | Reporter to use, comma-separated, can be "dot", "line", "list", or others (default: "list"). You can also pass a path to a custom reporter file.                           |
| `--retries <retries>`                 | Maximum retry count for flaky tests, zero for no retries (default: no retries).                                                                                            |
| `--shard <shard>`                     | Shard tests and execute only the selected shard, specified in the form "current/all", 1-based, e.g., "3/5".                                                                |
| `--timeout <timeout>`                 | Specify test timeout threshold in milliseconds, zero for unlimited (default: 30 seconds).                                                                                  |
| `--trace <mode>`                      | Force tracing mode, can be `on`, `off`, `on-first-retry`, `on-all-retries`, `retain-on-failure`, `retain-on-first-failure`.                                                |
| `--tsconfig <path>`                   | Path to a single tsconfig applicable to all imported files (default: look up tsconfig for each imported file separately).                                                  |
| `--ui`                                | Run tests in interactive UI mode.                                                                                                                                          |
| `--ui-host <host>`                    | Host to serve UI on; specifying this option opens UI in a browser tab.                                                                                                     |
| `--ui-port <port>`                    | Port to serve UI on, 0 for any free port; specifying this option opens UI in a browser tab.                                                                                |
| `-u` or `--update-snapshots [mode]`   | Update snapshots with actual results. Possible values are "all", "changed", "missing", and "none". Running tests without the flag defaults to "missing"; running tests with the flag but without a value defaults to "changed". |
| `--update-source-method [mode]`       | Update snapshots with actual results. Possible values are "patch" (default), "3way" and "overwrite". "Patch" creates a unified diff file that can be used to update the source code later. "3way" generates merge conflict markers in source code. "Overwrite" overwrites the source code with the new snapshot values. |
| `-x`                                  | Stop after the first failure.                                                                                                                                              |

### Show Report
Display HTML report from previous test run. Read more about the HTML reporter.

#### Syntax
```bash
npx playwright show-report [report] [options]
```

#### Examples
```bash
# Show latest test report
npx playwright show-report

# Show a specific report
npx playwright show-report playwright-report/

# Show report on custom port
npx playwright show-report --port 8080
```

#### Options
| Option        | Description                            |
|---------------|----------------------------------------|
| `--host <host>` | Host to serve report on (default: localhost) |
| `--port <port>` | Port to serve report on (default: 9323)  |

### Install Browsers
Install browsers required by Playwright. Read more about Playwright's browser support.

#### Syntax
```bash
npx playwright install [options] [browser...]
npx playwright install-deps [options] [browser...]
npx playwright uninstall
```

#### Examples
```bash
# Install all browsers
npx playwright install

# Install only Chromium
npx playwright install chromium

# Install specific browsers
npx playwright install chromium webkit

# Install browsers with dependencies
npx playwright install --with-deps
```

#### Install Options
| Option        | Description                                        |
|---------------|----------------------------------------------------|
| `--force`     | Force reinstall of stable browser channels         |
| `--with-deps` | Install browser system dependencies                |
| `--dry-run`   | Don't perform installation, just print information |
| `--only-shell`| Only install chromium-headless-shell instead of full Chromium |
| `--no-shell`  | Don't install chromium-headless-shell              |

#### Install Deps Options
| Option      | Description                                        |
|-------------|----------------------------------------------------|
| `--dry-run` | Don't perform installation, just print information |

## Generation & Debugging Tools

### Code Generation
Record actions and generate tests for multiple languages. Read more about Codegen.

#### Syntax
```bash
npx playwright codegen [options] [url]
```

#### Examples
```bash
# Start recording with interactive UI
npx playwright codegen

# Record on specific site
npx playwright codegen https://playwright.dev

# Generate Python code
npx playwright codegen --target=python
```

#### Options
| Option                         | Description                                                          |
|--------------------------------|----------------------------------------------------------------------|
| `-b, --browser <name>`         | Browser to use: `chromium`, `firefox`, or `webkit` (default: `chromium`) |
| `-o, --output <file>`          | Output file for the generated script                                 |
| `--target <language>`          | Language to use: `javascript`, `playwright-test`, `python`, etc.       |
| `--test-id-attribute <attr>`   | Attribute to use for test IDs                                        |

### Trace Viewer
Analyze and view test traces for debugging. Read more about Trace Viewer.

#### Syntax
```bash
npx playwright show-trace [options] <trace>
```

#### Examples
```bash
# View a trace file
npx playwright show-trace trace.zip

# View trace from directory
npx playwright show-trace trace/
```

#### Options
| Option                 | Description                                                          |
|------------------------|----------------------------------------------------------------------|
| `-b, --browser <name>` | Browser to use: `chromium`, `firefox`, or `webkit` (default: `chromium`) |
| `-h, --host <host>`    | Host to serve trace on                                               |
| `-p, --port <port>`    | Port to serve trace on                                               |

## Specialized Commands

### Merge Reports
Read blob reports and combine them. Read more about merge-reports.

#### Syntax
```bash
npx playwright merge-reports [options] <blob dir>
```

#### Examples
```bash
# Combine test reports
npx playwright merge-reports ./reports
```

#### Options
| Option                    | Description                                                                                                                             |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| `-c, --config <file>`     | Configuration file. Can be used to specify additional configuration for the output report                                               |
| `--reporter <reporter>`   | Reporter to use, comma-separated, can be "list", "line", "dot", "json", "junit", "null", "github", "html", "blob" (default: "list")       |

### Clear Cache
Clear all Playwright caches.

#### Syntax
```bash
npx playwright clear-cache
```
  
</details>


























<br><br>
<br><br>



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
