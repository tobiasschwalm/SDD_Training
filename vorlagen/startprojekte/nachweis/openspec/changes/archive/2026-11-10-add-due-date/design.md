## Context

Aufgaben sind `{ title, priority }` und liegen laut ADR 0001 als JSON im Local Storage. Die Logik steht laut ADR 0002 ohne Zugriff auf die Seite in `src/tasks.js`, die Tests laufen mit `node:test` ohne Browser. `app.js` zeigt die Liste entweder in der Reihenfolge des Anlegens oder über `by_priority`. Beide Wege geben jeder Aufgabe ihren Index in der ganzen Liste mit, damit Bearbeiten und Löschen die richtige Aufgabe treffen. Motivation und Umfang: siehe proposal.md.

## Goals / Non-Goals

**Goals:**
- Die Regel „überfällig oben“ prüfen die Tests ohne Abhängigkeit vom Tag, an dem sie laufen.
- Eine Funktion legt die angezeigte Reihenfolge fest, `app.js` rechnet nichts selbst.

**Non-Goals:**
- Kein neues ADR. Speicherort und Schnitt zwischen Logik und Seite bleiben, wie sie sind.
- Keine Uhrzeit, keine Zeitzonen, keine Formatierung des Datums für die Anzeige.

## Decisions

- **Datum als Text `"YYYY-MM-DD"` im Feld `due`.** So liefert es `<input type="date">`, und so steht es im JSON. Texte in diesem Format lassen sich direkt vergleichen: `task.due < today`. Alternative `Date`-Objekte: JSON speichert sie ohnehin als Text, und der Vergleich mit Uhrzeit und Zeitzone kann am Tagesrand falsch liegen.
- **Ohne Datum fehlt das Feld `due`.** `add_task(tasks, title, priority, due)` setzt `due` nur, wenn ein nicht leeres Datum übergeben wird. Ein leeres Datumsfeld liefert `""`, und das zählt als kein Datum. Der bestehende Test „Ohne Angabe der Priorität“ erwartet genau `{ title, priority }` und bleibt so unverändert grün. `load` braucht keine Änderung, gespeicherte Aufgaben ohne `due` sind gültig.
- **`today` wird übergeben.** Neue Funktion `shown_tasks(tasks, today, sorted)` in `src/tasks.js` liefert die angezeigte Liste, jede Aufgabe mit ihrem Index. Bei `sorted` erst `by_priority`, sonst die Liste mit Index. Danach stehen die überfälligen Aufgaben vorn, sonst bleibt die Reihenfolge (stabiles Aufteilen in zwei Gruppen). Alternative `new Date()` in `tasks.js`: Die Tests hingen dann vom Datum ab, an dem sie laufen.
- **Heute in `app.js` aus der lokalen Zeit.** `app.js` bildet `today` aus `getFullYear`, `getMonth` und `getDate`. `toISOString()` rechnet in UTC und läge in Deutschland zwischen Mitternacht und ein oder zwei Uhr noch auf dem Vortag.
- **Reihenfolge der Tests nach Rot vor Grün.** Jeder neue Test muss vor seinem Code rot sein. Deshalb kommt „Heute fällig ist nicht überfällig“ vor „Überfällige Aufgabe steht oben“. Umgekehrt wäre der Test zur Grenze sofort grün. Das Szenario zum Sortieren hat drei Aufgaben, damit es rot ist, solange `shown_tasks` den Wert `sorted` nicht beachtet.
- **Anzeige.** Ein Datumsfeld im Formular. In der Liste steht hinter der Priorität `fällig 2026-11-20`, bei Aufgaben ohne Datum nichts. Das Datum bleibt beim Bearbeiten des Titels erhalten, weil `edit_task` die übrigen Felder übernimmt.

## Risks / Trade-offs

- [Die Liste ändert über Nacht ihre Reihenfolge, ohne dass jemand etwas tut] → Gewollt. `today` wird bei jedem `render` neu bestimmt. Bleibt die Seite über Mitternacht offen, gilt das neue Datum erst beim nächsten Rendern. Für eine Person reicht das.
- [Die Verdrahtung in `app.js`, auch die Bildung von `today`, bleibt ohne Test (ADR 0002)] → Im Browser prüfen mit einer Aufgabe, die gestern fällig war.
- [Ein Datum, das nicht dem Format `"YYYY-MM-DD"` folgt, würde falsch verglichen] → Nur `<input type="date">` erzeugt Daten, und das liefert immer dieses Format oder `""`.
