## Context

Laut ADR 0001 speichert die App nach jeder Änderung im Local Storage. Die Logik in `src/tasks.js` kennt die Seite nicht.

## Decisions

- `edit_task(tasks, index, title)` prüft den Titel wie `add_task` und liefert bei leerem Titel die Liste unverändert mit dem Hinweis.
- `delete_task(tasks, index, confirmed)` entfernt die Aufgabe nur, wenn `confirmed` wahr ist. Die Rückfrage stellt `src/app.js` mit `confirm`, die Logik bekommt nur die Antwort.
- Bearbeiten über `prompt` mit dem bisherigen Titel als Vorgabe. Abbrechen ändert nichts.
- Gespeichert wird wie bisher nach jeder Änderung.
