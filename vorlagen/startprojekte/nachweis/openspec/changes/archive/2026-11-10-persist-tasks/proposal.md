## Why

Nach dem Neuladen der Seite ist die Liste leer. Wer seine Aufgaben am nächsten Tag wieder öffnet, findet nichts mehr.

## What Changes

- Aufgaben bleiben nach dem Neuladen erhalten.
- Beim ersten Start ohne gespeicherte Aufgaben ist die Liste leer.
- Wo gespeichert wird, entscheidet ADR 0001.

## Capabilities

### New Capabilities

- keine

### Modified Capabilities

- `aufgaben`: neues Requirement „Aufgaben bleiben erhalten“.

## Impact

`src/tasks.js` und `src/app.js` geändert, neu `test/storage.test.js`. ADR 0001 liegt vor dem Change in `adr/`.
