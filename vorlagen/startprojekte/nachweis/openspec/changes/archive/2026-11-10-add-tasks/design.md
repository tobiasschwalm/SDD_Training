## Context

Laut Kontext der `config.yaml`: Plain JavaScript im Browser, keine Bibliotheken, kein Build-Schritt, Tests mit `node:test`. Das Projekt enthält noch keinen Code.

## Decisions

- `src/tasks.js` enthält die Logik ohne Zugriff auf die Seite: `add_task(tasks, title)`. Eine Aufgabe ist `{ title }`, die Funktion liefert eine neue Liste und einen Hinweis. Die Datei läuft als klassisches Skript im Browser und exportiert die Funktion zusätzlich für `node:test`, wenn `module` existiert.
- `add_task` entfernt Leerraum um den Titel. Bleibt nichts übrig, liefert sie die Liste unverändert und den Hinweis „Bitte einen Titel eingeben“.
- `src/app.js` verbindet Formular, Liste und Logik. Klassische Skripte statt ES-Module, damit `index.html` per Doppelklick ohne Server läuft.
- Die Aufgaben leben nur im Speicher der Seite. Nach dem Neuladen ist die Liste leer.
