# Microsoft Defender – Windows-Integration, Konfiguration und Regelbetrieb

**Projekt:** IT-Neuorganisation  
**Scope:** ausschließlich neue Windows-Client-Infrastruktur  
**Server:** ausgeschlossen  
**Stand:** 2026-10-11  
**Referenz:** BSI-Grundschutz++ 2026  
**Produktbasis:** Microsoft Defender for Business / Microsoft Defender XDR, Intune, Entra Conditional Access  
**UI-/Clickpath-Abgleich:** 2026-10-11 gegen Microsoft Learn

> **Auditgrundsatz:** Nicht im Projekt nachgewiesene Tenant-Einstellungen werden nicht als Ist-Zustand behauptet.
> Sie sind mit **zu verifizieren** gekennzeichnet. Der Soll-Zustand ist davon getrennt.

## 1. Zielbild

Microsoft Defender ist der primäre Endpoint-Security-/EDR-Stack für Windows-Clients.

**Reaktionskette:**

`Defender Detection → Device Risk → Intune Non-compliant → Entra Conditional Access → Zugriff entzogen → Incident Response`

Der Zielzustand ergänzt diese technische Kette um einen externen **24/7 MDR/SOC-Dienst**.

## 2. Designentscheidungen

| Entscheidung | Soll | Sicherheitswirkung | Restrisiko / Betriebswirkung | BSI-GS++ Bezug |
|---|---|---|---|---|
| Defender als primärer Endpoint-Schutz | ja | einheitliche AV/EDR/XDR-Telemetrie; enge Entra-/Intune-Integration | Herstellerabhängigkeit | KONF, DET, REA |
| Intune ist führende Policy-Quelle | ja | reproduzierbare und zentral nachweisbare Konfiguration | Konflikte bei paralleler GPO/Defender-Policy möglich | KONF.2.1 |
| Windows-Clients automatisch zu MDE onboarden | ja | vollständige EDR-Sicht und Device-Risk-Signal | fehlerhaftes Assignment erzeugt blinde Flecken | DET, KONF |
| Defender Antivirus Always-on | aktiviert | Echtzeitschutz gegen Schadcode | mögliche App-Kompatibilitätsprobleme | KONF.7.1 |
| Behavior Monitoring / Cloud-delivered Protection | aktiviert | bessere Erkennung unbekannter/Verhaltens-basierter Angriffe | Cloudabhängigkeit, Datenschutzprüfung | KONF.7, DET |
| Tamper Protection | aktiviert | verhindert lokale Manipulation sicherheitskritischer Einstellungen | erschwert lokale Fehlersuche | KONF |
| Network Protection | **Block** | blockiert bekannte schädliche Ziele/Netzaktivität | mögliche Fehlblockierungen | KONF.7.4 |
| ASR Rules | stufenweise: Audit → Block; Microsoft-Standardregeln unmittelbar restriktiv | reduziert Office-/Script-/LOLBin-Angriffsfläche | Kompatibilitätsrisiken, daher Pilotierung | KONF.7.10, KONF.7.12 |
| Controlled Folder Access | Pilot/Audit, danach gezielt Block | zusätzlicher Ransomware-Schutz | hoher Abstimmungsbedarf bei Fachanwendungen | KONF.7 |
| Windows Firewall | alle Profile aktiv; eingehend Default-Deny | reduziert laterale Angriffsfläche | notwendige Ausnahmen müssen gepflegt werden | ARCH, KONF |
| AV-/ASR-Ausnahmen | nur eng begründet, dokumentiert und befristet | verhindert dauerhafte Schutzlücken | Fehlkonfiguration kann Anwendungen beeinträchtigen | UMS, KONF |
| Device-Risk Compliance | **Low** | Medium/High Risk wird non-compliant | Fehlklassifizierung kann Zugriff sperren | DET, REA |
| Conditional Access | `Require device to be marked as compliant` für relevante Unternehmensressourcen; Break-glass ausgenommen | kompromittierte Geräte verlieren Cloud-Zugriff | Fehlkonfiguration kann Benutzer aussperren | BER, REA |
| CA-Rollout | zunächst `Report-only`, dann `On` | reduziert Lockout-Risiko | Übergangsphase ohne Blockwirkung | TEST |
| Automated Investigation / Remediation | aktiviert | verkürzt Reaktionszeit | automatische Aktionen müssen überprüft werden | REA |
| Device Isolation | Admin-Maßnahme bei bestätigtem/hoch wahrscheinlichem Incident | stoppt laterale Bewegung und Datenabfluss | betroffener Client verliert Netzkonnektivität | REA |
| MDR/SOC | Zielzustand 24/7 | menschliche Bewertung und Reaktion außerhalb Dienstzeit | bis Umsetzung verbleibt organisatorische Reaktionslücke | DET.2.2, REA |

