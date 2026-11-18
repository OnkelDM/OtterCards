# OtterCards (Web-App)

Kundenkarten digital im Handy. Läuft komplett im Browser, alle Daten bleiben lokal auf dem Gerät
(kein Server, kein Konto).

## Dateien

| Datei | Zweck |
|---|---|
| `index.html` | die ganze App (HTML, CSS, JavaScript in einer Datei) |
| `manifest.json` | Name, Farben und Icons für den Home-Bildschirm |
| `sw.js` | Service Worker: macht die App offline nutzbar |
| `icon-*.png`, `apple-touch-icon.png` | App-Icons |
| `vendor/` | Code-Erkennung (ZXing als WebAssembly, MIT-Lizenz), Lizenztexte in `vendor/LICENSES.txt` |

Lokal ausprobieren: `index.html` einfach im Browser öffnen (Offline-Modus und Installation brauchen HTTPS, siehe unten).

## Veröffentlichen mit GitHub Pages (kostenlos)

1. Auf github.com ein neues Repository anlegen (z. B. `ottercards`).
2. Alle Dateien aus diesem Ordner hochladen (**inklusive Ordner `vendor`**, aber ohne `node_modules` und `package.json`) („Add file → Upload files“).
3. Im Repository: **Settings → Pages → Source: „Deploy from a branch“ → Branch `main`, Ordner `/ (root)` → Save**.
4. Nach ca. einer Minute ist die App unter `https://<dein-name>.github.io/ottercards/` erreichbar.
5. Diesen Link an die gewünschten Personen schicken.

Alternativen: Cloudflare Pages oder Netlify (Ordner per Drag & Drop hochladen).

## Auf den Home-Bildschirm (iPhone)

1. Link in **Safari** öffnen.
2. Teilen-Symbol → **„Zum Home-Bildschirm“** → Hinzufügen.
3. OtterCards über das Icon starten (nicht über Safari). Nur so bleiben die Daten dauerhaft gespeichert.

Android/Chrome: Menü → „App installieren“.

## Backup (wichtig)

Die Karten liegen nur auf dem jeweiligen Gerät. Sie gehen verloren, wenn die App vom Home-Bildschirm
gelöscht oder „Website-Daten“ gelöscht werden oder das Handy gewechselt wird.

- **Menü (⋯) → Backup erstellen:** speichert alle Karten inkl. Fotos als `ottercards-backup-….json`
  (auf dem iPhone z. B. in „Dateien“/iCloud Drive sichern).
- **Menü (⋯) → Backup einspielen:** „Zusammenführen“ ergänzt, „Ersetzen“ ersetzt alle Karten.
- Nach 30 Tagen ohne Backup erinnert die App.

## Updates ausliefern

1. Dateien ändern und hochladen.
2. In `sw.js` die Versionsnummer in `CACHE = 'ottercards-v1.1.0'` erhöhen (z. B. `v1.1.1`).
3. Die Geräte holen sich die neue Version beim nächsten Start (ggf. App einmal schließen und neu öffnen).

## Was geht, was nicht

- Barcodes/QR-Codes werden ohne Fremdbibliothek erzeugt: QR, Code 128, EAN-13, EAN-8, UPC-A, Code 39, ITF.
- Automatische Code-Erkennung (Foto und Live-Scanner) läuft komplett im Browser mit ZXing (Ordner `vendor/`),
  auch auf dem iPhone und offline. Beim ersten Gebrauch wird die Erkennung geladen (ca. 1 MB, danach zwischengespeichert).
  Erkannt werden QR, Code 128, EAN-13, EAN-8, UPC-A, Code 39 und ITF. Andere Typen (z. B. UPC-E, Data Matrix, PDF417)
  werden gelesen, die Nummer wird übernommen und als Text angezeigt.
  Für gute Treffer: Karte gerade, scharf, ohne Spiegelung und der Code sollte mindestens ein Viertel der Bildbreite füllen.
- Kein automatisches Zuschneiden der Karte, keine automatische Helligkeit (Webseiten dürfen das nicht).
- „Logo von Webseite laden“ fragt einen Favicon-Dienst ab (Google, ersatzweise DuckDuckGo).
  Das ist optional und der einzige Moment, in dem die App etwas ins Internet sendet.


## Zahl im Foto lesen (ab Version 1.2.0)

Findet die App im Foto keinen Barcode/QR-Code, liest sie die aufgedruckte Nummer per Texterkennung (läuft komplett auf dem Gerät, Ordner `vendor/ocr/`, zusammen ca. 16 MB).
Die gefundenen Zahlen erscheinen als antippbare Vorschläge unter „Nummer“ – bitte immer mit der Karte vergleichen. Auch der Knopf „Zahl im Foto suchen“ startet die Erkennung.
Beim ersten Mal wird der Ordner geladen (Internet nötig), danach steht er offline bereit. **Beim Hochladen auf GitHub den ganzen Ordner `vendor` samt `vendor/ocr` mitnehmen.**
Nach Änderungen: `CACHE` in `sw.js` hochzählen, damit sich die Apps aktualisieren.
