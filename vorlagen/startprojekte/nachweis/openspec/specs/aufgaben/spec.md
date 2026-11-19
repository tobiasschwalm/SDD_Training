# aufgaben Specification

## Purpose
Legt fest, wie eine Person ihre Aufgaben anlegt, ändert und in einer Liste sieht.

## Requirements

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
