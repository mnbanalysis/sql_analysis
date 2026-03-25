# MNB Székház Költségvetés Elemző

Ez a repository a felújítási projekt (eredetileg egy nagy és nehezen feldolgozható, kép-alapú PDF) költségvetési adatainak digitalizált, feldolgozott és elemezhető formátumait tartalmazza. A projekt célja, hogy átláthatóvá és kereshetővé tegye a több ezer tételből álló költségvetést.

## 🗂️ A repository felépítése

*   **`sql_analyzer.html`**: A projekt egyik legfontosabb eszköze. Egy böngészőből futtatható, kliens oldali SQL elemző és vizualizációs eszköz (AlaSQL és Chart.js alapon). Lehetővé teszi a több mint 7800 tisztított tétel azonnali, adatbázisszerű lekérdezését, gyanús tételek (pl. extrém túlárazások, indokolatlan munkadíjak) szűrését és vizuális megjelenítését. Tele van előre megírt, gyorsan futtatható okoslekérdezésekkel.
*   **`database.js`** & **`database.json`**: A teljes, digitalizált és megtisztított költségvetés aggregált adatbázisa. Tartalmazza a főkategóriákat, alkategóriákat és a legalsó szintű, konkrét árazott tételeket.
*   **`html_report/`**: A PDF feldolgozása során oldalanként generált strukturált HTML fájlok. Ezek az oldalak már kényelmesen böngészhető, formázott táblázatos formában tartalmazzák az eredeti képi dokumentumból kinyert adatokat. Minden fájl (pl. `oldal_210_aljzatbetonok.html`) egy adott PDF oldalt reprezentál, beágyazva az adott oldalhoz tartozó fejléc-képet is a könnyebb vizuális ellenőrzés érdekében.
*   **`csv/`**: Az OCR és vizuális adatkinyerés (Google Gemini Vision) során legelső lépésként, nyers formátumban elmentett táblázatos adatok. Itt találhatók a gépi feldolgozásra szánt darabolt adatok oldalanként (pl. `page_210.csv`), mielőtt azokat egy nagy, összefüggő SQL adatbázissá (`database.js`) tisztítottuk és összesítettük volna.
*   **`images/`**: A dokumentumból automatikusan kivágott, az elemzésekhez és ellenőrzésekhez használt eredeti képrészletek. Mivel az eredeti PDF scannelt volt (szövegként nem másolható), ez a mappa tartalmazza a kinyert fejléc-blokkokat és vizuális referenciákat (`page_..._header.png`) minden egyes feldolgozott oldalhoz, bizonyítékként a kinyert adatok hitelességére.
*   **`analysis_report/` ⚠️ (Kísérleti állapot / Experimental)**: LLM alapú generált szöveges elemzéseket és ár-érték értékeléseket tartalmazó szekció. Jelenleg fejlesztés és validálás alatt áll, az itt található megállapítások automatikusan generáltak.

## 🚀 Használat (SQL Elemző)

Mivel a teljes adatbázis be van ágyazva, semmilyen szerveroldali technológiára (PHP, Python, MySQL) nincs szükség a használatához!

1.  Töltsd le a repository-t.
2.  Nyisd meg a `sql_analyzer.html` fájlt bármilyen modern böngészőben (Chrome, Edge, Firefox, Safari).
3.  A jobb oldali panelen kattints a **Gyors Lekérdezések** gombjaira, vagy írj saját SQL (SQLite szintaxisú) lekérdezéseket a bal oldali szövegdobozba.
4.  Ha a lekérdezésed pontosan két oszlopot ad vissza (pl. "Kategória Neve" és "Összérték"), a rendszer automatikusan grafikont generál a táblázat mellé.

## 🛠️ Technológia

A digitalizáció egy egyedi, lépésről-lépésre haladó Python + Google Gemini Vision API megoldással készült, hogy megkerüljük az ingyenes token limiteket és a gigantikus (31 MB-os, szöveges réteget nem tartalmazó) PDF méretből fakadó problémákat. Az adatok konszolidációja a Python Pandas könyvtárának segítségével történt.