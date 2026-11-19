# Aufgabe: ToDo-App im Browser

## Auftrag

Es soll eine ToDo-App im Browser mit SDD entwickelt werden. Neue ToDos können angelegt, bearbeitet oder gelöscht werden. ToDos können nach Priorität sortiert werden. Die Daten sollen im Local Storage des Browsers gespeichert werden. Diese Entscheidung soll als ADR im MADR-Format festgehalten werden. Die Anwendung soll mit reinem JavaScript ohne Bibliotheken entwickelt werden. Am Ende sollen alle Changes archiviert sein.

Die App entsteht in vier Schritten, jeder Schritt ist ein Change. Fertig ist ein Schritt, wenn `openspec validate <name> --strict` vor dem Archivieren keinen Fehler meldet und der Change danach archiviert ist.

Lösungsstände zu jedem Schritt hat der Trainer. `<kurs>` ist der Ordner der Lernumgebung oder des Kursrepositorys.

## Start (5 Minuten)

Einen neuen, leeren Projektordner anlegen, darin `openspec init --tools claude` ausführen und einen Ordner `adr/` anlegen. Der Projektkontext in `openspec/config.yaml` nennt Ziel, Technik und die Sprache der Artefakte und verweist auf `adr/`. In Spec und ADR heißt ein ToDo „Aufgabe“, die Capability `aufgaben`.

## Schritt 1: ToDos anlegen und anzeigen (20 Minuten)

Ziel: „ToDos lassen sich anlegen und in einer Liste anzeigen.“

Vorher mit `/openspec-explore` den Umfang klären, dann `/openspec-propose`. Danach die Artefakte lesen, `openspec validate <name> --strict`, `/openspec-apply-change`, Seite öffnen, `/openspec-archive-change`. So in jedem Schritt.

## Schritt 2: ToDos bleiben nach dem Neuladen erhalten (20 Minuten)

Ziel: „ToDos bleiben nach dem Neuladen erhalten.“

Mit `/openspec-explore` Optionen zum Speicherort sammeln. Danach in derselben Sitzung mit einer normalen Eingabe, ohne Skill, das ADR anfordern: Die Entscheidung für Local Storage steht als ADR nach MADR in `adr/`, der Agent schreibt es nach der Vorlage (vorher `cp <kurs>/vorlagen/adr/0000-vorlage.md adr/`).

Das ADR prüfen: Nennt es mindestens zwei Optionen mit dem Grund der Ablehnung? Nennt Confirmation, woran ein Review die Entscheidung prüft? Fehlendes vom Agenten ergänzen lassen. Dann `/openspec-propose`.

## Schritt 3: ToDos bearbeiten und löschen (15 Minuten)

Ziel: „ToDos lassen sich bearbeiten und löschen.“

Change mit `/openspec-propose`. Vor dem Umsetzen wird das Delta geprüft.

Zu zweit: Delta tauschen, lesen und eine Rückfrage notieren. Leitfrage: Ist die Spec präzise genug für die Übergabe an den Agenten? Danach den Prüf-Agenten starten und seinen Befund mit der Rückfrage vergleichen.

Allein: direkt den Prüf-Agenten starten.

Der Prüf-Agent prüft das Delta in einer neuen Session (zweites Terminal, `claude` im Projektordner), mit diesem Auftrag:

> Prüfe den Change unter `openspec/changes/<name>/` gegen `openspec/specs/` und `adr/`. Ändere keine Datei. Liste je Fund die Stelle und die Frage, die offen bleibt.

Zwei Funde auswählen und in der ersten Session im Delta beantworten lassen (`/openspec-update-change`), den Rest verwerfen. Danach umsetzen und archivieren.

## Schritt 4: Priorität und Sortierung (15 Minuten)

Ziel: „ToDos bekommen beim Anlegen eine Priorität und lassen sich danach sortieren.“

Change mit `/openspec-propose`. Nach dem Umsetzen und vor dem Archivieren sucht ein Prüf-Agent in einer neuen Session Lücken, mit diesem Auftrag:

> Lies `openspec/specs/`, `openspec/changes/<name>/`, `src/` und `test/`. Nenne jede Stelle, an der der Code etwas entscheidet, das die Spec nicht festlegt. Ändere keine Datei.

Je Stelle entscheiden: Szenario ergänzen lassen, in `design.md` festhalten lassen oder liegen lassen. Mindestens eine Stelle als Szenario nachziehen, dann archivieren.

Ergebnis: Die App läuft im Browser, vier Changes sind archiviert, `openspec list` zeigt keine offenen Changes.

## Auswertung (5 Minuten)

- Was hat der Agent entschieden, das im Ziel nicht stand?
- Welchen Fund des Prüf-Agenten habt ihr übernommen, welchen verworfen?
- Welche dieser Entscheidungen gibt bei euch ein Mensch frei?

## Zusatz für Schnelle

Ein fünfter Change: Erledigte ToDos lassen sich ausblenden. Vorher prüfen, ob das ein neues Requirement ist oder ein MODIFIED an „anzeigen“.

## Falls ihr hängt

- Einrichten: `openspec init --tools claude`, danach Claude Code im Projektordner neu starten. Vorlage für die `config.yaml`: `<kurs>/vorlagen/config/todo-app.yaml`.
- ADR: Der Skill `/openspec-explore` schreibt nur Dateien des Change. Das ADR deshalb nach dem Erkunden mit einer normalen Eingabe anfordern.
- Fragt der Agent beim Archivieren nach: die Warnung zu einer fehlenden `design.md` bestätigen und „Sync now“ wählen. Ohne Sync fehlt die Änderung in `openspec/specs/`.
- Formfehler im Change: `openspec validate <name> --strict`.
- Ein geändertes Requirement steht unter MODIFIED mit allen bisherigen Szenarien, sonst bricht `archive` ab.
- Tests ohne Bibliothek: `node --test`. Der Test übergibt den Speicher als Objekt, weil es localStorage in Node nicht ohne Weiteres gibt.
- Seite öffnen: `open index.html`, unter Windows `start index.html`, unter Linux `xdg-open index.html`.
- Schritt wiederholen: vor dem Schritt eine Kopie des Projektordners anlegen (`cp -R todo-app todo-app-vor-schritt-3`) und mit der Kopie neu beginnen.
