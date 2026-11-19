# Aufgabe: Schema mit ADR-Prüfung übernehmen

Das Community-Schema `spec-driven-with-adr` setzt zwischen `design` und `tasks` ein Artefakt `adr`: eine ADR-Prüfung je Change. Ihr übernehmt es in die ToDo-App, macht es zum Standard und passt seine Anweisung an das Projekt an. Alles andere bleibt wie im Original, so lassen sich spätere Fassungen des Schemas leicht übernehmen. An einem Probe-Change seht ihr, was das Schema im Status zeigt und was es nicht erzwingt. 40 Minuten.

## Start (5 Minuten)

Im eigenen Projektordner der ToDo-App nach der Übung „ToDo-App unter Testregeln“, alle Changes archiviert. Wer den Ordner nicht hat, bekommt vom Trainer den Ordner `todo-tag2`, legt ihn als neuen Projektordner `<kurs>/projekte/todo` ab und richtet ihn ein:

```bash
cd <kurs>/projekte/todo && openspec init --tools claude
```

Prüfen: `openspec list` meldet „No active changes found.“, unter `adr/` liegen `0001-tasks-in-local-storage.md` und `0002-logic-without-dom.md`. Das Schema liegt zum Kopieren in `<kurs>/vorlagen/schema/spec-driven-with-adr/`, Herkunft und Lizenz stehen dort in `LICENSE`. Jeder Befehl `openspec schema …` meldet zuerst „Note: Schema commands are experimental and may change.“ Das ist kein Fehler.

## Schritte

1. Schema übernehmen und prüfen (5 Minuten mit Schritt 2).
   ```bash
   mkdir -p openspec/schemas
   cp -R <kurs>/vorlagen/schema/spec-driven-with-adr openspec/schemas/
   openspec schema validate spec-driven-with-adr
   openspec schemas
   ```
   Erwartet: „Schema 'spec-driven-with-adr' is valid“ und die Folge `proposal → specs → design → adr → tasks`.
2. Schema als Standard setzen. In `openspec/config.yaml` die erste Zeile `schema: spec-driven` ersetzen durch `schema: spec-driven-with-adr`. Für ein kopiertes Schema gibt es dafür keinen Befehl, `--default` kennt nur `openspec schema init`.
3. Die Anweisung des Artefakts lesen und anpassen (20 Minuten). Das Artefakt steht in `openspec/schemas/spec-driven-with-adr/schema.yaml` unter `- id: adr`, die Anweisung dort bei `instruction:`.
   a. Lesen und je Frage die Stelle markieren: Was gehört in die Datei `adr.md` im Change, was nach `adr/NNNN-titel.md`? Ab wann verlangt die Anweisung ein neues ADR (Schritt 2 der Anweisung)? Was gilt, wenn nichts die Hürde nimmt („do not invent ADRs“)? Wie ändert sich eine angenommene Entscheidung („IRON RULE“)?
   b. Die Auslöser des Projekts eintragen. Die Hürde in Schritt 2 ist allgemein formuliert. Eine Liste macht sie für dieses Projekt prüfbar, etwa „Eine öffentliche Schnittstelle entsteht oder ändert sich“. Direkt nach dem Satz, der mit „prior).“ endet (Ende von Schritt 2), zwei Leerzeilen und diesen Block einfügen, wie zwischen den anderen Absätzen der Anweisung. Die Textzeilen beginnen mit sechs Leerzeichen, die Spiegelstriche mit acht:
      ```yaml
      In diesem Projekt bindet eine Entscheidung langfristig, wenn einer dieser
      Auslöser zutrifft:

        - Neue Abhängigkeit zu einem Modul oder externen Dienst
        - Neue Komponente, neuer Dienst oder neue Technologie
        - Mehr als eine Capability betroffen
        - Eine öffentliche Schnittstelle entsteht oder ändert sich
        - Schwer umkehrbar: Datenmodell, Protokoll, Speicherort
        - Eine Architekturregel oder ihre Ausnahmeliste ändert sich

      Den zutreffenden Auslöser im ADR und in adr.md nennen.
      ```
   c. Das Format des Projekts eintragen. `adr/0001-tasks-in-local-storage.md` öffnen und mit der Klammer in Schritt 3 der Anweisung vergleichen: Welche Abschnitte hat ADR 0001, die „MADR-short“ nicht nennt? Dann die Klammer `(MADR-short: title, status/date, context, decision, consequences).` ersetzen, etwa durch: `MADR 4.0.0 wie adr/0001-tasks-in-local-storage.md, auf Deutsch, mit Considered Options und Confirmation. Das Feld Supersedes steht im Kopf als supersedes: ADR-NNNN.`
