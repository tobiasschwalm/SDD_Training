## 1. Tests aus den Szenarien

- [x] 1.1 Test „Aufgabe mit Titel“ in `test/tasks.test.js`: nach `add_task([], "Milch kaufen")` steht „Milch kaufen“ in der Liste
- [x] 1.2 Test „Leerer Titel“: `add_task([], "")` liefert eine leere Liste und den Hinweis „Bitte einen Titel eingeben“
- [x] 1.3 Test „Leerraum um den Titel“: nach `add_task([], "  Milch kaufen  ")` steht „Milch kaufen“ in der Liste

## 2. Umsetzung

- [x] 2.1 `add_task` in `src/tasks.js` anlegen, bis `node --test` alle drei Tests grün zeigt
- [x] 2.2 `index.html` und `src/app.js` anlegen und im Browser nachsehen, dass eine neue Aufgabe in der Liste erscheint
