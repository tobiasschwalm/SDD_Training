# Projektordner

Zwei Übungen aus der Sektion „OpenSpec einrichten und Changes anlegen“ im eigenen Projektordner: Einrichten und Hello World in SDD. Die ToDo-App läuft in einem zweiten, neuen Ordner nach demselben Muster.

Jeden Projektordner legt ihr selbst an, unter `<kurs>/projekte/`, Name nach Wahl. `<kurs>` ist der Ordner der Lernumgebung oder des Kursrepositorys. Vorlagen liegen unter `<kurs>/vorlagen/`.

## Übung: OpenSpec einrichten

1. Version prüfen und das Profil um verify ergänzen. Die Einstellung gilt für alle Projekte auf dem Rechner.

   ```bash
   openspec --version            # 2.0.0
   openspec config profile
   ```

   Im Dialog „Workflows only“ wählen, mit den Pfeiltasten zu „Verify change“ gehen (elfter Eintrag), mit der Leertaste ankreuzen, Enter. Den Hinweis auf `openspec update` übergehen, `init` folgt gleich.

2. Einen neuen, leeren Ordner anlegen und darin OpenSpec einrichten.

   ```bash
   mkdir -p <kurs>/projekte/hello && cd <kurs>/projekte/hello
   openspec init --tools claude  # oder euer Werkzeug
   ```

3. Den Agenten im Ordner starten und prüfen, ob die Skills erscheinen, etwa `/openspec-propose`.

4. Die erzeugten Dateien ansehen und beantworten:

   - Welche Skills hat `init` angelegt, und wo liegen sie?
   - Was lässt sich in `openspec/config.yaml` eintragen?
   - Welche der neuen Dateien gehören ins Repository?

Zurücksetzen: Ordner löschen und neu anlegen.

## Übung: Hello World in SDD

Im selben Ordner geht es weiter: Aufgabe `<kurs>/aufgaben/02-hello-world.md`.

## Übung: ToDo-App im Browser

In einem neuen, leeren Ordner, etwa `todo-app`, angelegt wie oben: Aufgabe `<kurs>/aufgaben/03-todo-app.md`.
