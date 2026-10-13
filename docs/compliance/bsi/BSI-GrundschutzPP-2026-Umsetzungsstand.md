# BSI-Grundschutz++ 2026 – Projektweiter Umsetzungsstand

**Projekt:** IT-Neuorganisation  
**Dokumenttyp:** zentrales Compliance-/Umsetzungsregister  
**Stand:** 2026-10-13  
**Bewertungsbasis:** BSI-Grundschutz++ 2026, Stand-der-Technik-Bibliothek  
**Bewertungsart:** vorläufige Projektbewertung aus dokumentiertem Projektkontext; kein formales Audit.

**Dokumentationsstandard:** BSI-Anforderungen, Erfüllungsbewertung, Defizite und Anforderungsnachweise werden ausschließlich hier gepflegt. Fachdokumentationen enthalten technische Designentscheidungen und den Soll/Ist-Zustand ohne BSI-Control-Mapping. Ein technischer Beleg ist nicht automatisch ein Compliance-Nachweis.

**Fachreferenz:** [SEC-DEF-001 – Defender Windows Integration und Betrieb](../../endpoint-security/defender/Defender-Windows-Integration-und-Betrieb.md)

## Statusmodell

| Status | Bedeutung |
|---|---|
| 🟢 erfüllt | im Projekt nachweisbar umgesetzt |
| 🟡 teilweise | wesentliche Teile umgesetzt bzw. beschlossen; Nachweise oder Teilmaßnahmen fehlen |
| 🔴 offen | noch nicht umgesetzt oder nicht ausreichend beschrieben |
| ⚪ N/A / Scope prüfen | für den betrachteten Informationsverbund ggf. nicht anwendbar |
| 🔵 zu verifizieren | Umsetzung wahrscheinlich, aber im Projektkontext nicht ausreichend nachgewiesen |

## Projektweiter Überblick

| Praktik | Vorläufiger Status | Projektbezug / vorhandene Maßnahmen | Wesentliche Lücke / nächster Nachweis |
|---|---:|---|---|
| GC – Governance und Compliance | 🟡 | Zielarchitektur, Rollen- und Sicherheitsentscheidungen werden projektbezogen dokumentiert | formale IS-Leitlinie, Compliance-Verzeichnis, Freigaben |
| STM – Strukturmodellierung | 🟡 | neue Infrastruktur wird nach Identität, Client, Netz, PKI und Diensten strukturiert | vollständiges Asset-/Informationsverbund-Modell |
| UMS – Umsetzung | 🟡 | technische Umsetzung läuft über Entra, Intune, Defender, Autopilot, PKI | zentrales Maßnahmenregister mit Owner/Termin/Nachweis |
| VRB – Verbesserung | 🔴 | punktuelle Nachsteuerung im Projekt | formalisierter Verbesserungsprozess und Wirksamkeitsprüfung |
| PERF – Monitoring & Evaluation | 🟡 | Intune-/Defender-Reporting vorgesehen | KPI, regelmäßige Review-Termine, Managementbericht |
| RISK – Risikomanagement | 🟡 | Risiken werden in Designentscheidungen berücksichtigt | formales Risikoregister, Risikoowner, Freigabe Restrisiken |
| ASST – Informationen und Assets | 🟡 | Intune/Entra/Defender liefern Geräte- und Softwarebestände | verbindliches Asset-Register und Sollabgleich |
| PERS – Personal | 🔵 | administrative Rollen und abgestufte Adminkonten geplant | Rollenbeschreibung, Vertretung, On-/Offboarding |
| BES – Beschaffungsmanagement | 🟡 | Autopilot-Konventionen für neue Geräte definiert | formaler Sicherheitsanforderungskatalog für Lieferanten |
| DLS – Dienstleistersteuerung | 🔵 | Microsoft und weitere Dienstleister sind architekturrelevant | Sicherheitsanforderungen, Nachweise, Eskalations- und Exit-Regeln |
| TEST – Änderungen und Tests | 🟡 | Pilot-/Audit-/Report-only-Vorgehen wird genutzt | formaler Change-/Testprozess inkl. Rückfallplan |
| GEB – Gebäudemanagement | ⚪ | nicht Schwerpunkt der neuen cloud-/clientorientierten Infrastruktur | Anwendbarkeit für Standorte separat bewerten |
| SENS – Sensibilisierung | 🔴 | im Projektkontext nicht ausreichend dokumentiert | Awareness-/Schulungsplan inkl. Schadsoftware und Phishing |
| ARCH – Architektur | 🟡 | Zero Trust, Entra, Intune, EPA/GSA und Segmentierung bilden Zielarchitektur | formale Architekturfreigabe, Datenflüsse, Abhängigkeiten |
| BER – Berechtigung | 🟡 | gestufte Adminkonten, Entra CA, PIM/Passkeys/YubiKey-Konzept | vollständige RBAC-Matrix und regelmäßige Rezertifizierung |
| NOT – Notfallplanung | 🔴 | Break-glass-Konten betrachtet | Notfallhandbuch, Wiederanlauf, Kommunikations- und Recovery-Tests |
| DET – Detektion | 🟡 | Microsoft Defender/EDR/XDR als primäre Plattform | 24/7-MDR/SOC noch Zielzustand; Monitoring- und Logging-Konzept formalisieren |
| REA – Sicherheitsvorfallsbehandlung | 🟡 | Isolation, Remediation und CA-Sperrkette vorgesehen | Incident-Runbooks, Eskalation, MDR/SOC, Übungen |
| KONF – Konfiguration | 🟡 | Intune-basierte zentrale Windows-Konfiguration, Defender, Autopilot | vollständiger Soll-Export, Ausnahmeprozess, Drift-Kontrolle |
| DEV – Entwicklung | ⚪ | derzeit kein eigener Softwareentwicklungs-Workstream dokumentiert | bei Eigenentwicklung Scope neu bewerten |

