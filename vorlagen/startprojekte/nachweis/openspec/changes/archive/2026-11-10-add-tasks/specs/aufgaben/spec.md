## Purpose

Legt fest, wie eine Person ihre Aufgaben anlegt, ändert und in einer Liste sieht.

## ADDED Requirements

### Requirement: Aufgabe anlegen
Die App SHALL eine Aufgabe mit einem Titel anlegen und sie in der Liste zeigen. Einen leeren Titel SHALL die App ablehnen.

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
