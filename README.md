# SignTail Inspector

## Beschreibung
SignTail Inspector ist eine vollständig lokale HTML-Anwendung, die signierte PDFs parallel rendert und forensisch analysiert. Eine eingebettete, offline ausgelieferte pdf.js-Engine stellt sowohl den signierten Grundzustand als auch die inkrementellen Nachträge dar. Gefundene Objekte lassen sich per Checkbox gezielt aktivieren, sodass ihre Inhalte als Overlay direkt in der Vorschau erscheinen oder ausgeblendet bleiben. Alle Schritte laufen offline im Browser – es werden keinerlei Dateien hochgeladen.

## Funktionsweise
1. Das ausgewählte PDF wird als `Uint8Array` eingelesen. Aus dem `/ByteRange` wird der signierte Stand rekonstruiert und gemeinsam mit dem finalen Dokument über die eingebettete pdf.js-Engine gerendert.
2. Der `/ByteRange` der Signatur wird geparst, um die beiden signierten Segmente exakt zu bestimmen.
3. Der zugehörige `/Contents`-Eintrag wird als PKCS#7/CMS-Signatur dekodiert. Anhand von Issuer und Seriennummer wird das passende Zertifikat gewählt, die `signedAttributes` werden DER-konform rekodiert, der `messageDigest` validiert und die Signatur mit der WebCrypto-API (RSA oder ECDSA) mathematisch verifiziert.
4. Der Bereich hinter dem Ende des ByteRange wird als inkrementelles Update interpretiert. Dort sucht das Tool nach neuen `obj ... endobj`-Blöcken, ermittelt Schlüsselwörter wie `/OCG`, `/OptionalContentGroup`, `/XObject`, `/Subtype /Image`, `stream`, `/Annots` oder `/AcroForm` und erstellt strukturierte Funde.
5. Beim Rendern werden die signierten Seiten und die finale Version pixelweise verglichen. Unterschiedliche Bildbereiche werden freigestellt, sodass die Nachträge separat als Overlay eingeblendet werden können.
6. Für jedes gefundene Objekt steht eine Checkbox zur Verfügung. Darüber können Rohdaten sichtbar gemacht und die zugehörigen Overlays in der Vorschau ein- oder ausgeblendet werden; die Buttons „Alle einblenden/Alle ausblenden“ steuern sämtliche sichtbaren Layer gemeinsam.
7. Die kryptografische Bewertung (Hash-Vergleich, Signaturprüfung und Zertifikatsinformationen) wird zusammen mit Hinweisen in der Zusammenfassung angezeigt.

## Kategorien
- **Kategorie 1 – signierter Stand unverändert:** Signatur mathematisch gültig, keine nachträglichen Objekte erkannt.
- **Kategorie 2 – unklar / Formular- oder Layer-Update:** Nachträgliche Objekte enthalten überwiegend Formulare, Annotationen oder OCG-Strukturen.
- **Kategorie 3 – hohe Wahrscheinlichkeit für Manipulation:** Signatur ungültig oder nachträgliche Objekte deuten auf sichtbare Inhalte, XObjects oder andere Layout-Änderungen hin.
- **Kategorie 4 – keine Signatur:** Kein `/ByteRange` vorhanden; das Dokument ist unsigniert.

## Layer- und Objekt-Ansicht
Die Layer-Liste kombiniert Rohdaten mit einer visuellen Heuristik: Für jedes Objekt werden erkannte Koordinaten (z. B. `/Rect`, `/BBox`, `QuadPoints`) auf die pdf.js-Vorschau projiziert und die Differenz zum signierten Ursprung als transparentes Overlay eingeblendet. Wo keine Koordinaten vorliegen, wird – sofern ein Pixel-Differenzbild existiert – ein grobes Änderungsrechteck angezeigt. Komprimierte Streams (etwa mit `/Filter /FlateDecode`) können nur als potenziell relevante Daten markiert werden; fehlt jede Positionsinformation, bleibt die Checkbox deaktiviert. Über die Checkboxen lassen sich einzelne Objekte oder komplette Gruppen („Alle einblenden/Alle ausblenden“) sichtbar machen, sodass nachträgliche Ergänzungen schnell isoliert werden können.

## PDF-Vorschau
Das PDF wird vollständig innerhalb der Seite mit einer eingebetteten pdf.js-Instanz gerendert. Standardmäßig zeigt die Vorschau den signierten Stand; aktivierte Nachträge werden als zusätzliche Ebenen eingeblendet. So lässt sich unmittelbar nachvollziehen, welche Inhalte erst nach der Signatur hinzugekommen sind.

## Demos
Legen Sie Beispiel-PDFs in den Ordner `Demos/`, der sich neben `index.html` befindet. Öffnen Sie `index.html` direkt im Browser (z. B. per Doppelklick) und wählen Sie die gewünschte Demo-Datei über den Datei-Dialog aus. Reine HTML/JS-Seiten können ohne Webserver nicht automatisch auf den Ordner zugreifen; die Datei muss manuell ausgewählt werden.

## Limitierungen
- Keine PKI-Validierung der Zertifikatskette: OCSP-, CRL- oder Vertrauenskettentests werden nicht durchgeführt.
- Komprimierte oder verschlüsselte PDF-Objekte lassen sich nur begrenzt analysieren; es erfolgt eine textuelle Markierung ohne Dekompression.
- Die Overlays basieren auf heuristisch extrahierten Koordinaten und Pixel-Differenzen; je nach PDF können Position und Größe abweichen oder komplett fehlen.
- Das Ergebnis ist keine rechtsverbindliche Signaturprüfung und ersetzt keine tiefgehende forensische Analyse.

## Sicherheit
Alle Operationen laufen vollständig lokal im Browser. Es gibt keinerlei Netzwerkkommunikation oder Datei-Uploads; Zertifikate und Signaturen werden ausschließlich clientseitig verarbeitet.