## 3. Soll-Konfiguration

### 3.1 Intune ↔ Defender Verbindung

| Parameter | Soll | Ist |
|---|---|---|
| Intune connection | On | zu verifizieren |
| Connect Windows devices to Defender for Endpoint | On | zu verifizieren |
| EDR onboarding | Auto from connector | zu verifizieren |
| Sample Sharing | All, sofern Datenschutzfreigabe vorliegt | zu verifizieren |

**Aktueller Clickpath – Verbindung prüfen**

`Microsoft Intune admin center → Endpoint security → Defender for Endpoint`

**Verbindung aktivieren**

`Microsoft Defender portal → System → Settings → Endpoints → General → Advanced features → Intune connection → On → Save preferences`

**Windows-EDR-Policy**

`Microsoft Intune admin center → Endpoint security → Endpoint detection and response → Summary → Create Policy → Platform: Windows → Profile: Endpoint detection and response`

Empfehlung: **Custom setup** statt pauschaler All-Devices-Schnellbereitstellung, damit Pilot-, Produktions- und Ausnahmegruppen sauber steuerbar bleiben.

### 3.2 Defender Antivirus

**Clickpath**

`Microsoft Intune admin center → Endpoint security → Antivirus → Create Policy → Platform: Windows → Profile: Microsoft Defender Antivirus`

| Einstellung | Soll |
|---|---|
| Allow Real-Time Monitoring | Allowed |
| Allow On Access Protection | Allowed |
| Real Time Scan Direction | Monitor all files |
| Allow behavior monitoring | Allowed |
| Cloud-delivered protection | Enabled |
| PUA protection | Block |
| Network protection | Enabled (block mode) |
| Tamper Protection | Enabled |
| Exclusions | keine pauschalen Ausnahmen; nur dokumentierte Einzel-Ausnahmen |

### 3.3 Attack Surface Reduction

**Clickpath**

`Microsoft Intune admin center → Endpoint security → Attack surface reduction → Create Policy → Platform: Windows → Profile: Attack Surface Reduction Rules`

Designregel:

- Microsoft-Standard-Schutzregeln: soweit von Microsoft als ohne vorgelagerten Test geeignet eingestuft, `Block` bzw. `Warn`.
- alle übrigen Regeln: Pilot in `Audit`; nach Auswertung und Fachanwendungstest `Block`.
- Ausnahmen ausschließlich regelbezogen und dokumentiert.
- Gerätegruppen verwenden; keine Benutzergruppen für MDE Security Settings Management.

### 3.4 Firewall

**Clickpath**

`Microsoft Intune admin center → Endpoint security → Firewall → Create Policy`

Soll:

- Domain / Private / Public Firewall: Enabled
- eingehende Verbindungen: grundsätzlich blockieren, sofern nicht explizit erlaubt
- Regeln zentral verwalten und dokumentieren
- keine dauerhaften lokalen Benutzer-Ausnahmen

### 3.5 Device Risk → Compliance

**Clickpath**

