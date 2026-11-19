## Why

Die Priorität sagt, wie wichtig eine Aufgabe ist, aber nicht, bis wann sie erledigt sein muss. Eine Aufgabe, deren Termin schon vorbei ist, geht in der Liste unter.

## What Changes

- Eine Aufgabe kann beim Anlegen ein Fälligkeitsdatum bekommen. Das Datum ist optional.
- Eine Aufgabe ist überfällig, wenn ihr Fälligkeitsdatum vor heute liegt. Heute fällig ist noch nicht überfällig.
- Überfällige Aufgaben stehen oben in der Liste, auch wenn nach Priorität sortiert wird.
- Bewusst nicht festgelegt: Reihenfolge innerhalb der überfälligen Aufgaben, optische Markierung, nachträgliches Ändern des Datums.

## Capabilities

### New Capabilities

- keine

### Modified Capabilities

- `aufgaben`: Requirement „Aufgabe anlegen“ um das optionale Fälligkeitsdatum erweitert, Requirement „Aufgaben sortieren“ um überfällige Aufgaben ergänzt, neues Requirement „Überfällige Aufgaben oben“.

## Impact

`index.html`, `src/tasks.js`, `src/app.js` und `test/tasks.test.js`. Gespeicherte Aufgaben ohne Datum bleiben gültig, eine Umstellung des gespeicherten Formats ist nicht nötig.
