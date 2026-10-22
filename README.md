# ICC Berlin – Digitale Begehung

Begehbares 3D-Modell des Internationalen Congress Centrums Berlin mit Three.js (r128). Enthalten sind Vorraum, Ebene 0 (Boulevard mit Seitenschiffen und der Lichtplastik „Das Gehirn“), Ebene 1 und 2, Saal 1, Saal 2, Bühne, Saal 6 und weitere Säle, Dachterrasse und Tragwerk, das Autofoyer, die dreigeschossige Brücke zu den Messehallen 14/15, der Fußgängertunnel unter der Kreuzung Messedamm/Masurenallee/Neue Kantstraße, die Unterführung der A100, Parkhaus mit Dachgarten, ZOB, S-Bahn und Funkturm, dazu ein Stadtpanorama als Hintergrund.

Das Modell ist eine Rekonstruktion nach öffentlichen Beschreibungen (Landesdenkmalamt Berlin, Wikipedia, schramms.de) und Eckdaten (313 × 89 × 40 m). Grundrisse, Raumfolgen und Ausstattung sind vereinfacht und teils angenommen. Alle Texturen und das Panorama werden prozedural im Browser erzeugt; es sind keine fremden Bilder eingebettet.


## Datengrundlage (Version 3)

- Außenform des ICC nach Fotos (Wikimedia Commons) und dem OSM-Grundriss: Nordblock mit Kragbändern, zehn Dachbuchten mit geneigten Schultern und Querrippen, je Seite zwei Fachwerkträger und vier Doppelzylinder-Türme, Bühnenhaus, Brücke (OSM: 5–20 m) und Parkhaus mit Dachgarten und Rundturm.
- Umgebung aus einem OpenStreetMap-Export (© OpenStreetMap-Mitwirkende, ODbL): Gebäude mit Höhen, Straßen und Wege mit Breiten, Gleise, Bahnsteige, Grünflächen, Bäume. Höhenlagen (A100-Trog, Ringbahn-Einschnitt, Rampen, Tunnel) werden aus den OSM-Ebenen mit begrenzter Steigung abgeleitet.
- Die Weltdaten stehen als JSON im ersten Skriptblock (`window.__WD`). Koordinaten: Meter, Ursprung im ICC, z entlang der Längsachse (Süden positiv), um 12,4° gegen Nord gedreht.
- Testmodus ohne Schatten: `index.html?lite`.
- Hauptkörper nach hochauflösenden Luftbildern: zehn getrennte Dachbuchten mit breiten Querrippen, senkrechte Lamellenwand hinter den Fachwerkträgern und gewölbte Schulter darüber, Trägerpfosten auf den Rippen, dicht gerippte Zylindertürme, Oberlichter und Technikaufbauten auf dem Nordblock. Sommerdunst als Standardlicht, natürlichere Bäume und Bodenflächen, Stadtring aus Blockrandbebauung mit Ziegeldächern.
- Fußgängertunnel nach Plan und Innenfoto: Säulenhalle unter der Kreuzung mit runden, orange gefliesten Säulen und schwarzen Trommelleuchten, orange geflieste Brüstungen an den Abgängen, oranges Portal am ICC und braune Kastenpavillons an den übrigen Zugängen. Messe-Haupteingang Nord mit Turm, Schriftzug „MESSE BERLIN“, Säulenvorhalle und Fahnenwald auf dem Hammarskjöldplatz.
- Weitere Details nach Street View: gerundete Fensterrahmen in den Lamellenwänden und am Brückenuntergeschoss, Rampenspindel und Lüftungsöffnungen am Parkhaus, Treppenturm, Skulptur und Fahnenmasten am Eingang Ost der Messe, Sheddächer mit „Funkturm Restaurant“, Schilderbrücke über der A100, orange geflieste Unterführungswände am Messedamm.
- Südende und Ostseite nach Street View: Parkhaus mit gerundeten Deckbändern, Lüftungsrohr und orangen Tunneleinfahrten, Bullaugen an der Südstirn, dunkle Lüftungstürme und Paneelzaun an der Ostseite, zwei Säulen-Rundbauten am Messedamm (OSM Halle 13 und 16).
- Ergänzt nach Street-View-Aufnahmen (nur als Vorlage, nicht eingebettet): Fahnenmasten am Vorplatz, Backsteinpylone und Graffiti-Brüstungen an der Brücke Neue Kantstraße, S-Bahnhofsgebäude Messe Nord/ZOB mit Walmdach, Rundbau mit Säulen am Messedamm, Ziegeldächer der Wohnhäuser, Straßenleuchten entlang der Hauptstraßen, Lichtmodus „Bewölkt“.

## Starten

Die Anwendung ist eine einzelne Datei ohne Build-Schritt. Three.js wird von cdnjs geladen.

```bash
python3 -m http.server 8000
# dann http://localhost:8000 öffnen
```

Über GitHub Pages (Settings → Pages → Branch `main`, Ordner `/`) ist sie direkt online erreichbar.

## Bedienung

| | Smartphone/Tablet | Desktop |
|---|---|---|
| Gehen | linke Bildschirmhälfte ziehen | W A S D oder Pfeiltasten, Umschalt = schneller |
| Umsehen | rechte Bildschirmhälfte ziehen | Maus mit gedrückter Taste ziehen |
| Übersicht | ziehen dreht, zwei Finger zoomen/verschieben, tippen springt hin | ziehen dreht, rechte Maustaste verschiebt, Mausrad zoomt, Klick springt hin |

„Orte“ springt zu 19 festen Standpunkten. In der Übersicht blendet „Dach ausblenden“ Dächer und Decken aus. „Nacht“ schaltet die Beleuchtung um.

## Aufbau des Codes (`index.html`)

| Abschnitt | Inhalt |
|---|---|
| Himmel, Licht, Nebel | Verlaufshimmel, Sonne mit Schatten, Hemisphären- und Umgebungslicht |
| Texturen, Materialien | prozedurale Canvas-Texturen (Aluminium, Fliesen, Asphalt, Pflaster …) |
| Geometrie-Helfer | `B()` Kollisionsboxen, `D()` Deko, `slab()` Flächen mit Aussparungen, `rampZ/rampX`, `stairsZ/stairsX`, Instancing |
| `buildICC()` | Hülle aus Profil-Extrusionen, Querschotten, Säle, Bühne, Foyers, Dachtragwerk |
| `buildBridge()`, `buildMesse()` | Übergang und Messehalle |
| `buildTunnel()` | Säulenhalle, drei Gänge, Treppen mit Rolltreppen |
| `buildA100()`, `buildSBahn()`, `buildZOB()`, `buildFunkturm()` | Umgebung |
| `STATIONS`, `ZONES` | Sprungpunkte und Infotexte je Bereich |
| Spieler und Kollision | Raycasts nach unten (Boden, Treppen) und horizontal (Wände, Gleiten) |

Koordinaten: Meter, y nach oben, Norden = −z, Osten = +x. Das ICC liegt mittig im Ursprung, der Messedamm westlich (x ≈ −80), die A100 östlich (x ≈ 95), die Neue Kantstraße nördlich (z ≈ −193).

## Genauere Daten einbinden

Liegen Pläne oder ein CAD-, IFC- oder glTF-Modell vor, lassen sich die schematischen Bauteile ersetzen, zum Beispiel per `GLTFLoader` mit eigenen Kollisionsflächen.
