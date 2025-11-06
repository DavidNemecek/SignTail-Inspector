# SignTail Inspector

SignTail Inspector ist eine vollständig lokale HTML-Anwendung zur forensischen Analyse signierter PDF-Dokumente. Die Seite kombiniert einen eingebetteten pdf.js-Viewer, kryptografische Signaturprüfungen sowie eine Auflistung aller inkrementellen Objekt-Nachträge. Sämtliche Verarbeitungsschritte erfolgen offline im Browser – es werden keine Dateien an einen Server übertragen.

## Funktionsumfang

- **Paralleles Rendering:** Der signierte Ausgangsstand und das finale Dokument werden nebeneinander gerendert, um Differenzen schnell sichtbar zu machen.
- **Automatische Objekt-Aktivierung:** Alle gefundenen, nicht signierten Objekte werden nach dem Laden eines Dokuments sofort aktiviert und – sofern Geometrieinformationen vorliegen – als Overlay in der Vorschau hervorgehoben.
- **Deutlicher Warnhinweis:** Sobald nachsignierte oder anderweitig unsignierte Objekte erkannt werden, erscheint unterhalb des Headers ein roter Warnbalken mit Ausrufezeichen über die gesamte Breite der Seite.
- **Übersichtliche Objektliste:** Für jedes Objekt stehen Metadaten, erkannte Koordinaten und ein Rohdaten-Ausschnitt bereit. Mit den Buttons „Alle anzeigen/Ausblenden“ lässt sich die Sichtbarkeit zentral steuern.
- **Mehrsprachige Oberfläche:** Die Benutzeroberfläche kann per Dropdown zwischen Deutsch und Englisch wechseln.

## Nutzung

1. Öffnen Sie die Datei `index.html` direkt im Browser (z. B. per Doppelklick oder über einen lokalen Webserver).
2. Ziehen Sie ein signiertes PDF auf die Upload-Fläche oder wählen Sie eine Datei über den Dateidialog aus.
3. Nach dem Laden zeigt die Zusammenfassung Dateiname, Signaturstatus, ByteRange und Anzahl der gefundenen Objekte an.
4. Sind nicht signierte Objekte vorhanden, werden sie automatisch markiert, das entsprechende Overlay aktiviert und der Warnbalken eingeblendet.
5. Nutzen Sie die Objektliste, um einzelne Einträge ein- oder auszublenden, Rohdaten anzusehen oder sich auf bestimmte Seiten zu fokussieren.

## Demos & Beispiele

Beispieldateien können im Ordner `demos/` abgelegt werden. Da die Anwendung rein statisch ist, muss die gewünschte Datei manuell über den Dateidialog ausgewählt werden – ein direkter Zugriff auf das Dateisystem ist aus Sicherheitsgründen nicht möglich.

## Grenzen der Analyse

- Die Zertifikatskette wird nicht gegen externe Vertrauensanker, OCSP oder CRLs geprüft.
- Komprimierte, verschlüsselte oder anderweitig binäre PDF-Objekte lassen sich nur eingeschränkt interpretieren; sie werden textuell markiert, bleiben aber im Zweifelsfall unvollständig.
- Positionsangaben werden heuristisch aus `/Rect`, `/BBox`, `QuadPoints` & Co. gewonnen. Je nach PDF-Struktur können Überlagerungen daher leicht abweichen oder komplett fehlen.
- Die Auswertung ersetzt keine rechtlich verbindliche Signaturprüfung oder eine umfassende forensische Analyse.

## Datenschutz & Sicherheit

Alle Operationen laufen lokal im Browser. Weder PDF-Dateien noch Zertifikatsinformationen verlassen das Gerät, wodurch Signaturprüfungen auch in sensiblen Umgebungen ohne Netzwerkzugriff möglich sind.
