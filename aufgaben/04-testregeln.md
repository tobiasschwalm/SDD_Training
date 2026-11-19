# Aufgabe: ToDo-App unter Testregeln

Ein fünfter Change für die ToDo-App, diesmal mit Testregeln in `openspec/config.yaml`: Der Agent soll je Szenario zuerst einen Unit-Test planen, der vor dem Code rot ist, und anhalten, wenn ein Test der Spec widerspricht. Ob er das tut, zeigen `tasks.md` und das Transkript. OpenSpec prüft es nicht. Etwa 40 Minuten.

## Start

Im eigenen Projektordner der ToDo-App, Stand nach Change 4, alle vier Changes archiviert (`openspec list` meldet „No active changes found.“). Wer nicht so weit ist, bekommt vom Trainer den Ordner `todo-app-stand-4`, legt ihn als neuen Projektordner `<kurs>/projekte/todo-app-5` ab (nicht über den eigenen Stand kopieren) und richtet ihn ein:

```bash
cd <kurs>/projekte/todo-app-5 && rm -f README.md && openspec init --tools claude
```

Prüfen: `openspec list` meldet „No active changes found.“, `openspec validate --all --strict` meldet „1 passed, 0 failed“, `node --test` 13 grüne Tests. Das Archiv ist in dieser Kopie leer, die Hauptspec `openspec/specs/aufgaben/spec.md` enthält alle vier Changes.

Den Agenten in einer neuen Sitzung starten, bei Claude Code `/clear` oder ein neues Fenster.

## Schritte

1. Testregeln eintragen.
   a. In `openspec/config.yaml` alles ab `rules:` durch den Inhalt von `<kurs>/vorlagen/config/testregeln.yaml` ersetzen. Die Regeln sind die von der Folie „Test zuerst über die config.yaml“, angepasst an die ToDo-App: `node:test`, Logik in `src/tasks.js`, Tests in `test/`.
   b. Prüfen, dass OpenSpec sie weitergibt: `openspec new change probe`, dann `openspec instructions tasks --change probe` zeigt die Regeln im Block `<rules>`. Danach `rm -r openspec/changes/probe`.
2. Mit `/openspec-explore` den Wunsch „Aufgaben bekommen ein Fälligkeitsdatum, überfällige stehen oben“ klären. Ein oder zwei Antworten reichen, etwa: Das Datum ist optional, überfällig heißt vor heute, überfällige stehen auch bei Sortierung nach Priorität oben. Weitere Rückfragen dürfen offen bleiben. In derselben Sitzung `/openspec-propose`. Nach `/clear` wäre das Ergebnis aus explore weg.
3. `tasks.md` prüfen, erst dann weiter. Für die Freigabe hat OpenSpec keinen Befehl: Freigegeben ist die `tasks.md`, wenn ihr beide Fragen mit Ja beantwortet und danach Schritt 4 startet. Im Team wäre das die Freigabe im Pull Request vor dem Umsetzen.
   a. Steht je Szenario zuerst ein Test-Task mit dem Szenario-Namen, und erwartet er einen roten Lauf, weil das Verhalten fehlt?
   b. Schreibt ein Code-Task mehr vor, als sein Test verlangt? Stimmt etwas nicht, mit `/openspec-update-change` nachbessern lassen.
4. Mit `/openspec-apply-change` in einer neuen Sitzung umsetzen lassen und das Transkript beobachten.
   a. Hält der Agent an, wenn ein Test der Spec widerspricht oder ein neuer Test sofort grün ist? Dann entscheidet der Mensch: Test genauer fassen oder Szenario in der Spec ergänzen.
   b. Ergänzt er die Testdateien nur, oder ändert er bestehende Tests? Das Transkript zeigt jede Bearbeitung einer Datei mit den geänderten Zeilen. Tipp: `node --test` beim ersten Mal dauerhaft erlauben, sonst fragt der Agent bei jedem Lauf.
5. Den neuen Code in `src/tasks.js` lesen: Was entscheidet der Code, das die Spec nicht festlegt? Etwa die Reihenfolge unter den überfälligen Aufgaben oder was mit dem heutigen Datum gilt.
6. Vor dem Archiv `/openspec-verify-change` ausführen. Nennt der Bericht die Stelle, die ihr im Code gefunden habt? Danach mit `/openspec-archive-change` abschließen. Fragt der Agent nach dem Sync der Spec, mit „Ja“ antworten.

Ergebnis: je Szenario ein Test, erst rot, dann grün, und eine Stelle im Code, die die Spec nicht festlegt.

## Auswertung

- Hat der Agent die Reihenfolge gehalten, erst Test, dann Code?
- Wo hat er angehalten, und wo nicht?
- Was entscheidet der Code ohne Spec, und hat `/openspec-verify-change` die Stelle genannt?

## Zusatz für Schnelle

Die Regeln aus der `config.yaml` entfernen (alles ab `rules:`), denselben Wunsch in einem neuen Change nur mit `/openspec-propose` planen lassen und die beiden `tasks.md` vergleichen. Wo stehen jetzt die Tests, und erwartet ein Task noch einen roten Lauf?

## Falls ihr hängt

- Lösungsstände und das Protokoll des Trainerlaufs hat der Trainer.
- Den Skill `/openspec-verify-change` gibt es nicht: Das Profil enthält verify nicht. `openspec config profile` ausführen, verify auswählen, danach `openspec update`.
- Testlauf von Hand: `node --test` im Projektordner, ein `package.json` ist nicht nötig.
- Der Agent hält nicht an: Die Regeln sind nur Text in der Anweisung, OpenSpec prüft nichts davon. Das Transkript zeigt, was er stattdessen getan hat. Das ist ein Ergebnis für die Auswertung, kein Fehler der Übung.
- Fehlt der Block `<rules>`: Einrückung in `config.yaml` prüfen, ein Doppelpunkt mit Leerzeichen in einer Regel braucht Anführungszeichen.
- Nach dem Archiv fehlt die Spec unter `openspec/specs/`: die Sync-Frage wurde verneint. Den Schritt mit der Kopie des Projektordners wiederholen und die Frage bejahen.
- Schritt wiederholen: vor Schritt 4 eine Kopie des Projektordners anlegen (`cp -R todo-app todo-app-vor-apply`) und mit der Kopie neu beginnen.
