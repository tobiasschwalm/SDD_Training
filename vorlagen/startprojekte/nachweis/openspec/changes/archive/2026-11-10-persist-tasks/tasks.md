## 1. Tests aus den Szenarien

- [x] 1.1 Test „Zustand nach dem Neuladen“ in `test/storage.test.js`: nach `save` mit „Milch kaufen“ liefert `load` aus derselben Attrappe „Milch kaufen“
- [x] 1.2 Test „Erster Start ohne Aufgaben“: `load` aus einer leeren Attrappe liefert eine leere Liste

## 2. Umsetzung

- [x] 2.1 `load` und `save` in `src/tasks.js` anlegen, bis `node --test` alle fünf Tests grün zeigt
- [x] 2.2 `src/app.js` lädt beim Start und speichert nach jeder Änderung. Im Browser eine Aufgabe anlegen, neu laden und nachsehen, dass sie in der Liste steht
- [x] 2.3 Confirmation aus ADR 0001 ausführen: `grep -rnE "fetch\(|XMLHttpRequest|indexedDB" --include="*.js" --include="*.html" .` findet nichts
