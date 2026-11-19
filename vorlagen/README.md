# Vorlagen

Dateien zum Kopieren in den eigenen Projektordner. `<kurs>` ist der Ordner der Lernumgebung oder des Kursrepositorys. Keine dieser Dateien ist Teil von OpenSpec.

| Datei | Zweck | Benutzt in |
|---|---|---|
| `adr/0000-vorlage.md` | gekürzte ADR-Vorlage nach MADR 4.0.0, nach `adr/` kopieren. Herkunft und Lizenz (MIT OR CC0-1.0) stehen in `adr/HERKUNFT.md`. | ToDo-App mit SDD |
| `config/hello-sdd.yaml` | `openspec/config.yaml` für das Hello World: Projektkontext und zwei Regeln für Tasks | Hello World in SDD |
| `config/todo-app.yaml` | `openspec/config.yaml` für die ToDo-App: Projektkontext mit Verweis auf `adr/`, Regeln für Design und Tasks | ToDo-App mit SDD |
| `config/testregeln.yaml` | Abschnitte `rules` und `operations` mit den Testregeln. Ersetzt in der `config.yaml` der ToDo-App alles ab `rules:`. | ToDo-App unter Testregeln |
| `config/ids-openfasttrace.yaml` | `openspec/config.yaml` mit Regeln für IDs im Format von OpenFastTrace | Vorführung des Trainers zur Trace-Prüfung |
| `schema/spec-driven-with-adr/` | Community-Schema mit dem Zusatzartefakt `adr`, nach `openspec/schemas/` kopieren. Herkunft und Lizenz (MIT) stehen in `LICENSE` im Ordner. | Schema mit ADR-Prüfung übernehmen |
| `startprojekte/parallel/` | vorbereiteter Projektordner: `openspec/` der ToDo-App mit zwei fertigen Changes am selben Requirement | Zwei Changes, ein Requirement |
| `startprojekte/pipeline/` | vorbereiteter Projektordner: `openspec/` mit einem offenen Change und die Workflow-Datei `.github/workflows/openspec.yml` | Prüfung am Pull Request |
| `startprojekte/nachweis/` | vorbereiteter Projektordner: `openspec/` mit sechs archivierten Changes und `verlauf.bundle`, der Git-Verlauf des Projekts mit Code | Kurzaufgabe: Vom Requirement zum Ticket |

Kopieren, Beispiel für die ToDo-App:

```bash
cp <kurs>/vorlagen/config/todo-app.yaml openspec/config.yaml
mkdir -p adr && cp <kurs>/vorlagen/adr/0000-vorlage.md adr/
```

`openspec init` legt eine eigene `config.yaml` an. Die Vorlage ersetzt sie.

Ein Startprojekt kopieren, Beispiel für die Übung Parallel:

```bash
mkdir -p <kurs>/projekte/parallel && cp -R <kurs>/vorlagen/startprojekte/parallel/. <kurs>/projekte/parallel/
```

## Lizenz

Eigene Texte stehen unter MIT, siehe `LICENSE` im Wurzelordner der Lernumgebung. Fremde Teile nennen Herkunft und Lizenz im jeweiligen Ordner: das Schema in `schema/spec-driven-with-adr/LICENSE`, die ADR-Vorlage in `adr/HERKUNFT.md`.

OpenSpec, Claude Code und OpenFastTrace sind Namen oder Marken ihrer Inhaber. Dieses Material ist von deren Herstellern unabhängig.
