# SDD – Lernumgebung

Lernumgebung für den Kurs **Spec-Driven Development mit OpenSpec**. Sie bringt alle Werkzeuge mit. Die Projekte der Übungen legt ihr selbst an.

## Start mit DevContainer

Voraussetzungen: [Docker Desktop](https://www.docker.com/products/docker-desktop) und [VS Code](https://code.visualstudio.com/) mit der Erweiterung **Dev Containers** (`ms-vscode-remote.remote-containers`).

1. Repository klonen und den Ordner in VS Code öffnen
2. „Reopen in Container“ wählen, oder über die Befehlspalette: _Dev Containers: Reopen in Container_
3. Warten, bis der Container gebaut ist (einmalig einige Minuten)
4. Einmal anmelden: im Terminal `claude` starten oder in der Claude-Code-Erweiterung. Die Anmeldung gilt für beide.

Die Anmeldung liegt in einem Volume auf `~/.claude` und bleibt bei „Rebuild Container“ erhalten. Öffnet sich im Container kein Browser, die Adresse aus dem Terminal im Browser des eigenen Rechners öffnen und den Code einfügen. Ist `ANTHROPIC_API_KEY` gesetzt, benutzt Claude Code den Schlüssel statt der Anmeldung.

Im Container stehen bereit:

| Werkzeug | Version | Prüfen |
|---|---|---|
| OpenSpec | 2.0.0 | `openspec --version` |
| Claude Code | 2.1.322 | `claude --version` |
| Node.js | 22 | `node --version` |
| OpenFastTrace | 4.11.0, mit Java 21 | `java -jar "$OFT_JAR" help` |

Dazu git, jq und Python 3 mit pytest. Andere Agenten als Claude Code bekommen ihre Anweisungen über `openspec instructions`.

## Aufbau

| Ordner | Inhalt |
|---|---|
| `aufgaben/` | die acht Aufgabenblätter in Kursreihenfolge, `01-openspec-einrichten.md` bis `08-nachweis.md` |
| `vorlagen/` | Dateien zum Kopieren in den eigenen Projektordner: `config.yaml` je Übung, ADR-Vorlage nach MADR, Schema mit ADR-Prüfung und unter `startprojekte/` drei vorbereitete Projektordner für die Übungen Parallel, Pipeline und Nachweis. Übersicht in `vorlagen/README.md`. |
| `projekte/` | eure eigenen Projektordner. Der Ordner steht nicht unter Versionskontrolle dieses Repositorys. |

In den Aufgabenblättern steht `<kurs>` für den Ordner dieses Repositorys, im Container `/workspaces/<name-des-repositorys>`.

## Ablauf einer Übung

Jede Übung beginnt mit einem eigenen Projektordner unter `projekte/`.

Neues Projekt, etwa für „OpenSpec einrichten“ und das Hello World:

```bash
mkdir -p projekte/hello && cd projekte/hello
openspec init --tools claude
```

Projekt mit Vorlagen, etwa die ToDo-App:

```bash
mkdir -p projekte/todo-app && cd projekte/todo-app
openspec init --tools claude
cp ../../vorlagen/config/todo-app.yaml openspec/config.yaml
mkdir -p adr && cp ../../vorlagen/adr/0000-vorlage.md adr/
```

Projekt aus einem Startprojekt, etwa „Zwei Changes, ein Requirement“:

```bash
mkdir -p projekte/parallel && cp -R vorlagen/startprojekte/parallel/. projekte/parallel/
cd projekte/parallel
```

Die genauen Schritte stehen im jeweiligen Aufgabenblatt. Zurücksetzen heißt: den Projektordner löschen und neu anlegen. Wer mit der eigenen ToDo-App zurückliegt, bekommt vom Trainer einen fertigen Projektordner. Lösungen hat der Trainer.

## Prüfen

Im Terminal, im Projektordner:

```bash
openspec validate --all --strict
node --test
```

In VS Code: `Ctrl+Shift+B` startet **OpenSpec: Validieren**, _Tasks: Run Test Task_ startet die Tests. Beide wirken auf das Projekt der gerade geöffneten Datei.

## Ohne DevContainer

Für eine lokale Installation: OpenSpec 2.0.0 (`npm install -g @fission-ai/openspec@2.0.0`, braucht Node.js 22.12 oder neuer), Claude Code oder ein anderer Coding-Agent, git, für die Trace-Vorführung Java 17 oder neuer und OpenFastTrace 4.11.0 als JAR.

## Claude Code und Daten

Claude Code ist ein Produkt von Anthropic und unterliegt dessen Nutzungsbedingungen. Jede Person meldet sich mit einem eigenen Konto oder einem vom Arbeitgeber gestellten API-Schlüssel an, ein Konto wird nicht geteilt. Eingaben und Dateien, die der Agent liest, gehen an Anthropic. Weitergegeben wird nur dieses Repository mit dem Dockerfile, kein vorgebautes Image.

Die Nutzungsdaten und die Update-Abfrage von OpenSpec sind im Container abgeschaltet (`OPENSPEC_TELEMETRY=0`, `OPENSPEC_NO_UPDATE_CHECK=1`).

## Lizenz und Marken

Eigener Code und eigene Texte stehen unter MIT, siehe `LICENSE`. Was `openspec init` erzeugt, steht unter der MIT-Lizenz von OpenSpec. OpenFastTrace steht unter GPL-3.0. Das Dockerfile lädt das JAR beim Bau von Maven Central, dieses Repository enthält es nicht.

OpenSpec, Claude Code, OpenFastTrace, Docker und Visual Studio Code sind Namen oder Marken ihrer Inhaber. Dieses Material ist von deren Herstellern unabhängig.
