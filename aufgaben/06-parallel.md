# Aufgabe: Zwei Changes, ein Requirement

Zwei vorbereitete Changes der ToDo-App ändern dasselbe Requirement „Aufgabe löschen“. Beide behalten alle Szenarionamen und ändern nur Text. OpenSpec meldet in diesem Fall nichts. Ihr archiviert den ersten, lest den Spec-Diff, bevor ihr den zweiten archiviert, und führt beide Änderungen zusammen. Etwa 25 Minuten mit Auswertung, nur die Kommandozeile, kein Agent.

## Start

Einen eigenen Projektordner anlegen und das Startprojekt hineinkopieren: `mkdir -p <kurs>/projekte/parallel && cp -R <kurs>/vorlagen/startprojekte/parallel/. <kurs>/projekte/parallel/ && cd <kurs>/projekte/parallel`. Er enthält die ToDo-App nach Tag 1, nur `openspec/`, ohne Code. Die Changes liegen fertig unter `openspec/changes/`, ihre Tasks sind abgehakt:

- A `loeschen-mit-titel`: Die Rückfrage beim Löschen nennt den Titel der Aufgabe.
- B `loeschen-dauerhaft`: Eine gelöschte Aufgabe fehlt auch nach dem Neuladen.

## Schritte

1. Vorhersagen, 5 Minuten mit Schritt 2. Beide Deltas lesen (`openspec/changes/*/specs/aufgaben/spec.md`) und in einer Zeile aufschreiben, wie der Requirement-Satz von „Aufgabe löschen“ lauten soll, wenn beide Changes archiviert sind.
2. Beide Changes prüfen, den ersten archivieren.
   ```bash
   openspec validate loeschen-mit-titel --strict
   openspec validate loeschen-dauerhaft --strict
   openspec archive loeschen-mit-titel --yes
   ```
   Beide Prüfungen melden „is valid“.
3. Den Spec-Diff von B lesen, 5 Minuten.
   ```bash
   openspec show loeschen-dauerhaft --diff
   ```
   Der Diff steht unter „Specifications Changed (diffs)“, darüber steht das Proposal. Er vergleicht das Delta von B mit der Hauptspec von jetzt. Welche Minus-Zeilen hat B nie geschrieben? Sie stammen aus A und gingen verloren, würde B jetzt archiviert.
4. Delta von B korrigieren und archivieren, 5 Minuten.
   a. In `openspec/changes/loeschen-dauerhaft/specs/aufgaben/spec.md` die Änderungen von A übernehmen: den Requirement-Satz und die beiden WHEN-Zeilen mit der Rückfrage „„Milch kaufen“ löschen?“. Die Änderung von B bleibt stehen.
   b. `openspec validate loeschen-dauerhaft --strict` und noch einmal `openspec show loeschen-dauerhaft --diff`. Jetzt zeigt der Diff nur noch die Änderung von B: den verlängerten Requirement-Satz als Minus- und Plus-Zeile und die neue AND-Zeile. Die beiden WHEN-Zeilen sind unverändert.
   c. Archivieren und die Hauptspec prüfen:
   ```bash
   openspec archive loeschen-dauerhaft --yes
   openspec validate --all --strict
   ```
   In `openspec/specs/aufgaben/spec.md` stehen unter „Aufgabe löschen“ Titel und Neuladen nebeneinander, kein Text von A fehlt.
5. Eine Teamregel in drei Sätzen aufschreiben, 5 Minuten: Wer archiviert wann, was liest der Archivierende vorher im Spec-Diff, wer ist für die Capability verantwortlich.

Ergebnis: eine Hauptspec mit beiden Änderungen und eine Teamregel.

## Auswertung

5 Minuten.

- Ohne Schritt 3 wäre B einfach archiviert worden. Welcher Befehl hätte gewarnt, und an welcher Stelle hätte ein Mensch den Verlust noch gesehen?
- Arbeiten A und B auf zwei Branches: Wann meldet Git einen Konflikt in der Hauptspec, und wann nicht?
- Welche der drei Regeln kostet das Team am wenigsten und findet den Fall trotzdem?

## Zusatz für Schnelle

- Der laute Fall: Aus dem Delta von B das Szenario „Löschen abbrechen“ löschen (vor Schritt 4 oder in einer frischen Kopie nach Schritt 2). Dann `openspec validate loeschen-dauerhaft --strict` und `openspec archive loeschen-dauerhaft --yes`. Welche Meldungen kommen, und warum findet OpenSpec diesen Fall, den aus Schritt 3 aber nicht?
- Der stille Fall: In einer frischen Kopie A und B ohne Schritt 3 und 4 nacheinander archivieren, dann `openspec/specs/aufgaben/spec.md` lesen. Beide Befehle enden ohne Warnung. Was fehlt in der Hauptspec?

## Falls ihr hängt

- `archive` meldet „current spec contains scenario(s) not present in the modified block“ und „Aborted. No files were changed.“: Im Delta von B fehlt ein Szenario oder ein Szenarioname ist verändert. Beide Szenarien mit unverändertem Namen müssen im MODIFIED-Block stehen.
- `validate` meldet in Schritt 4 einen Fehler: Jedes Szenario beginnt mit `#### Scenario:` und hat eine Zeile mit **WHEN** und eine mit **THEN**.
- Zurück auf den Anfang: den Ordner löschen und neu aus dem Kursrepository kopieren.
- Die Lösung (korrigiertes Delta, Hauptspec mit beiden Änderungen, Teamregel, Ausgaben) hat der Trainer.
