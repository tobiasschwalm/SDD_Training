# Tasks

Jeder Test-Task endet mit `git add test/`. Jeder Code-Task prüft mit `git diff --quiet test/`, dass die Testdateien seit dem Test-Task unverändert sind. Die bestehenden dreizehn Tests bleiben unverändert, nur die `require`-Zeile in `test/tasks.test.js` bekommt neue Namen.

## 1. Fälligkeitsdatum beim Anlegen

- [x] 1.1 Test „Aufgabe mit Fälligkeitsdatum“ in `test/tasks.test.js`: `add_task([], "Milch kaufen", "hoch", "2026-11-20")` liefert `[{ title: "Milch kaufen", priority: "hoch", due: "2026-11-20" }]`. `node --test` zeigt diesen Test rot
- [x] 1.2 `add_task` in `src/tasks.js` setzt `due`, wenn ein Datum übergeben wird, und lässt das Feld ohne Angabe weg. Nur so viel, wie der Test verlangt: Ein leerer Text wird noch nicht gesondert behandelt. Prüfen: `node --test` zeigt alle vierzehn Tests grün, `git diff --quiet test/` meldet keine Änderung
- [x] 1.3 Test „Leeres Datumsfeld“: `add_task([], "Milch kaufen", "mittel", "")` liefert `[{ title: "Milch kaufen", priority: "mittel" }]` ohne `due`. `node --test` zeigt diesen Test rot
- [x] 1.4 `add_task` behandelt `""` wie kein Datum. Prüfen: `node --test` zeigt alle fünfzehn Tests grün, `git diff --quiet test/` meldet keine Änderung

## 2. Überfällige Aufgaben oben

- [x] 2.1 Leere Funktion `shown_tasks(tasks, today, sorted)` in `src/tasks.js` anlegen und exportieren. Prüfen: `node --test` zeigt die fünfzehn Tests weiter grün
- [x] 2.2 Test „Heute fällig ist nicht überfällig“: `shown_tasks` mit `today` `"2026-11-10"`, `sorted` `false` und „Milch kaufen“ ohne Datum, „Brot backen“ fällig `"2026-11-10"` liefert die Titel „Milch kaufen“, „Brot backen“. `node --test` zeigt diesen Test rot
- [x] 2.3 `shown_tasks` liefert die Aufgaben mit ihrem Index in der ganzen Liste, in der Reihenfolge des Anlegens. Prüfen: `node --test` zeigt alle sechzehn Tests grün, `git diff --quiet test/` meldet keine Änderung
- [x] 2.4 Test „Überfällige Aufgabe steht oben“: `shown_tasks` mit `today` `"2026-11-10"`, `sorted` `false` und „Milch kaufen“ ohne Datum, „Post holen“ fällig `"2026-11-20"`, „Brot backen“ fällig `"2026-11-09"` liefert „Brot backen“, „Milch kaufen“, „Post holen“. `node --test` zeigt diesen Test rot
- [x] 2.5 `shown_tasks` stellt Aufgaben mit `due < today` stabil vor die übrigen. Prüfen: `node --test` zeigt alle siebzehn Tests grün, `git diff --quiet test/` meldet keine Änderung
- [x] 2.6 Test „Überfällige Aufgabe steht beim Sortieren oben“: `shown_tasks` mit `today` `"2026-11-10"`, `sorted` `true` und „Post holen“ niedrig ohne Datum, „Milch kaufen“ hoch ohne Datum, „Brot backen“ niedrig fällig `"2026-11-09"` liefert „Brot backen“, „Milch kaufen“, „Post holen“. `node --test` zeigt diesen Test rot
- [x] 2.7 `shown_tasks` sortiert bei `sorted` zuerst mit `by_priority`, dann stehen die überfälligen Aufgaben vorn. Prüfen: `node --test` zeigt alle achtzehn Tests grün, `git diff --quiet test/` meldet keine Änderung

## 3. Seite

- [x] 3.1 Datumsfeld im Formular in `index.html`. `app.js` übergibt den Wert an `add_task`, bildet `today` aus der lokalen Zeit (`getFullYear`, `getMonth`, `getDate`), zeigt die Liste über `shown_tasks` und schreibt `fällig <Datum>` hinter die Priorität. Prüfen im Browser: Aufgaben mit gestern, heute und ohne Datum anlegen. Nur die gestern fällige steht oben, mit und ohne Sortieren. In der sortierten Ansicht eine Aufgabe bearbeiten und eine löschen, es trifft jeweils die richtige. Nach dem Neuladen sind die Daten noch da

## 4. Aufräumen

- [x] 4.1 Aufräumen ohne neues Verhalten: Die Aufbereitung der Liste in `render` in `app.js` ist durch `shown_tasks` ersetzt, die Kommentare in `src/tasks.js` nennen das Feld `due`. Prüfen: `node --test` zeigt alle achtzehn Tests grün, `git diff --quiet test/` meldet keine Änderung, `grep -nE "document|window" src/tasks.js` findet nichts
