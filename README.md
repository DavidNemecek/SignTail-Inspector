# SignTail Inspector

SignTail Inspector ist eine vollständig lokale HTML-Anwendung, die signierte PDFs analysiert. Die Seite rendert das finale Dokument neben dem signierten Stand, markiert inkrementelle Objekte und hebt nachträgliche Inhalte als Overlays hervor. Eine sichtbare Warnleiste signalisiert sofort, wenn nicht signierte Objekte oder ein fehlender Signaturbereich erkannt werden.

## Hauptfunktionen
- **Offline-Analyse:** Alle Operationen laufen direkt im Browser. Dateien werden nicht hochgeladen oder weitergegeben.
- **Signaturbewertung:** `/ByteRange` und `/Contents` werden geparst, die Signatur mittels WebCrypto geprüft und Zertifikatsinformationen dargestellt.
- **Vergleich gerenderter Stände:** pdf.js rendert sowohl den signierten Snapshot als auch die finale Version, sodass Unterschiede unmittelbar sichtbar werden.
- **Objektliste mit Overlays:** Nicht signierte Objekte werden automatisch hervorgehoben, standardmäßig aktiviert und lassen sich über Checkboxen oder die Schaltflächen „Alle anzeigen/Ausblenden“ steuern.
- **Mehrsprachigkeit:** Oberfläche und Meldungen stehen auf Englisch und Deutsch zur Verfügung.

## Bedienung
1. Öffne `index.html` im Browser (Doppelklick oder lokal gehostet).
2. Ziehe ein signiertes PDF auf den Upload-Bereich oder wähle es per Dateidialog.
3. Über die Warnleiste erkennst du sofort, ob unsignierte Inhalte gefunden wurden.
4. Nutze die Objektliste, um einzelne Objekte zu untersuchen. Aktivierte Objekte blenden ihre Koordinaten als Overlay in der Vorschau ein.
5. Wechsle bei Bedarf den Anzeigemodus zwischen „Enddokument“ und „Signierter Stand“.

## Hinweise zur Objektansicht
- Die Liste zeigt erkannte Metadaten wie Offset, vermutete Seite, Rechtecke und den Objekttyp (z. B. Annotation, Formularfeld, XObject).
- Rohdaten eines Objekts lassen sich einblenden, um Anpassungen direkt zu prüfen.
- Wenn keine Geometrie verfügbar ist, bleibt lediglich die textuelle Beschreibung bestehen.

## Demos & Tests
Beispiel-PDFs können im Ordner `demos/` abgelegt werden. Die Seite kann ohne Webserver genutzt werden; wähle die Datei einfach über den Upload-Dialog aus.

## Grenzen des Tools
- Es erfolgt keine PKI-Validierung (OCSP/CRL). Die Prüfung beschränkt sich auf die kryptografische Signatur.
- Stark komprimierte oder verschlüsselte Streams lassen sich nur eingeschränkt analysieren.
- Positionsdaten basieren auf Heuristiken; Overlays können in seltenen Fällen ungenau sein.
- Die Ergebnisse ersetzen keine forensische Detailprüfung oder rechtlich verbindliche Begutachtung.

## Lizenz
Dieses Projekt ist ausschließlich zu Demonstrationszwecken gedacht. Eine Lizenzdatei liegt dem Repository bei oder muss ergänzend hinzugefügt werden.
