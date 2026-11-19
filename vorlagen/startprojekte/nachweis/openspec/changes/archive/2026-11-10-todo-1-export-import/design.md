## Context

Aufgaben sind `{ title, priority }` mit optionalem `due` als `"YYYY-MM-DD"` und liegen laut ADR 0001 als JSON-Text im Local Storage eines Browsers. Die Logik steht laut ADR 0002 ohne Zugriff auf die Seite in `src/tasks.js`, die Tests laufen mit `node:test`. Ticket TODO-1 verlangt Export und Import als Datei und sagt: „Exportierte Dateien bleiben auch nach späteren Versionen importierbar.“ Offen im Ticket waren Dateiformat und Dubletten, geklärt im Proposal.

Bestand ohne Spec: `src/backup.js` mit `backup_text(tasks)` liefert die Liste als JSON-Text, eine reine Liste ohne Formatangabe. Die Seite ruft die Funktion nicht auf, `test/backup.test.js` prüft sie, ein Requirement fehlt.

## Goals / Non-Goals

**Goals:**
- Eine Datei von heute lässt sich mit jeder späteren Version der App importieren, und ein Test hält das fest.
- Export und Import prüfen die Tests ohne Browser, `app.js` liest und schreibt nur die Datei.

**Non-Goals:**
- `src/backup.js` bleibt unverändert und bekommt kein Requirement. Der Export nutzt die Funktion nicht, weil ihr Text keine Formatversion hat.
- Kein Ersetzen der Liste, kein Zusammenführen von Dubletten, keine Zusammenarbeit mehrerer Personen.

## Decisions

- **Format nach ADR 0003.** Die Datei `aufgaben.json` ist ein JSON-Objekt `{ "format": "todo-app-export", "version": 1, "tasks": [...] }`, die Aufgaben mit denselben Feldern wie im Speicher. Alternativen und Gründe stehen im ADR.
- **Zwei Funktionen in `src/tasks.js`.** `export_text(tasks)` liefert den Text der Datei, eingerückt mit zwei Leerzeichen und mit Zeilenende am Schluss. `import_text(tasks, text)` liefert `{ tasks, hint }` wie `add_task`. So bleibt die Logik ohne Seite (ADR 0002).
- **Feste Beispieldatei als Prüfstein.** `test/fixtures/export-v1.json` ist eine Datei der Formatversion 1. Der Test zum Export vergleicht `export_text` Zeichen für Zeichen mit ihr, der Test „Datei der Formatversion 1“ importiert sie. Die Datei wird nie geändert. Eine neue Formatversion bekommt eine eigene Beispieldatei, die alten bleiben.
- **Dublette heißt gleicher Titel.** Verglichen wird der Titel genau so, wie er gespeichert ist (beim Anlegen schon ohne Leerraum am Rand). Priorität und Datum zählen nicht.
- **Prüfung der Datei.** Keine Exportdatei ist ein Text, der kein JSON ist, oder ein JSON ohne `format` gleich `"todo-app-export"`, etwa der Text von `backup_text`. Eine `version` größer als 1 meldet „neuere Version“. Die Aufgaben übernimmt der Import mit ihren Feldern aus der Datei, ohne Titel, Priorität oder Datum einzeln zu prüfen (Gate 1, offen für ein eigenes Ticket).
- **Feste Reihenfolge der Felder.** `export_text` schreibt je Aufgabe `title`, `priority` und, falls vorhanden, `due`, unabhängig von der Reihenfolge im Speicher. `load` liefert `priority` vor `title`, ohne feste Reihenfolge wiche der Text nach dem Neuladen von der Beispieldatei ab.
- **Hinweis ohne Beugung.** „Importiert: 2, als Dublette übersprungen: 0“ passt für jede Zahl, ohne Einzahl und Mehrzahl zu unterscheiden.
- **Reihenfolge der Tests nach Rot vor Grün.** Erst „Datei der Formatversion 1“ in eine leere Liste, dann „Import hängt an die Liste an“. Umgekehrt wäre der zweite Test sofort grün. Danach Dubletten, fremde Datei, Liste ohne Kennung, neuere Version, jeder rot, solange sein Code fehlt.
- **Seite.** Ein Knopf „Exportieren“ lädt `aufgaben.json` über einen Link mit `Blob` herunter. Ein Dateifeld „Importieren“ liest die Datei mit `file.text()` und zeigt `hint` im vorhandenen Hinweisfeld.

## Risks / Trade-offs

- [Das Speicherformat ändert sich später, etwa durch ein neues Feld] → Neue Formatversion mit eigener Beispieldatei. `import_text` liest jede bekannte Version. Was geschieht, regelt ein neues ADR, das ADR 0003 ablöst.
- [Der Text der Beispieldatei hängt an der Einrückung von `JSON.stringify`] → Gewollt: Jede Änderung am Text des Exports macht den Test rot und zwingt zur Entscheidung.
- [Eine Datei mit vielen Dubletten meldet nur Zahlen] → Für eine Person reicht das. Welche Titel übersprungen wurden, zeigt die Liste.
- [Die Verdrahtung in `app.js` bleibt ohne Test (ADR 0002)] → Im Browser prüfen: exportieren, Liste leeren, importieren, erneut importieren.

## Open Questions

- Soll `src/backup.js` entfallen, wenn es den Export gibt? Nicht Teil von TODO-1, eigenes Ticket.
- Aufgaben ohne Titel, mit unbekannter Priorität oder ungültigem Datum in einer Exportdatei: ablehnen oder ergänzen? Nach Gate 1 eigenes Ticket.
