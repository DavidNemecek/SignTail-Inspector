# SignTail Inspector

SignTail Inspector ist eine vollständig lokale HTML-Anwendung zur forensischen Analyse digital signierter PDF-Dokumente. Die Seite rendert den signierten Ursprung sowie alle nach der Signatur eingefügten Objekte nebeneinander und hebt Abweichungen visuell hervor – ganz ohne Server oder Installation.

## Hauptfunktionen
- **Offline-Analyse:** Alle Operationen laufen im Browser. Weder PDF-Dateien noch Zertifikate verlassen das lokale System.
- **Signaturprüfung:** Die Anwendung liest `/ByteRange` und `/Contents`, rekonstruiert die signierte Datenmenge und verifiziert PKCS#7/CMS-Signaturen mit der WebCrypto-API.
- **Objekt-Inspektion:** Inkrementelle `obj … endobj`-Blöcke hinter dem signierten Bereich werden extrahiert, typisiert (z. B. Annotationen, Formulare, XObjects) und mitsamt Rohdaten aufgelistet.
- **Visuelle Overlays:** Erkannte Rechtecke oder QuadPoints werden als Overlay über die gerenderte Seite gelegt. Checkboxen erlauben, jedes Objekt oder alle Objekte gleichzeitig ein- bzw. auszublenden.
- **Warnhinweise:** Sobald nach der Signatur neue Objekte erkannt werden, blendet die Anwendung einen roten Hinweisbalken zwischen Kopfzeile und Upload-Bereich ein.

## Nutzung
1. Öffnen Sie `index.html` direkt im Browser (Doppelklick reicht).
2. Ziehen Sie ein signiertes PDF auf den Upload-Bereich oder wählen Sie es über den Dateidialog.
3. Warten Sie, bis beide Dokumentansichten geladen sind. Die Ansicht „Final document“ zeigt den aktuellen Stand, „Signed snapshot“ den signierten Ursprung.
4. Aktivieren oder deaktivieren Sie Objekte über die Checkboxen. Die Buttons „Alle anzeigen“ und „Alle ausblenden“ steuern sämtliche Overlays gemeinsam.
5. Prüfen Sie den Signaturstatus und die Zertifikatsdetails in der rechten Seitenleiste.

## Analyse im Detail
### Signatur-Validierung
- `Uint8Array`-Parsing des PDFs zur Ermittlung von ByteRange-Segmenten.
- Rekodierung der `signedAttributes` und kryptografische Prüfung des Message Digest.
- RSA- und ECDSA-Signaturen werden mittels WebCrypto verifiziert.

### Inkrementelle Objekte
- Parsing neuer Objekte nach dem Ende des ByteRange.
- Klassifikation anhand typischer Schlüssel wie `/Subtype`, `/OCG`, `/AcroForm`, `/Annots` usw.
- Extraktion von Referenzen, Koordinaten (`/Rect`, `/BBox`, `QuadPoints`) sowie Seitenbezügen.

### Visualisierung
- Einbettung von pdf.js rendert signierten und finalen Stand parallel.
- Für jede Seite wird ein Overlay-Layer erzeugt, in den die Rechtecke der erkannten Objekte projiziert werden.
- Die Liste synchronisiert sich mit der aktiven Sprache (Deutsch/Englisch) und aktualisiert Beschriftungen dynamisch.

## Demos
Beispiel-PDFs können im Ordner `demos/` abgelegt werden. Da Browser-HTML keinen direkten Ordnerzugriff besitzt, müssen Demo-Dateien manuell über den Dateiauswahldialog geladen werden.

## Entwicklungshinweise
- pdf.js ist eingebettet und wird aus einer Base64-kodierten Distribution geladen, sodass keine externen Abhängigkeiten erforderlich sind.
- Der gesamte Code liegt in `index.html`. Anpassungen an UI oder Logik sollten dort vorgenommen werden.
- Für Übersetzungen steht ein einfaches Dictionary zur Verfügung (`translations`-Objekt).

## Einschränkungen
- Keine automatische PKI-Validierung (OCSP, CRL, Vertrauenskette).
- Komprimierte oder verschlüsselte Streams werden nicht dekomprimiert, sondern nur textuell markiert.
- Koordinatenbasierte Overlays hängen von den PDF-Metadaten ab und können bei exotischen Dokumenten fehlen oder ungenau sein.
- Die Analyse ersetzt keine rechtsverbindliche Signaturprüfung.

## Sicherheit
SignTail Inspector kommuniziert niemals mit externen Diensten. Alle Daten verbleiben im lokalen Browser-Tab.
