## MODIFIED Requirements

### Requirement: Aufgabe löschen
Die App SHALL eine Aufgabe nach einer Rückfrage aus der Liste entfernen. Eine gelöschte Aufgabe SHALL auch nach dem Neuladen der Seite fehlen.

#### Scenario: Aufgabe löschen
- **GIVEN** „Milch kaufen“ und „Brot backen“ stehen in der Liste
- **WHEN** jemand „Milch kaufen“ löscht und die Rückfrage bestätigt
- **THEN** steht nur noch „Brot backen“ in der Liste
- **AND** nach dem Neuladen der Seite steht weiter nur „Brot backen“ in der Liste

#### Scenario: Löschen abbrechen
- **GIVEN** „Milch kaufen“ steht in der Liste
- **WHEN** jemand „Milch kaufen“ löscht und die Rückfrage verneint
- **THEN** steht „Milch kaufen“ weiter in der Liste
