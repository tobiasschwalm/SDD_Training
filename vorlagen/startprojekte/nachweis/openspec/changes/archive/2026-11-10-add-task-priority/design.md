## Context

Aufgaben sind bisher `{ title }` und liegen laut ADR 0001 im Local Storage. Gespeicherte Aufgaben aus den Schritten davor haben keine Priorität.

## Decisions

- Eine Aufgabe ist `{ title, priority }` mit `priority` als `"hoch"`, `"mittel"` oder `"niedrig"`. `add_task(tasks, title, priority)` setzt ohne Angabe `"mittel"`.
- `load` ergänzt bei gespeicherten Aufgaben ohne Priorität `"mittel"`. Gespeichert wird beim nächsten Ändern im neuen Format.
- `by_priority(tasks)` liefert die Aufgaben sortiert mit ihrem Index in der ganzen Liste, damit Bearbeiten und Löschen in der sortierten Ansicht die richtige Aufgabe treffen.
- Ein Kästchen „Nach Priorität sortieren“ in `index.html`. Ob es nach dem Neuladen gesetzt bleibt, legt die Spec nicht fest. Der Code beginnt unsortiert.
- Aus dem Review entschieden: Reihenfolge bei gleicher Priorität. `Array.prototype.sort` ist stabil, gleiche Prioritäten bleiben in der Reihenfolge des Anlegens. Für eine Person reicht das, ein Requirement ist nicht nötig.
- Die Regel für gespeicherte Aufgaben ohne Priorität steht unter „Aufgabe anlegen“, weil dort der Standardwert mittel festgelegt ist.
