## MODIFIED Requirements

### Requirement: Aufgabe anlegen
Die App SHALL eine Aufgabe mit einem Titel und einer Priorität hoch, mittel oder niedrig anlegen und sie in der Liste zeigen. Ohne Angabe SHALL die Priorität mittel gelten, auch für gespeicherte Aufgaben ohne Priorität. Einen leeren Titel SHALL die App ablehnen.

#### Scenario: Aufgabe mit Titel
- **WHEN** jemand den Titel „Milch kaufen“ eingibt und bestätigt
- **THEN** steht „Milch kaufen“ in der Liste

#### Scenario: Leerer Titel
- **WHEN** jemand ein leeres Eingabefeld bestätigt
- **THEN** legt die App keine Aufgabe an
- **AND** zeigt den Hinweis „Bitte einen Titel eingeben“

#### Scenario: Leerraum um den Titel
- **WHEN** jemand den Titel „  Milch kaufen  “ eingibt und bestätigt
- **THEN** steht „Milch kaufen“ in der Liste

#### Scenario: Aufgabe mit Priorität
- **WHEN** jemand den Titel „Milch kaufen“ mit der Priorität hoch anlegt
- **THEN** steht „Milch kaufen“ mit der Priorität hoch in der Liste

#### Scenario: Ohne Angabe der Priorität
- **WHEN** jemand den Titel „Milch kaufen“ ohne Priorität anlegt
- **THEN** steht „Milch kaufen“ mit der Priorität mittel in der Liste

#### Scenario: Gespeicherte Aufgabe ohne Priorität gilt als mittel
- **GIVEN** „Brot backen“ wurde vor Einführung der Priorität gespeichert
- **WHEN** jemand die Seite neu lädt
- **THEN** steht „Brot backen“ mit der Priorität mittel in der Liste

## ADDED Requirements

### Requirement: Aufgaben sortieren
Die App SHALL die Liste auf Wunsch nach Priorität sortieren, hoch vor mittel vor niedrig.

#### Scenario: Nach Priorität sortieren
- **GIVEN** „Brot backen“ hat die Priorität niedrig, „Milch kaufen“ hoch und „Post holen“ mittel
- **WHEN** jemand die Liste nach Priorität sortiert
- **THEN** zeigt die Liste „Milch kaufen“, „Post holen“, „Brot backen“ in dieser Reihenfolge
