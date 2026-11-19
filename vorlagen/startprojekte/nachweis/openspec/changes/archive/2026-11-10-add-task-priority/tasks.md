## 1. Tests aus den Szenarien

- [x] 1.1 Tests „Aufgabe mit Titel“, „Leerer Titel“ und „Leerraum um den Titel“ bleiben unverändert grün
- [x] 1.2 Test „Aufgabe mit Priorität“ in `test/tasks.test.js`: `add_task([], "Milch kaufen", "hoch")` liefert „Milch kaufen“ mit der Priorität hoch
- [x] 1.3 Test „Ohne Angabe der Priorität“: `add_task([], "Milch kaufen")` liefert „Milch kaufen“ mit der Priorität mittel
- [x] 1.4 Test „Gespeicherte Aufgabe ohne Priorität gilt als mittel“ in `test/storage.test.js`: `load` liefert für ein gespeichertes `{ title: "Brot backen" }` die Priorität mittel
- [x] 1.5 Test „Nach Priorität sortieren“: `by_priority` liefert „Milch kaufen“, „Post holen“, „Brot backen“ in dieser Reihenfolge

## 2. Umsetzung

- [x] 2.1 Priorität in `add_task` und `load`, `by_priority` in `src/tasks.js`, bis `node --test` alle dreizehn Tests grün zeigt
- [x] 2.2 Auswahl der Priorität im Formular und Kästchen „Nach Priorität sortieren“ in `index.html` und `src/app.js`. Im Browser drei Aufgaben mit verschiedenen Prioritäten anlegen, sortieren und in der sortierten Ansicht eine bearbeiten
