## MODIFIED Requirements

### Requirement: Aufgabe anlegen
Die App SHALL eine Aufgabe mit einem Titel, einer Priorität hoch, mittel oder niedrig und optional einem Fälligkeitsdatum anlegen und sie in der Liste zeigen. Ohne Angabe SHALL die Priorität mittel gelten, auch für gespeicherte Aufgaben ohne Priorität. Ohne Angabe SHALL die Aufgabe kein Fälligkeitsdatum haben. Einen leeren Titel SHALL die App ablehnen.

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

#### Scenario: Aufgabe mit Fälligkeitsdatum
- **WHEN** jemand den Titel „Milch kaufen“ mit der Priorität hoch und dem Fälligkeitsdatum 20.11.2026 anlegt
- **THEN** steht „Milch kaufen“ mit der Priorität hoch und dem Fälligkeitsdatum 20.11.2026 in der Liste

#### Scenario: Leeres Datumsfeld
- **WHEN** jemand den Titel „Milch kaufen“ anlegt und das Feld für das Fälligkeitsdatum leer lässt
- **THEN** steht „Milch kaufen“ ohne Fälligkeitsdatum in der Liste

### Requirement: Aufgaben sortieren
Die App SHALL die Liste auf Wunsch nach Priorität sortieren, hoch vor mittel vor niedrig. Überfällige Aufgaben SHALL dabei oben bleiben.

#### Scenario: Nach Priorität sortieren
- **GIVEN** „Brot backen“ hat die Priorität niedrig, „Milch kaufen“ hoch und „Post holen“ mittel
- **WHEN** jemand die Liste nach Priorität sortiert
- **THEN** zeigt die Liste „Milch kaufen“, „Post holen“, „Brot backen“ in dieser Reihenfolge

#### Scenario: Überfällige Aufgabe steht beim Sortieren oben
- **GIVEN** heute ist der 10.11.2026
- **AND** „Post holen“ hat die Priorität niedrig, „Milch kaufen“ hoch, beide ohne Fälligkeitsdatum
- **AND** „Brot backen“ hat die Priorität niedrig und ist am 09.11.2026 fällig
- **WHEN** jemand die Liste nach Priorität sortiert
- **THEN** zeigt die Liste „Brot backen“, „Milch kaufen“, „Post holen“ in dieser Reihenfolge

## ADDED Requirements

### Requirement: Überfällige Aufgaben oben
Die App SHALL Aufgaben, deren Fälligkeitsdatum vor dem heutigen Tag liegt, als überfällig oben in der Liste zeigen. Eine Aufgabe, die heute fällig ist, SHALL nicht als überfällig gelten.

#### Scenario: Heute fällig ist nicht überfällig
- **GIVEN** heute ist der 10.11.2026
- **AND** „Milch kaufen“ hat kein Fälligkeitsdatum und wurde zuerst angelegt
- **AND** „Brot backen“ ist am 10.11.2026 fällig
- **WHEN** jemand die Liste ansieht
- **THEN** zeigt die Liste „Milch kaufen“, „Brot backen“ in dieser Reihenfolge

#### Scenario: Überfällige Aufgabe steht oben
- **GIVEN** heute ist der 10.11.2026
- **AND** „Milch kaufen“ hat kein Fälligkeitsdatum, „Post holen“ ist am 20.11.2026 fällig und „Brot backen“ am 09.11.2026, in dieser Reihenfolge angelegt
- **WHEN** jemand die Liste ansieht
- **THEN** zeigt die Liste „Brot backen“, „Milch kaufen“, „Post holen“ in dieser Reihenfolge
