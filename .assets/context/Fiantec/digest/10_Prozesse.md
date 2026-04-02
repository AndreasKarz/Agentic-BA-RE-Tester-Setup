# Fachprozesse

**Source:** `RAW/01_Handbücher/FiANTEC Systemdoku/00 Architektur_und_Prozesse/` + `Fachprozesse & Anleitungen/`  
**→ Digest:** [01 Handbücher](01_Handb%C3%BCcher.md) | **Prozessarchitektur:** `Prozessarchitektur FiANTEC_v01.04.xlsx`

## Prozesslandschaft FiANTEC v4

Die FiANTEC-Prozesse sind in vier Kategorien gegliedert. Jede Kategorie hat einen eigenen Ordner im RAW unter `00 Architektur_und_Prozesse/`:

### K — Konfigurationsprozesse (Systemaufbau)

| ID | Prozess | RAW-Ordner | Beschreibung |
| --- | --- | --- | --- |
| K\_01 | Mandant/Absatzkanal einrichten | `010_K_01 Mandant_Absatzkanal einrichten/` | Ersteinrichtung von Mandanten und Absatzkanälen |
| K\_02 | Fabriken/Produktbereiche einrichten | `020_K_02_Fabriken_Produktbereiche_einrichten/` | Factories und Produktbereiche konfigurieren |
| K\_03 | Provisionskonfiguration einrichten | `030_K_03_Provisionskonfiguration einrichten/` | Provisionsarten, Staffeln, Vergütungsregeln |

### SV — Setup Vertriebsorganisation

| ID | Prozess | RAW-Ordner | Beschreibung |
| --- | --- | --- | --- |
| SV\_01 | Vertriebsorganisation einrichten | `040_SV_01_Vertriebsorganisation einrichten/` | VOE, Aufbauorga, Gebiete, VMA initialisieren |

### OP — Operative Prozesse (Tagesgeschäft)

| ID | Prozess | RAW-Ordner | Beschreibung |
| --- | --- | --- | --- |
| OP\_01 | Erstbetreuung anlegen | `050_OP_01_Erstbetreuung_anlegen/` | Zuweisung eines Vertrags/Kunden an eine Vertriebseinheit |
| OP\_02 | Leistungsträgersatz anlegen/ändern | `060_OP_02_Leistungsträgersatz anlegen_ändern/` | Definition und Mutation von Leistungsträgern |
| OP\_03 | Mutation anlegen | `070_OP_03_Mutation anlegen/` | Änderungen an Vertrags- und Stammdaten |
| OP\_04 | Provision berechnen | `080_OP_04_Provision berechnen/` | Kernprozess: Provisionsberechnung auf Basis von GeVo/FE/FFE |
| OP\_05 | Tagesendverarbeitung (TEV) | `090_OP_05_Tagesendverarbeitung/` | Nightly Batch: Provisionsberechnungen abschliessen |
| OP\_06 | Spezialprozesse periodisch | `100_OP_06_Spezialprozesse_periodisch/` | Periodische Jobs (z.B. JEVE, RAD, EGR) |
| OP\_07 | Monatsendverarbeitung (MEV) | `110_OP_07_Monatsendverarbeitung_durchführen/` | Monatlicher Batch: Abrechnungen, SAP-Export, Berichte |
| OP\_08 | Betreuungen umteilen | `120_OP_08_Betreuungen_Umteilen/` | Portfoliomigration, Gebietsänderungen |

### SP — Stammdatenpflege

| ID | Prozess | RAW-Ordner | Beschreibung |
| --- | --- | --- | --- |
| SP\_01 | Vertriebsorganisation pflegen | `130_SP_01_Vertriebsorganisation pflegen/` | Laufende Anpassungen an VOE, VMA, Gebieten |
| SP\_02 | Mandant/Absatzkanal pflegen | `140_SP_02_Mandant_Absatzkanal_pflegen/` | Änderungen an Mandantenkonfiguration |
| SP\_03 | Provisionskonfiguration pflegen | `150_SP_03_Provisionskonfiguration pflegen/` | Anpassungen Vergütungsregeln, Staffeln, Prov.art-Konfig |
| SP\_04 | OREC (Operational Risk) | `160_SP_04_Operational Risk Event Collection (OREC)/` | Risikomanagement und IKS-Prozesse |

> **Alle RAW-Pfade:** Relativ zu `RAW/01_Handbücher/FiANTEC Systemdoku/00 Architektur_und_Prozesse/`

## Kernprozessbereiche (Querschnitt)

### Provisionsverarbeitung

*   **Provisionsberechnung:** OP\_04 → Kernprozess, ausgelöst durch GeVo-Import, Fabrikereignisse (FE) oder Fremdfabrikereignisse (FFE)
*   **Batch-Verarbeitung:** OP\_05 (TEV, täglich), OP\_07 (MEV, monatlich), OP\_06 (JEVE, jährlich)
*   **Provisionsabstimmung:** Ergebnis der MEV, Export nach SAP-FSCD/SAP-FI
*   **Detaillierte Businesslogik:** `FIA 03 BusinessLogik_v01.45.docx` in `03 Business Logik und Vertriebsorganisation/`

### Vertragsmanagement

*   Policen-Erstellung und -Änderung (OP\_03)
*   Vertragsbetreuung und Portfoliozuweisung (OP\_01, OP\_08)
*   Portfolio-Verwaltung (SP\_01)

### Datenverarbeitung

*   GeVo-Import, SEPA-Export, SAP FI/HCM-Export
*   Batch-Jobs: TEV (OP\_05), MEV (OP\_07), JEVE (OP\_06)
*   Schnittstellenmanagement → siehe [01 Handbücher](01_Handb%C3%BCcher.md) → Schnittstellen-Navigation

### Reporting

*   Standardreports und Crystal Reports
*   Recherchen: Listen-, System-, Anwenderrecherchen
*   Recherchedesigner (Lizenzerweiterung, siehe FiANTEC v4 Handbuch)

## Prozessmodellierung (Tools)

| Tool | Verwendung | RAW-Pfad |
| --- | --- | --- |
| **ADONIS** | Offizielle Prozessmodellierung Swiss Life | `Fachprozesse & Anleitungen/ADONIS, Camunda und BPMN/` |
| **Camunda Modeler** | BPMN 2.0 Modellierung mit Token-Simulation | `Fachprozesse & Anleitungen/ADONIS, Camunda und BPMN/camunda modeler mit Tokensimulator/` |

> **Schulungsmaterial:** ADONIS Schulung für Modellierer, BPMN 2.0 Poster, Prozess-Steckbrief Vorlage (PPTX)  
> Weitere prozessrelevante Dokumente: [01 Handbücher](01_Handb%C3%BCcher.md) → Abschnitt 03 Business Logik