4. Erneut prüfen und einen Probe-Change ohne `--schema` anlegen (5 Minuten mit Schritt 5).
   ```bash
   openspec schema validate spec-driven-with-adr
   openspec new change todo-1-probe
   openspec status --change todo-1-probe
   openspec instructions adr --change todo-1-probe
   ```
   Erwartet: `schema validate` meldet „Schema 'spec-driven-with-adr' is valid“, `new change` meldet „Schema: spec-driven-with-adr“. `status` zeigt `adr (blocked by: design)` und `tasks (blocked by: specs, adr)`. Im Block `<instruction>` stehen eure Auslöser und euer Format, unter „Write to:“ der Pfad zu `adr.md` im Change.
5. Im Probe-Change eine `tasks.md` anlegen, ohne `adr.md`. Vorher aufschreiben: Was melden `status`, `validate` und `archive`? Dann ausführen. Die Zeile `skip_specs: true` sagt, dass der Probe-Change kein Verhalten ändert, sonst lehnt `validate` ihn wegen des fehlenden Deltas ab.
   ```bash
   echo "- [x] 1.1 Probe" > openspec/changes/todo-1-probe/tasks.md
   echo "skip_specs: true" >> openspec/changes/todo-1-probe/.openspec.yaml
   openspec status --change todo-1-probe
   openspec validate todo-1-probe --strict
   openspec archive todo-1-probe --yes
   rm -r openspec/changes/archive/*-todo-1-probe
   ```
   Mit der eigenen Vorhersage vergleichen. Die letzte Zeile löscht den Probe-Change aus dem Archiv.

Ergebnis: ein gültiges Schema mit euren Auslösern und eurem ADR-Format, als Standard gesetzt. Vor den Tasks sieht es einen Prüfvermerk `adr.md` im Change vor, neue ADRs entstehen unter `adr/`.

## Auswertung (5 Minuten)

- Was hält ein fehlendes `adr.md` auf: `status`, `validate`, `archive` oder keines davon?
- Das Schema zeigt die Reihenfolge nur im Status. Wer prüft im Team, ob die ADR-Prüfung stattfand und ob ein ADR fehlt?
- Welcher Auslöser fehlt für euer eigenes System?

## Zusatz für Schnelle

- Zyklus: unter `- id: adr` bei `requires:` die Zeile `- design` in `- tasks` ändern, `openspec schema validate spec-driven-with-adr` lesen, zurückändern.
- Die Anweisung für `design` im selben Schema lesen: Was soll der Agent mit den geltenden ADRs tun, bevor er entwirft, und was mit einem ADR, das er überdenken will?

## Falls ihr hängt

- „Unknown schema 'spec-driven-adr'. Available: …“ bei `new change`: Der Eintrag `schema:` in `config.yaml` muss genau dem Ordnernamen unter `openspec/schemas/` entsprechen.
- `new change` meldet „Schema: spec-driven“: Die Änderung an `config.yaml` ist nicht gespeichert.
- Die Einfügestellen nicht gefunden: Im Original steht „prior).“ allein in Zeile 255 von `schema.yaml`, die Klammer mit „MADR-short“ in Zeile 261.
- `schema validate` meldet einen YAML-Fehler: Einrückung des eingefügten Blocks prüfen. Er gehört zu `instruction:` und ist mindestens so tief eingerückt wie der Text darüber.
- Im Block `<instruction>` stehen die Auslöser in einer Zeile: Die Spiegelstriche brauchen zwei Leerzeichen mehr als der Text davor.
- Auflösung zu Schritt 5: `status` zeigt `[x] tasks`, obwohl `adr` offen ist. `validate` meldet „Change 'todo-1-probe' is valid“, `archive` archiviert den Change. Nur `openspec instructions tasks` warnt den Agenten vor fehlenden Vorgängern.
- `openspec schema list` gibt es nicht, die Liste heißt `openspec schemas`.
- Die Lösung (angepasstes Schema und `config.yaml`) und das Protokoll des Laufs hat der Trainer.
