# ADR Review Manifest

## ADR Review Completed

- Date: 2026-11-10
- Reviewer: Agent beim propose, Prüfung durch den Menschen an Gate 1
- Change: todo-1-export-import (Ticket TODO-1)

## In-Force ADR Context Reviewed

- adr/0001-tasks-in-local-storage.md - ADR 0001 gelesen, bleibt in Kraft. Die Aufgaben bleiben im Local Storage, die Exportdatei übernimmt dessen Felder. ADR 0001 nennt ein zweites Gerät als Grund für eine Neuentscheidung. Der Export bringt Aufgaben von Hand auf ein anderes Gerät, beide Geräte arbeiten weiter mit eigener Liste ohne Abgleich.
- adr/0002-logic-without-dom.md - ADR 0002 gelesen, bleibt in Kraft. `export_text` und `import_text` stehen in `src/tasks.js` ohne Zugriff auf die Seite, `app.js` liest und schreibt nur die Datei.

## Repository-Level ADRs Created

- adr/0003-export-file-versioned-json.md - neues ADR 0003: Exportdatei als JSON mit Formatkennung und Versionsnummer. Auslöser: Eine öffentliche Schnittstelle entsteht (die Datei verlässt die App) und schwer umkehrbar (Datenmodell der Datei).

## Notes

- Dieser Vermerk hält nur die Prüfung fest. Kontext, Optionen und Begründung stehen in ADR 0003.
- Kein Auslöser für Dubletten, Reihenfolge beim Import und Hinweistexte. Sie stehen im Delta und in design.md, nicht in einem ADR.
- Kein ADR wird abgelöst, ADR 0001 und 0002 bleiben unverändert.
