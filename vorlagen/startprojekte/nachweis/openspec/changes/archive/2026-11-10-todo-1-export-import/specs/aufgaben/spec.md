## ADDED Requirements

### Requirement: Aufgaben exportieren
Die App SHALL alle Aufgaben mit Titel, Priorität und Fälligkeitsdatum in der Reihenfolge des Anlegens in eine Datei exportieren, unabhängig von Sortierung und Anzeige. Die Datei SHALL ihre Formatversion nennen, derzeit 1.

#### Scenario: Export mit allen Angaben
- **GIVEN** „Milch kaufen“ hat die Priorität hoch und ist am 20.11.2026 fällig
- **AND** „Brot backen“ hat die Priorität niedrig und kein Fälligkeitsdatum
- **WHEN** jemand die Aufgaben exportiert
- **THEN** nennt die Datei die Formatversion 1
- **AND** enthält „Milch kaufen“ mit der Priorität hoch und dem Fälligkeitsdatum 20.11.2026, danach „Brot backen“ mit der Priorität niedrig ohne Fälligkeitsdatum

### Requirement: Aufgaben importieren
Die App SHALL die Aufgaben einer Exportdatei in der Reihenfolge der Datei ans Ende der Liste anhängen. Eine Aufgabe, deren Titel schon in der Liste steht, SHALL die App nicht noch einmal anlegen, die Aufgabe in der Liste bleibt unverändert. Nach dem Import SHALL die App zeigen, wie viele Aufgaben sie angelegt und wie viele sie als Dublette übersprungen hat. Eine Datei, die keine Exportdatei der App ist, SHALL die App ablehnen und die Liste unverändert lassen.

#### Scenario: Import hängt an die Liste an
- **GIVEN** „Post holen“ steht in der Liste
- **WHEN** jemand eine Exportdatei mit „Milch kaufen“ importiert
- **THEN** zeigt die Liste „Post holen“, „Milch kaufen“ in dieser Reihenfolge
- **AND** zeigt die App den Hinweis „Importiert: 1, als Dublette übersprungen: 0“

#### Scenario: Dublette beim Import
- **GIVEN** „Milch kaufen“ mit der Priorität mittel steht in der Liste
- **WHEN** jemand eine Exportdatei mit „Milch kaufen“ in der Priorität hoch und „Brot backen“ in der Priorität niedrig importiert
- **THEN** zeigt die Liste „Milch kaufen“ mit der Priorität mittel, danach „Brot backen“ mit der Priorität niedrig
- **AND** zeigt die App den Hinweis „Importiert: 1, als Dublette übersprungen: 1“

#### Scenario: Datei ist keine Exportdatei
- **GIVEN** „Milch kaufen“ steht in der Liste
- **WHEN** jemand eine Textdatei mit dem Inhalt „Milch kaufen, Brot backen“ importiert
- **THEN** steht nur „Milch kaufen“ in der Liste
- **AND** zeigt die App den Hinweis „Die Datei ist keine Exportdatei der ToDo-App“

#### Scenario: Aufgabenliste ohne Kennung der App
- **GIVEN** „Milch kaufen“ steht in der Liste
- **WHEN** jemand eine Datei importiert, die eine Aufgabenliste enthält, aber nicht als Exportdatei der ToDo-App gekennzeichnet ist
- **THEN** steht nur „Milch kaufen“ in der Liste
- **AND** zeigt die App den Hinweis „Die Datei ist keine Exportdatei der ToDo-App“

### Requirement: Exportdateien bleiben importierbar
Die App SHALL Exportdateien jeder Formatversion importieren, die sie kennt, auch nachdem eine neuere Formatversion eingeführt wurde. Eine Datei mit einer höheren Formatversion, als die App kennt, SHALL die App ablehnen und die Liste unverändert lassen.

#### Scenario: Datei der Formatversion 1
- **GIVEN** die Liste ist leer
- **AND** eine Datei der Formatversion 1 enthält „Milch kaufen“ mit der Priorität hoch und dem Fälligkeitsdatum 20.11.2026, danach „Brot backen“ mit der Priorität niedrig
- **WHEN** jemand diese Datei importiert
- **THEN** zeigt die Liste „Milch kaufen“ mit der Priorität hoch und dem Fälligkeitsdatum 20.11.2026, danach „Brot backen“ mit der Priorität niedrig

#### Scenario: Datei aus einer neueren Version
- **GIVEN** „Milch kaufen“ steht in der Liste
- **WHEN** jemand eine Exportdatei der Formatversion 2 importiert
- **THEN** steht nur „Milch kaufen“ in der Liste
- **AND** zeigt die App den Hinweis „Die Datei stammt aus einer neueren Version der ToDo-App“
