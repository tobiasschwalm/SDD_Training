# Aufgabe: Prüfung am Pull Request

Eine Workflow-Datei für GitHub Actions prüft an jedem Pull Request alle Specs und offenen Changes mit `openspec validate`. Ihr führt denselben Befehl lokal aus, baut zwei Fehler ein und seht, welchen die Prüfung findet. Etwa 20 Minuten mit Auswertung, nur die Kommandozeile, kein Agent, kein GitHub-Konto. Die Workflow-Datei liegt fertig im Ordner, sie läuft hier nicht.

## Start

Einen eigenen Projektordner anlegen und das Startprojekt hineinkopieren: `mkdir -p <kurs>/projekte/ci-pruefung && cp -R <kurs>/vorlagen/startprojekte/pipeline/. <kurs>/projekte/ci-pruefung/ && cd <kurs>/projekte/ci-pruefung`. Er enthält die ToDo-App nach Tag 1, nur `openspec/`, und den offenen Change `loeschen-dauerhaft`: Eine gelöschte Aufgabe fehlt auch nach dem Neuladen. Der Ordner `.github` ist versteckt, `ls -a` zeigt ihn.

## Schritte

1. Workflow lesen, 5 Minuten. `.github/workflows/openspec.yml` öffnen und zu jeder Zeile im Schritt „Specs und Changes prüfen“ sagen, was ohne sie passiert: ohne `@2.0.0`, ohne `--strict`, ohne `--all`, ohne `--no-interactive`, ohne `OPENSPEC_TELEMETRY`.
2. Denselben Befehl lokal ausführen und den Exit-Code lesen, 5 Minuten. `openspec` ist dasselbe Programm wie im Workflow, nur ohne `npx`:
   ```bash
   openspec validate --all --strict --no-interactive; echo "Exit $?"
   openspec validate --all --strict --no-interactive --json --report findings
   ```
   Exit 0 heißt: der Pull Request bleibt grün. Die JSON-Ausgabe beginnt mit `"kind": "validation-findings"` und `"version": "1.0"`. Unter `itemFindings` stehen nur Einträge mit Befund, hier noch keiner.
3. Fehler a, Requirement ohne Pflichtwort, 5 Minuten mit Schritt 4.
   a. In `openspec/changes/loeschen-dauerhaft/specs/aufgaben/spec.md` den Satz unter `### Requirement: Aufgabe löschen` ersetzen durch: „Die App entfernt eine Aufgabe nach einer Rückfrage aus der Liste. Eine gelöschte Aufgabe fehlt auch nach dem Neuladen der Seite.“
   b. Beide Befehle aus Schritt 2 wiederholen, dazu einmal ohne `--strict`. Welcher Exit-Code kommt mit und ohne `--strict`, und mit welcher Stufe (`level`) meldet OpenSpec den Befund?
   c. Zurück: den ursprünglichen Satz wieder einsetzen, „Die App SHALL eine Aufgabe nach einer Rückfrage aus der Liste entfernen. Eine gelöschte Aufgabe SHALL auch nach dem Neuladen der Seite fehlen.“
4. Fehler b, vages Requirement. Diesen Block an das Delta anhängen und Schritt 2 wiederholen:
   ```markdown
   ## ADDED Requirements

   ### Requirement: Schnelles Löschen
   Die App SHALL das Löschen von Aufgaben schnell und benutzerfreundlich machen.

   #### Scenario: Aufgabe schnell löschen
   - **WHEN** jemand eine Aufgabe löscht
   - **THEN** geht das schnell und ohne Umwege
   ```
   Was würde ein Reviewer an diesem Requirement fragen, und welche Zeile der Workflow-Datei hätte es aufgehalten?

Ergebnis: ein erprobter Prüfschritt für den Pull Request und ein Fehler, den er nicht findet.

## Auswertung

5 Minuten.

- Fehler a war ohne `--strict` nur eine Warnung mit Exit 0. Welche Folgen hätte das im Pull Request?
- Fehler b kam grün durch. Wer im Team sieht das Requirement, bevor es in die Hauptspec kommt?
- Welche der Prüfungen aus der Folie „Prüfungen am Pull Request“ würdet ihr zusätzlich in den Workflow nehmen?

## Zusatz für Schnelle

- Die Falle ohne Namen: `openspec validate --strict < /dev/null; echo "Exit $?"`. Ohne Terminal kann OpenSpec nicht nachfragen, welcher Change gemeint ist. Was meldet es, und was hieße das für den Job?
- Gegenprobe zum Schemaordner: In `openspec/changes/loeschen-dauerhaft/.openspec.yaml` die Zeile `schema: spec-driven` in `schema: spec-driven-adr` ändern, dann `openspec validate --all --strict --no-interactive` und `openspec status --change loeschen-dauerhaft`. Danach die Zeile zurückändern.

## Falls ihr hängt

- `echo "Exit $?"` zeigt immer 0: Der Befehl stand in einer Pipe, etwa mit `| head`. `$?` gilt dann für den letzten Befehl der Pipe.
- `validate` meldet „Nothing to validate“: `--all` fehlt.
- Ohne Netz läuft `npx` nicht. Die Übung braucht es nicht, lokal reicht `openspec` in Version 2.0.0 (`openspec --version`).
- Zurück auf den Anfang: den Ordner löschen und neu aus dem Kursrepository kopieren.
- Die Lösung mit allen Ausgaben hat der Trainer.
