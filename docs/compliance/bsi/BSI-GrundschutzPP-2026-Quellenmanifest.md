# BSI-Grundschutz++ 2026 – Quellenmanifest

**Projekt:** IT-Neuorganisation  
**Stand der Erhebung:** 2026-10-11  
**Zweck:** Nachweis der verwendeten Referenzbasis für die projektweite Grundschutz++-Bewertung.

## Maßgebliche Referenzbasis

Primäre Referenz ist die vom BSI veröffentlichte **Stand-der-Technik-Bibliothek** mit dem
maschinenlesbaren Grundschutz++-Anwenderkatalog im OSCAL-Format.

| Quelle | Verwendung |
|---|---|
| BSI Themenseite Grundschutz++ | fachliche Einstiegs- und Veröffentlichungsseite |
| BSI-Bund/Stand-der-Technik-Bibliothek | maßgebliche maschinenlesbare Referenz |
| `control_layer/Grundschutz++/Grundschutz++-resolved_catalog.json` | aufgelöster Grundschutz++-Anwenderkatalog |
| `control_layer/Grundschutz++/sources/` | Quellkataloge und Profile |
| `control_layer/Risikomanagement/` | Anforderungen zum Risikomanagement |
| `control_layer/Mappings/` | veröffentlichte Mapping-Sammlungen |
| `implementation_layer/` | veröffentlichte Implementierungsbeschreibungen |
| `documentation/namespaces/` | Definitionen der OSCAL-Metadaten und Namensräume |

## Relevante URLs

- BSI Grundschutz++:
  https://www.bsi.bund.de/DE/Themen/Unternehmen-und-Organisationen/Standards-und-Zertifizierung/Grundschutz-in-der-Informationssicherheit/Grundschutz-Plus-Plus/grundschutz-plus-plus_node.html
- BSI Stand-der-Technik-Bibliothek:
  https://github.com/BSI-Bund/Stand-der-Technik-Bibliothek
- Grundschutz++ resolved catalog:
  https://github.com/BSI-Bund/Stand-der-Technik-Bibliothek/blob/main/control_layer/Grundschutz%2B%2B/Grundschutz%2B%2B-resolved_catalog.json

## Versionsbezug

Der offizielle BSI-Katalog ist eine **Rolling Publication**. Anforderungen und IDs können sich ändern.
Alle projektspezifischen Dokumentationen müssen deshalb mindestens folgende Metadaten enthalten:

- Datum des letzten BSI-Abgleichs
- verwendete Katalogversion / Commit, soweit verfügbar
- Grundschutz++ Control-ID
- Status der Umsetzung
- Nachweis
- Abweichung / Restrisiko

## Lokale Ablage

Dieses Manifest ist kein Ersatz für den offiziellen BSI-Katalog. Der vollständige maschinenlesbare
Katalog soll im Projekt als unveränderte Referenzkopie archiviert werden. Da der veröffentlichte
resolved catalog mehrere Megabyte groß ist und über die hier verfügbare GitHub-Schnittstelle nicht
vollständig als Binär-/Rohdatei materialisiert werden konnte, verweist diese Version auf die
autoritative BSI-Quelle. Für Auditzwecke ist beim Archivieren zusätzlich ein Hash der heruntergeladenen
Originaldatei zu erfassen.

## Prüfhinweis

Automatisch erzeugte oder community-basierte Crosswalks dürfen nur als Orientierung verwendet werden.
Für die projektinterne Bewertung ist die BSI-Control-ID und der originale BSI-Anforderungstext maßgeblich.
