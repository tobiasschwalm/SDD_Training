## Why

In einer langen Liste stehen dringende Aufgaben zwischen unwichtigen. Wer wissen will, was zuerst dran ist, muss die ganze Liste lesen.

## What Changes

- Jede Aufgabe hat eine Priorität hoch, mittel oder niedrig, ohne Angabe mittel.
- Die Liste lässt sich nach Priorität sortieren, hoch zuerst.
- Nach dem Review ergänzt: Aufgaben, die vor diesem Change ohne Priorität gespeichert wurden, gelten als mittel.

## Capabilities

### New Capabilities

- keine

### Modified Capabilities

- `aufgaben`: Requirement „Aufgabe anlegen“ um die Priorität erweitert, neues Requirement „Aufgaben sortieren“.

## Impact

`index.html`, `src/tasks.js`, `src/app.js`, `test/tasks.test.js` und `test/storage.test.js`.
