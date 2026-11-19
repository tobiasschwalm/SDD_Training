## Why

Aufgaben lassen sich heute weder sichern noch auf ein anderes Gerät bringen (Ticket TODO-1). Sie liegen nur im Speicher eines Browsers, und das Löschen der Browserdaten löscht sie mit (ADR 0001).

## What Changes

- Alle Aufgaben lassen sich mit einem Klick in eine Datei exportieren, mit Titel, Priorität und Fälligkeitsdatum.
- Eine Exportdatei lässt sich importieren. Die Aufgaben der Datei kommen ans Ende der Liste.
- Dubletten (offen im Ticket): Eine Aufgabe, deren Titel schon in der Liste steht, legt der Import nicht noch einmal an. Die vorhandene Aufgabe bleibt unverändert. Die App meldet, wie viele Aufgaben angelegt und wie viele übersprungen wurden.
- Dateiformat (offen im Ticket): Jede Exportdatei nennt ihre Formatversion. Dateien jeder bekannten Formatversion bleiben importierbar, eine Datei mit unbekannter höherer Version lehnt die App ab. Das Format selbst legt ADR 0003 fest.
- Bewusst nicht im Change: Ersetzen der Liste beim Import, Zusammenführen von Dubletten, Import der Sicherung aus `src/backup.js`.

## Capabilities

### New Capabilities

- keine

### Modified Capabilities

- `aufgaben`: neue Requirements „Aufgaben exportieren“, „Aufgaben importieren“ und „Exportdateien bleiben importierbar“. Bestehende Requirements bleiben unverändert.

## Impact

`index.html`, `src/tasks.js`, `src/app.js`, neue Testdatei `test/exchange.test.js` mit einer festen Beispieldatei `test/fixtures/export-v1.json`. Das Format der Exportdatei ist eine Schnittstelle nach außen: Dateien von heute müssen spätere Versionen lesen (ADR 0003).

Bestand ohne Spec: `src/backup.js` liefert die Liste als JSON-Text ohne Formatversion. Die Seite nutzt die Funktion nicht, ein Requirement gibt es nicht. Der Change beschreibt sie nicht nachträglich und ändert sie nicht. Ob sie entfällt, entscheidet ein eigenes Ticket.


## Gate 1

Freigegeben am 2026-11-10 mit Auflagen, die vor der Freigabe eingearbeitet wurden. Ein Prüf-Agent in neuer Sitzung hat Change, Ticket, Hauptspec und ADRs gelesen und zehn Funde gemeldet.

- Eingearbeitet: Export in der Reihenfolge des Anlegens. Hinweis auch beim Import ohne Dublette. Neues Szenario „Aufgabenliste ohne Kennung der App“, weil Task 3.8 mehr umsetzte, als sein Test verlangte. In `adr.md` begründet, warum ADR 0001 trotz zweitem Gerät gilt.
- Bestätigt als Festlegung des Teams, nicht aus dem Ticket: Dublette heißt gleicher Titel, vorhandene Aufgabe bleibt. Neuere Formatversion wird abgelehnt. Hinweis mit Zahlen.
- Bewusst offen, eigenes Ticket: Aufgaben ohne Titel, unbekannte Priorität oder ungültiges Datum in einer Exportdatei. Die Zusage für spätere Versionen prüft die Beispieldatei aus ADR 0003, der „eine Klick“ wird im Browser geprüft.
- ADR 0003 steht mit dieser Freigabe auf accepted. Vor Gate 1 hätte proposed genügt.
