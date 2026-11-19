## Context

ADR 0001 legt fest: Die Aufgaben liegen im Local Storage des Browsers, die Logik bekommt den Speicher übergeben. Bisher leben die Aufgaben nur im Speicher der Seite.

## Decisions

- Gemäß ADR 0001: `load(storage)` und `save(storage, tasks)` in `src/tasks.js` bekommen ein Objekt mit `getItem` und `setItem`. Im Browser übergibt `src/app.js` den localStorage, im Test eine Attrappe auf Basis einer `Map`.
- Gespeichert wird die ganze Liste als JSON-Text unter dem Schlüssel `tasks`, nach jeder Änderung.
- Fehlt der Schlüssel, liefert `load` eine leere Liste.
- Keine Abweichung von ADR 0001, kein neues ADR nötig.
