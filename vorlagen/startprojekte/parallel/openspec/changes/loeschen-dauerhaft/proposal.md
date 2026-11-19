## Why

Die Spec sagt nicht, ob eine gelöschte Aufgabe nach dem Neuladen gelöscht bleibt. „Aufgaben bleiben erhalten“ verlangt nur, dass alle Aufgaben wiederkommen.

## What Changes

- Requirement „Aufgabe löschen“: Eine gelöschte Aufgabe fehlt auch nach dem Neuladen der Seite.

## Capabilities

### Modified Capabilities

- `aufgaben`: Löschen gilt auch nach dem Neuladen

## Impact

`test/storage.test.js` bekommt einen Test. `src/app.js` speichert schon nach jedem Löschen, Code ändert sich voraussichtlich nicht.
