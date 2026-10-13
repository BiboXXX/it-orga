# Dokumentationsstandard – IT-Neuorganisation

**Dokumenten-ID:** STD-DOC-001  
**Version:** 1.0 · **Stand:** 2026-10-13

## 1. Trennung der Verantwortlichkeiten

| Dokumenttyp | Enthält | Enthält ausdrücklich nicht |
|---|---|---|
| Fachdokumentation | Scope, Architektur, Designentscheidungen mit stabilen IDs, Soll/Ist, Sicherheitswirkung, Restrisiken, Konfiguration, englische verifizierte Clickpaths, Regelbetrieb, technische Betriebsnachweise | BSI-Control-Zuordnungen, Compliance-Status, Normerfüllungsquoten |
| Zentrales BSI-Register | aktuelle Grundschutz++-Controls, Mapping auf technische Entscheidungs-IDs, Bewertungsstatus, Kontrollinhaber, Evidenzen, Defizite, Restrisikofreigaben | duplizierte Konfigurationsanleitungen |
| BSI-Quellenmanifest | Referenzquelle, Versionsstand/Commit, Quellintegrität | anwendungsspezifischen Tenant-Istzustand |

## 2. Pflege und Auditfähigkeit

- Designentscheidungen erhalten stabile IDs, z. B. `DEF-01`. IDs dürfen nicht neu durchnummeriert werden, wenn später Entscheidungen entfallen.
- Kontroll-IDs und die Erfüllungsbewertung werden **nur im BSI-Register** gepflegt.
- Jede Compliance-Bewertung nennt den technischen Nachweis und – nach dessen Erhebung – einen konkreten Evidenzfundort, Erhebungsdatum und Verantwortlichen.
- **Geplant ≠ umgesetzt ≠ nachgewiesen.** Unbekannte Istwerte sind als `zu verifizieren` zu dokumentieren.
- Security-relevante Maßnahmen enthalten im Fachdokument immer Begründung, Sicherheitswirkung und Restrisiko.
- Änderungen an Microsoft-Clickpaths sind anhand aktueller Herstellerdokumentation vor Veröffentlichung zu prüfen und mit Prüfdatum zu versehen.
- BSI-Katalogänderungen führen zunächst zu einer Änderung des zentralen Registers. Die Fachunterlagen ändern sich nur, wenn technische Entscheidungen oder Betriebsabläufe angepasst werden müssen.
- Jede größere Änderung erfolgt versioniert über GitHub; Audit-Freigaben sollen nach einem Review erfolgen.

## 3. Aktuelle Referenzen

- [Zentrales BSI-Grundschutz++-Register](compliance/bsi/BSI-GrundschutzPP-2026-Umsetzungsstand.md)
- [BSI-Quellenmanifest](compliance/bsi/BSI-GrundschutzPP-2026-Quellenmanifest.md)
- [SEC-DEF-001: Defender – Windows-Integration und Betrieb](endpoint-security/defender/Defender-Windows-Integration-und-Betrieb.md)

**Geltungsbereich:** alle neuen Fach- und Compliance-Dokumente im Repo `it-orga`.
