# Tasks

Jeder Test-Task endet mit `git add test/`. Jeder Code-Task prüft mit `git diff --quiet test/`, dass die Testdateien seit dem Test-Task unverändert sind. Die neuen Tests stehen in `test/exchange.test.js`, die bestehenden neunzehn Tests bleiben unverändert.

## 1. Leere Funktionen

- [x] 1.1 Leere Funktionen `export_text(tasks)` und `import_text(tasks, text)` in `src/tasks.js` anlegen und exportieren. Prüfen: `node --test` zeigt die neunzehn Tests weiter grün

## 2. Export

- [x] 2.1 Beispieldatei `test/fixtures/export-v1.json` nach ADR 0003 anlegen: Formatversion 1, „Milch kaufen“ hoch fällig `"2026-11-20"`, „Brot backen“ niedrig ohne `due`, eingerückt mit zwei Leerzeichen, Zeilenende am Schluss. Test „Export mit allen Angaben“: `export_text` mit beiden Aufgaben in der Form, die `load` liefert (`priority` vor `title`), ergibt genau den Text der Beispieldatei. `node --test` zeigt diesen Test rot
- [x] 2.2 `export_text` schreibt Kennung, Version und je Aufgabe `title`, `priority`, `due` in dieser Reihenfolge. Prüfen: `node --test` zeigt alle zwanzig Tests grün, `git diff --quiet test/` meldet keine Änderung

## 3. Import

- [x] 3.1 Test „Datei der Formatversion 1“: `import_text([], <Beispieldatei>)` liefert „Milch kaufen“ hoch fällig `"2026-11-20"`, danach „Brot backen“ niedrig. `node --test` zeigt diesen Test rot
- [x] 3.2 `import_text` liest die Aufgaben aus der Datei. Nur so viel, wie der Test verlangt: Die bisherige Liste bleibt noch unbeachtet. Prüfen: `node --test` zeigt alle 21 Tests grün, `git diff --quiet test/` meldet keine Änderung
- [x] 3.3 Test „Import hängt an die Liste an“: `import_text` mit „Post holen“ in der Liste und einer Exportdatei mit „Milch kaufen“ liefert die Titel „Post holen“, „Milch kaufen“ und `hint` „Importiert: 1, als Dublette übersprungen: 0“. `node --test` zeigt diesen Test rot
- [x] 3.4 `import_text` hängt die Aufgaben der Datei an die Liste an und nennt die Zahl im `hint`. Prüfen: `node --test` zeigt alle 22 Tests grün, `git diff --quiet test/` meldet keine Änderung
- [x] 3.5 Test „Dublette beim Import“: Liste mit „Milch kaufen“ mittel, Datei mit „Milch kaufen“ hoch und „Brot backen“ niedrig. Ergebnis „Milch kaufen“ mittel, „Brot backen“ niedrig, `hint` „Importiert: 1, als Dublette übersprungen: 1“. `node --test` zeigt diesen Test rot
- [x] 3.6 `import_text` überspringt Aufgaben, deren Titel schon in der Liste steht, und zählt angelegte und übersprungene im `hint`. Prüfen: `node --test` zeigt alle 23 Tests grün, `git diff --quiet test/` meldet keine Änderung
- [x] 3.7 Test „Datei ist keine Exportdatei“: `import_text` mit „Milch kaufen“ in der Liste und dem Text `Milch kaufen, Brot backen` liefert die Liste unverändert und `hint` „Die Datei ist keine Exportdatei der ToDo-App“. `node --test` zeigt diesen Test rot
- [x] 3.8 `import_text` lehnt Text ab, der kein JSON ist. Prüfen: `node --test` zeigt alle 24 Tests grün, `git diff --quiet test/` meldet keine Änderung
- [x] 3.9 Test „Aufgabenliste ohne Kennung der App“: `import_text` mit „Milch kaufen“ in der Liste und dem Text von `backup_text` aus `src/backup.js` liefert die Liste unverändert und `hint` „Die Datei ist keine Exportdatei der ToDo-App“. `node --test` zeigt diesen Test rot
- [x] 3.10 `import_text` lehnt JSON ohne Kennung `todo-app-export` ab. Prüfen: `node --test` zeigt alle 25 Tests grün, `git diff --quiet test/` meldet keine Änderung
- [x] 3.11 Test „Datei aus einer neueren Version“: `import_text` mit „Milch kaufen“ in der Liste und einer Datei mit Kennung `todo-app-export` und `version` 2 liefert die Liste unverändert und `hint` „Die Datei stammt aus einer neueren Version der ToDo-App“. `node --test` zeigt diesen Test rot
- [x] 3.12 `import_text` lehnt eine `version` größer als 1 ab. Prüfen: `node --test` zeigt alle 26 Tests grün, `git diff --quiet test/` meldet keine Änderung

## 4. Seite

- [x] 4.1 In `index.html` ein Knopf „Exportieren“ und ein Dateifeld „Importieren“. `app.js` lädt `export_text(tasks)` als `aufgaben.json` herunter, liest eine gewählte Datei mit `file.text()`, übergibt sie an `import_text`, speichert und zeigt `hint`. Prüfen im Browser: zwei Aufgaben exportieren, eine löschen, die Datei importieren (nur die gelöschte kommt zurück, Hinweis nennt eine Dublette), nach dem Neuladen ist die Liste noch da

## 5. Aufräumen

- [x] 5.1 Aufräumen ohne neues Verhalten: Kennung und Version als Konstanten oben in `src/tasks.js`, Kommentare nennen ADR 0003. Prüfen: `node --test` zeigt alle 26 Tests grün, `git diff --quiet test/` meldet keine Änderung, `grep -nE "document|window" src/tasks.js` findet nichts