`Microsoft Intune admin center → Devices → Manage devices → Compliance → Policies → Create policy → Windows 10 and later → Compliance settings → Microsoft Defender for Endpoint`

Sollwert:

`Require the device to be at or under the machine risk score = Low`

**Wirkung:** Geräte mit `Medium` oder `High` Risk werden non-compliant.

### 3.6 Conditional Access

**Clickpath**

`Microsoft Intune admin center → Endpoint security → Conditional Access → Create new policy`

Soll:

- Include: definierter Benutzer-Scope
- Exclude: Emergency/Break-glass accounts
- Target resources: relevante Unternehmensressourcen, perspektivisch breit
- Grant: `Require device to be marked as compliant`
- initial: `Report-only`
- nach erfolgreichem Test: `On`

## 4. BSI-Grundschutz++-Zuordnung

| Control | Relevanz | Defender-/Intune-Umsetzung | Bewertung |
|---|---|---|---|
| KONF.2.1 Grundkonfiguration für Systeme | verbindliche Client-Baseline | Intune Endpoint Security / Baselines | teilweise, Tenant-Nachweis erforderlich |
| KONF.7.1 Echtzeitscanner | Schadcodeprävention | Defender Antivirus Real-time Protection | Soll erfüllt, Ist verifizieren |
| KONF.7.2 Regelmäßige Scans | Schadcodeprävention | Defender Scans / Cloud Protection | Ist verifizieren |
| KONF.7.3 Host-basierte Angriffserkennung | EDR | Defender for Business EDR | konzeptionell erfüllt |
| KONF.7.4 Angriffserkennung anhand von Netzverkehr | EDR/Network Protection | Network Protection + EDR | konzeptionell erfüllt |
| KONF.7.9 Einschränkung der Installation | Softwarekontrolle | Intune, EPM/Application Control | teilweise |
| KONF.7.10 Einschränkung der Ausführung | Execution Control | ASR/Application Control | teilweise |
| KONF.7.12 Einschränkung von Skripten | Script-Abuse | ASR + PowerShell-Härtung | teilweise |
| DET.3.1 ff. Protokollierung | Ereignisnachweis | Device Timeline / Incidents / Alerts | teilweise |
| DET.4.1 Überwachung der Protokollierung | Sensor-/Logging-Health | EDR Onboarding Status / Device Inventory | teilweise |
| DET.2.2 SOC | 24/7-Bewertung | MDR/SOC Zielzustand | offen |
| REA | Incident Response | Isolation, Scan, Investigation, Remediation, CA-Sperre | technisch weitgehend vorhanden; Prozess offen |

## 5. Regelbetrieb für Administratoren

Nur Tätigkeiten, die im normalen Betrieb regelmäßig erforderlich sind.

### 5.1 Incidents prüfen

**Clickpath**

`Microsoft Defender portal → Incidents & alerts → Incidents → <Incident>`

Prüfen:

1. Severity und Status
2. betroffene Devices und Users
3. Alerts und Attack Story
4. bereits automatisch ausgeführte Actions
5. Device Risk und aktuelle Aktivität

**Eskalieren**, wenn High Severity, bestätigte Credential-/Lateral-Movement-Indikatoren,
mehrere Geräte oder Datenabflussverdacht vorliegen.

### 5.2 Gerät untersuchen

**Clickpath**

`Microsoft Defender portal → Assets → Devices → <Device>`

Alternativ aus einem Incident:

`Microsoft Defender portal → Incidents & alerts → Incidents → <Incident> → select affected device`

Regelmäßig prüfen:

- Risk level
- Exposure level
- Active alerts
- Logged-on users
- Timeline
- Security recommendations

Für ASR-Ereignisse:

`Assets → Devices → <Device> → Timeline → Filter → Event group: ASR events → Apply`

### 5.3 Gerät isolieren

**Voraussetzung:** bestätigter oder hoch wahrscheinlicher Incident bzw. akute Eindämmung erforderlich.

**Clickpath**