## Schwerpunkt: Endpoint Security / Defender

| Grundschutz++ Bezug | Umsetzung durch Zielarchitektur | Status | Defizit / Restrisiko |
|---|---|---:|---|
| KONF.2.1 Grundkonfiguration für Systeme | Intune Security Baselines und Endpoint-Security-Policies als verbindlicher Windows-Sollzustand | 🟡 | tatsächlicher Tenant-Export als Nachweis fehlt |
| KONF.7.1 Echtzeitscanner | Microsoft Defender Antivirus Always-on Protection | 🔵 | Wirksamkeit auf repräsentativen Clients verifizieren |
| KONF.7.2 Regelmäßige Scans | Defender geplante/ereignisbasierte Scans | 🔵 | Scan-Policy und Reporting verifizieren |
| KONF.7.3 Host-basierte Angriffserkennung | Defender for Business EDR | 🟡 | EDR-Onboardingstatus vollständig nachweisen |
| KONF.7.4 Angriffserkennung anhand von Netzverkehr | Network Protection / EDR-Telemetrie | 🟡 | Scope und Block-/Audit-Status dokumentieren |
| KONF.7.9 Einschränkung der Installation | Intune/App-Control/EPM-Konzept | 🟡 | vollständige Allow-/Elevation-Strategie noch nicht abgeschlossen |
| KONF.7.10 Einschränkung der Ausführung | ASR / Application Control | 🟡 | Kompatibilitätstest und finaler Block-Modus nachweisen |
| KONF.7.12 Einschränkung von Skripten | ASR, PowerShell-Härtung, EDR | 🟡 | Zielmodus und Ausnahmen dokumentieren |
| DET.3.1 ff. Protokollierung | Defender Device Timeline, Alerts/Incidents, Windows Security Events | 🟡 | Aufbewahrungs- und Exportkonzept festlegen |
| DET.4.1 Überwachung der Protokollierung | Sensor Health / Device Inventory / EDR Onboarding Status | 🟡 | regelmäßige Kontrolle und KPI festlegen |
| DET.2.2 Security Operations Center | externer MDR/SOC als Zielzustand | 🔴 | 24/7-Betrieb noch nicht beschafft/umgesetzt |
| REA – Vorfallsbehandlung | Isolation, Scan, Investigation, Remediation, CA-Sperrkette | 🟡 | organisatorische Runbooks und Eskalationsmatrix fehlen |

## Nachverfolgbarkeit – Defender

Die folgende Matrix verweist auf die stabilen Designentscheidungen in [SEC-DEF-001](../../endpoint-security/defender/Defender-Windows-Integration-und-Betrieb.md#2-designentscheidungen). **Control-Bezeichnungen/IDs und Abdeckung sind vorläufig** und vor einer Audit-Freigabe mit dem versionierten Originalkatalog abzugleichen. Verlinkte Entwürfe ersetzen keine Ist-Nachweise.

| Grundschutz++ Bezug | Technische Design-IDs | Erforderliche Evidenz |
|---|---|---|
| KONF.2.1 | DEF-02, DEF-03, DEF-04, DEF-06, DEF-10 | Intune-Policy-Export, Gerätezuweisung |
| KONF.7.1 | DEF-04, DEF-05, DEF-06 | AV-Policy und realer Geräteschutzstatus |
| KONF.7.2 | DEF-04, DEF-05 | Scan-Konfiguration und Scan-Bericht |
| KONF.7.3 | DEF-01, DEF-03, DEF-15 | EDR-Onboarding-/Sensorbericht |
| KONF.7.4 | DEF-07 | Network-Protection-Policy und Block-Test |
| KONF.7.9 | DEF-08, DEF-11 | Application-Control-/EPM-Policies |
| KONF.7.10 | DEF-08, DEF-09 | ASR-Regelreport, App-Control-Auswertung |
| KONF.7.12 | DEF-08 | ASR-/Script-Härtungsnachweis |
| DET.3.1 ff. | DEF-01, DEF-03 | Incident-/Alert- und Retention-Konzept |
| DET.4.1 | DEF-03 | EDR-Onboarding-Status und Health-Review |
| DET.2.2 | DEF-17 | MDR-Vertrag, 24/7-Eskalation, Übungsprotokoll |
| REA | DEF-12, DEF-13, DEF-14, DEF-15, DEF-16, DEF-17 | CA-Test, Incident-Runbooks und Response-Protokoll |

## Nachweisführung

Für jeden als 🟢 bewerteten Punkt muss mindestens ein belastbarer Nachweis referenziert werden, z. B.:

- Policy-Export / JSON
- Intune Assignment- und Device-Status
- Defender Device Inventory / Sensor Health
- Defender Incident / Action Center
- Entra Sign-in Log mit Conditional-Access-Ergebnis
- freigegebene Designentscheidung
- Testprotokoll
- Screenshot nur ergänzend, nicht als alleiniger Nachweis

## Offene Grundschutz++-Schwerpunkte

1. formales Risikoregister und Restrisikofreigabe
2. IS-/Security-Governance mit freigegebenen Rollen
3. vollständige Asset- und Strukturmodellierung
4. Notfall- und Wiederanlaufplanung
5. Awareness- und Schulungsprogramm
6. MDR/SOC als 24/7-Zielbetrieb
7. zentraler Nachweis- und Change-Prozess
