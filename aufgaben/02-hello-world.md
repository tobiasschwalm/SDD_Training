# Aufgabe: Hello World in SDD

Eine kleine Browser-Anwendung in Plain JavaScript, ohne Bibliotheken, in zwei Teilen. Jeder Teil ist ein Change bis zum Archiv. Gearbeitet wird im Ordner aus „Übung: OpenSpec einrichten“.

## Start

Im eigenen Projektordner aus „Übung: OpenSpec einrichten“ (`<kurs>/aufgaben/01-openspec-einrichten.md`). Voraussetzung: ein Browser. Wer nicht mit Claude Code arbeitet, findet den Weg über die Kommandozeile unter „Ohne Claude Code“.

## Teil 1: Hello, World!

1. In `openspec/config.yaml` unter `context` festhalten: Browser-Anwendung, Plain JavaScript, keine Bibliotheken, Tests mit `node --test`.
2. Wunsch an den Agenten mit `/openspec-explore`: „Eine Seite zeigt beim Öffnen ‚Hello, World!‘.“ Der Agent fragt nach, etwa ob die Seite sonst etwas zeigt oder wie sie heißt. Zwei, drei Antworten geben, dann in derselben Sitzung `/openspec-propose` (kein `/clear` dazwischen, sonst ist das Geklärte weg).
3. Die Spec im Change prüfen und nachbessern: Steht Technik darin, die in die `config.yaml` gehört? Welche Annahmen hat der Agent still getroffen, welche Begriffe sind vage? Die Spec liegt unter `openspec/changes/<name>/specs/`, den Namen zeigt `openspec list`. Den Agenten nachbessern lassen, danach `openspec validate <name> --strict`.
4. `tasks.md` lesen: Nennt jede Aufgabe, wie sie geprüft wird? Falls der Agent eine `design.md` angelegt hat: Welche Entscheidungen hat er getroffen?
5. Mit `/openspec-apply-change` umsetzen lassen, `index.html` im Browser öffnen.
6. Mit `/openspec-archive-change` abschließen und die neue Spec unter `openspec/specs/` ansehen.

Ergebnis: Die Seite zeigt „Hello, World!“, und unter `openspec/specs/` liegt die neue Spec.

## Teil 2: Optional ein Name

1. Wunsch mit `/openspec-propose`: „Optional lässt sich ein Name eingeben. Dann lautet die Begrüßung ‚Hello, Name!‘.“
2. Das Delta prüfen und nachbessern lassen: Was ändert sich an der Spec, und was hat der Agent angenommen, das im Wunsch nicht steht? Ändert der Wunsch ein bestehendes Requirement, steht es unter MODIFIED mit allen bisherigen Szenarien. Steht nur ADDED im Delta: prüfen, ob das Requirement aus Teil 1 noch stimmt.
3. Mit `/openspec-apply-change` umsetzen lassen, die Seite im Browser öffnen.
4. Vor dem Archivieren mit `openspec show <name> --diff` lesen, wie sich die Hauptspec ändert. Dann mit `openspec archive <name>` abschließen und die Hauptspec unter `openspec/specs/` ansehen.

Ergebnis: Mit dem Namen Ada zeigt die Seite „Hello, Ada!“, ohne Namen weiter „Hello, World!“.

## Auswertung

- Was hat der Agent ergänzt, das im Wunsch nicht stand?
- Welche Zeile der Spec hat sich in Teil 2 geändert, und steht sie nach dem Archivieren so in der Hauptspec, wie der Diff sie zeigte?
- Steht in der Spec Verhalten oder Technik?

## Zusatz für Schnelle

Ein dritter Change: Die Begrüßung soll den Namen ohne Leerzeichen am Rand zeigen. Vorher mit `/openspec-explore` klären, was mit einem leeren Namen passiert.

## Ohne Claude Code

Namen selbst wählen, etwa `add-hello-world`, und je Artefakt die Anweisung holen. Die Dateien schreibt euer Agent:

```bash
openspec new change <name>
openspec instructions proposal --change <name>   # danach specs, design, tasks
openspec instructions apply --change <name>
openspec archive <name> --yes
```

## Falls ihr hängt

- Die Skills wie `/openspec-propose` fehlen: Agent im Projektordner neu starten.
- Formfehler im Change: `openspec validate <name> --strict`.
- Vorlage für die `config.yaml`: `<kurs>/vorlagen/config/hello-sdd.yaml`.
- Fragt der Agent in Teil 1 beim Archivieren nach: die Warnung zur fehlenden `design.md` bestätigen und „Sync now“ wählen. Ohne Sync bleibt `openspec/specs/` leer.
- Ein geändertes Requirement steht unter MODIFIED mit allen bisherigen Szenarien, sonst bricht `archive` ab.
- Seite öffnen: `open index.html`, unter Windows `start index.html`, unter Linux `xdg-open index.html`.
- Lösungsstände zu beiden Teilen hat der Trainer.
