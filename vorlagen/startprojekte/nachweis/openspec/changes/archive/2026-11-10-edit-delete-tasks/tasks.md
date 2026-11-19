## 1. Tests aus den Szenarien

- [x] 1.1 Test „Titel ändern“ in `test/tasks.test.js`: nach `edit_task` steht „Hafermilch kaufen“ statt „Milch kaufen“ in der Liste
- [x] 1.2 Test „Leerer Titel beim Bearbeiten“: `edit_task` mit leerem Titel behält „Milch kaufen“ und liefert den Hinweis „Bitte einen Titel eingeben“
- [x] 1.3 Test „Aufgabe löschen“: nach `delete_task` mit Bestätigung steht nur noch „Brot backen“ in der Liste
- [x] 1.4 Test „Löschen abbrechen“: nach `delete_task` ohne Bestätigung steht „Milch kaufen“ weiter in der Liste

## 2. Umsetzung

- [x] 2.1 `edit_task` und `delete_task` in `src/tasks.js` anlegen, bis `node --test` alle neun Tests grün zeigt
- [x] 2.2 Knöpfe „Bearbeiten“ und „Löschen“ je Aufgabe in `src/app.js`. Im Browser eine Aufgabe umbenennen, eine löschen, die Rückfrage einmal verneinen und nach dem Neuladen nachsehen, dass die Liste stimmt
