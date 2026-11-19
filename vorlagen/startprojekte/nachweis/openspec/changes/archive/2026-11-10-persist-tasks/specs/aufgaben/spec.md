## ADDED Requirements

### Requirement: Aufgaben bleiben erhalten
Die App SHALL alle Aufgaben nach dem Neuladen der Seite wieder zeigen.

#### Scenario: Zustand nach dem Neuladen
- **GIVEN** „Milch kaufen“ steht in der Liste
- **WHEN** jemand die Seite neu lädt
- **THEN** steht „Milch kaufen“ weiter in der Liste

#### Scenario: Erster Start ohne Aufgaben
- **GIVEN** es wurde noch keine Aufgabe angelegt
- **WHEN** jemand die Seite öffnet
- **THEN** ist die Liste leer
