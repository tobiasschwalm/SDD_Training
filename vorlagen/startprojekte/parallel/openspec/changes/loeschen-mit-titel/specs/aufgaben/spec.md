## MODIFIED Requirements

### Requirement: Aufgabe löschen
Die App SHALL eine Aufgabe nach einer Rückfrage, die den Titel der Aufgabe nennt, aus der Liste entfernen.

#### Scenario: Aufgabe löschen
- **GIVEN** „Milch kaufen“ und „Brot backen“ stehen in der Liste
- **WHEN** jemand „Milch kaufen“ löscht und die Rückfrage „„Milch kaufen“ löschen?“ bestätigt
- **THEN** steht nur noch „Brot backen“ in der Liste

#### Scenario: Löschen abbrechen
- **GIVEN** „Milch kaufen“ steht in der Liste
- **WHEN** jemand „Milch kaufen“ löscht und die Rückfrage „„Milch kaufen“ löschen?“ verneint
- **THEN** steht „Milch kaufen“ weiter in der Liste
