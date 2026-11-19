## ADDED Requirements

### Requirement: Aufgabe bearbeiten
Die App SHALL den Titel einer Aufgabe ändern. Einen leeren Titel SHALL die App ablehnen und den bisherigen Titel behalten.

#### Scenario: Titel ändern
- **GIVEN** „Milch kaufen“ steht in der Liste
- **WHEN** jemand den Titel in „Hafermilch kaufen“ ändert
- **THEN** steht „Hafermilch kaufen“ statt „Milch kaufen“ in der Liste

#### Scenario: Leerer Titel beim Bearbeiten
- **GIVEN** „Milch kaufen“ steht in der Liste
- **WHEN** jemand den Titel leert und bestätigt
- **THEN** steht weiter „Milch kaufen“ in der Liste
- **AND** zeigt die App den Hinweis „Bitte einen Titel eingeben“

### Requirement: Aufgabe löschen
Die App SHALL eine Aufgabe nach einer Rückfrage aus der Liste entfernen.

#### Scenario: Aufgabe löschen
- **GIVEN** „Milch kaufen“ und „Brot backen“ stehen in der Liste
- **WHEN** jemand „Milch kaufen“ löscht und die Rückfrage bestätigt
- **THEN** steht nur noch „Brot backen“ in der Liste

#### Scenario: Löschen abbrechen
- **GIVEN** „Milch kaufen“ steht in der Liste
- **WHEN** jemand „Milch kaufen“ löscht und die Rückfrage verneint
- **THEN** steht „Milch kaufen“ weiter in der Liste