`Microsoft Defender portal → Assets → Devices → <Device> → Response actions → Isolate device`

Nach Isolation:

- Incident weiter untersuchen
- automatisierte Investigation/Remediation kontrollieren
- Benutzer-/Identitätsrisiken separat prüfen
- bei Bedarf AV-Scan / Investigation Package auslösen

Isolation erst aufheben, wenn Ursache und Persistenz ausreichend behandelt sind.

### 5.4 Antivirus-Scan auslösen

**Clickpath**

`Microsoft Defender portal → Assets → Devices → <Device> → Response actions → Run antivirus scan`

Verwenden bei verdächtigen Dateien/Verhaltensindikatoren oder als ergänzende Maßnahme nach Remediation.

### 5.5 Automatische Maßnahmen kontrollieren

**Clickpath**

`Microsoft Defender portal → System → Action center`

Prüfen:

- ausgeführte oder ausstehende Remediation
- fehlerhafte Aktionen
- Rücknahme-/Freigabeentscheidungen
- Zusammenhang zum Incident

### 5.6 EDR-Onboarding überwachen

**Clickpath**

`Microsoft Intune admin center → Endpoint security → Endpoint detection and response → EDR Onboarding Status`

Ziel: alle produktiven Windows-Clients `Successfully onboarded`.

Zusätzlicher Nachweis:

`Microsoft Defender portal → Assets → Devices`

### 5.7 Compliance-Fehler prüfen

**Clickpath**

`Microsoft Intune admin center → Devices → Compliance → Device compliance`

Bei durch Defender verursachter Non-Compliance:

1. Device Risk im Defender-Portal prüfen
2. Incident/Alerts bearbeiten
3. Remediation abschließen
4. erneute Risikobewertung abwarten
5. Zugriff erst nach Compliance-Wiederherstellung zulassen

## 6. Nachweise für Audit

| Kontrolle | Primärnachweis |
|---|---|
| Defender-Onboarding | Intune `EDR Onboarding Status` + Defender `Assets → Devices` |
| AV-/ASR-Policy | Intune Policy Export + Assignment/Device Status |
| Firewall | Intune Firewall Policy + Device Status |
| Risk Threshold | Intune Compliance Policy Export |
| Zugriffssperre | Entra Sign-in Log / Conditional Access result |
| Incident Response | Defender Incident + Action Center |
| Isolation | Device Action History / Incident Activities |
| Ausnahmen | freigegebenes Ausnahmeverzeichnis mit Owner, Grund und Ablaufdatum |

## 7. Wesentliche Defizite / Restrisiken

| Defizit | Auswirkung | Priorität |
|---|---|---:|
| MDR/SOC 24/7 noch nicht umgesetzt | außerhalb Dienstzeiten keine garantierte menschliche Bewertung | hoch |
| Tenant-Konfiguration noch nicht vollständig als Audit-Evidence exportiert | Ist-Zustand nicht lückenlos nachweisbar | hoch |
| formale Incident-Runbooks fehlen | Reaktion kann personenabhängig sein | hoch |
| ASR/Application-Control-Endzustand noch zu verifizieren | potenziell größere Angriffsfläche | mittel-hoch |
| Logging-Aufbewahrung/SIEM-Strategie noch offen | eingeschränkte Langzeitanalyse/Korrelation | mittel |
| Awareness separat nicht dokumentiert | Schadcode-/Phishing-Risiko bleibt organisatorisch | mittel |

## 8. Änderungs- und Review-Regel

- sicherheitskritische Policy-Änderungen zunächst Pilot/Report-only/Audit, soweit technisch möglich
- jede Ausnahme mit Owner, Begründung und Ablaufdatum
- Policy-Konflikte aktiv überwachen
- Dokument und Clickpaths mindestens halbjährlich sowie nach größeren Microsoft-Portaländerungen prüfen
- BSI-Grundschutz++-Referenzen gegen aktuelle Rolling Publication abgleichen
