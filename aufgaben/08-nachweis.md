# Aufgabe: Vom Requirement zum Ticket

Zu einem Requirement der ToDo-App findet ihr den archivierten Change, der es eingeführt hat, und den Ticketschlüssel dazu. Danach geht ihr den umgekehrten Weg, von einer Funktion im Code zurück zum Requirement. Erlaubt sind Befehle von OpenSpec, die Dateisuche im Editor oder mit `grep` und in Schritt 5 `git`. Etwa 15 Minuten für die Schritte und 5 Minuten Auswertung, allein.

## Start

Einen eigenen Projektordner anlegen und das Startprojekt hineinkopieren: `mkdir -p <kurs>/projekte/nachweis && cp -R <kurs>/vorlagen/startprojekte/nachweis/. <kurs>/projekte/nachweis/ && cd <kurs>/projekte/nachweis`. Er enthält `openspec/` der ToDo-App nach dem Fall TODO-1 (Export und Import): die Hauptspec `aufgaben` und sechs archivierte Changes, keinen offenen. Dazu liegt dort `verlauf.bundle`, der Git-Verlauf desselben Projekts mit Code, für Schritt 5. `openspec list` meldet „No active changes found.“, `openspec list --archived` nennt die sechs Changes.

## Schritte

1. Requirement wählen.
   `openspec list --specs` zeigt die Capabilities. `openspec show aufgaben --type spec` zeigt die Requirements. Wählt eines zum Export oder Import und notiert den Namen genau so, wie er hinter `### Requirement:` steht.
2. Change finden.
   Im Archiv nach dem Namen suchen, mit der Dateisuche des Editors im Ordner `openspec/changes/archive` oder so:
   ```bash
   grep -rn "Requirement: <Name>" openspec/changes/archive
   ```
   Jeder Treffer liegt im Delta eines Change unter `openspec/changes/archive/<Datum>-<Name>/specs/`. Steht das Requirement dort unter `## ADDED Requirements`, hat dieser Change es eingeführt.
3. Ticketschlüssel finden.
   Öffnet `proposal.md` im Archivordner des Change und sucht den Ticketschlüssel, etwa TODO-1. `openspec show` hilft hier nicht: Einen archivierten Change kennt der Befehl nicht.
4. Dieselbe Kette ohne Schlüssel.
   Schritt 2 und 3 für „Überfällige Aufgaben oben“ wiederholen. Steht im Proposal dieses Change ein Ticketschlüssel? Notiert, an welcher Stelle die Kette dann reißt und was noch weiterführt.
5. Vom Code zurück zum Requirement.
   Den Git-Verlauf auspacken und fragen, welcher Commit die Funktion `export_text` geschrieben hat:
   ```bash
   git clone -q -b main verlauf.bundle verlauf && cd verlauf
   git blame -s -L '/^function export_text/,+2' src/tasks.js
   git show --stat --format='%h %s' <Commit>
   git log --oneline
   ```
   Nennt die Nachricht dieses Commits den Change oder den Ticketschlüssel? Ändert der Commit eine Datei unter `openspec/`? Welcher Commit im Verlauf führt weiter zum Change, und von dort zu welchem Requirement? Notiert die Stelle, an der dieser Weg reißt.

Ergebnis: eine Zeile mit Requirement, Change und Ticketschlüssel, dazu die Stelle, an der die Kette ohne Schlüssel reißt, und die Stelle, an der der Weg vom Code zurück reißt.

## Auswertung

- An welcher Stelle hing der Rückweg an einer Gewohnheit statt an einem Werkzeug?
- Was hätte im Commit oder im Code stehen müssen, damit der Weg von `export_text` zum Requirement ohne Suchen gelingt?
- Was führt ohne Ticketschlüssel noch vom Change zum Ticket?
- Was müsste im Proposal oder in `.openspec.yaml` des Change stehen, damit der Rückweg ohne Suchen gelingt? Die Doku von OpenSpec nennt für eigene Angaben Schlüssel mit `x-` in `.openspec.yaml`, etwa `x-jiraKey: TODO-1`. OpenSpec liest sie nicht und behält sie beim Archivieren.

## Zusatz für Schnelle

Nehmt ein Requirement, das spätere Changes geändert haben, etwa „Aufgabe anlegen“. Sucht erst nach dem Namen, dann nach einem Satzteil aus dem Rumpf, etwa „Priorität mittel“. Welche Changes erscheinen jeweils, und welcher hat das Requirement eingeführt?

Zu Schritt 5: Dieselbe Frage für `backup_text` in `src/backup.js`. Welcher Commit hat die Funktion geschrieben, und gibt es ein Requirement dazu?

## Falls ihr hängt

- Die Suche findet nichts: Der Name muss genau stimmen, auch in Groß- und Kleinschreibung. `grep -rni` findet beide Schreibweisen.
- Mehrere Treffer: Ein Requirement steht in jedem Change, der es eingeführt (ADDED) oder geändert (MODIFIED) hat. Eingeführt hat es der Change mit ADDED.
- `openspec show <Datum>-<Name>` meldet „Unknown item“: Archivierte Changes zeigt `openspec show` nicht. Die Dateien im Archivordner lassen sich direkt öffnen.
- `git clone` meldet einen fehlenden Verweis HEAD: Die Option `-b main` fehlt.
- `git blame` meldet „no match“: Der Befehl läuft im Ordner `verlauf`, die Funktion steht in `src/tasks.js`.
- Kein Ticketschlüssel im Proposal: Dann führen nur Change-Name, Datum und Text des Proposals weiter. Das ist ein Ergebnis für die Auswertung, kein Fehler der Übung.
