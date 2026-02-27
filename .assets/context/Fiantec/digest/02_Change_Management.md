# 02 Change Management

**RAW:** `RAW/02_Change Management` | **Files:** 963 | **Size:** ~252.8 MB | **Letzte Aktualisierung:** 2026-02-27

**Stichwörter:** Release, Deployment, Produktivsetzung, GoLive, Change Request, CR, Zertifizierung, Release Bulletin, Zertifikat, ProvFia, DMV, Datenmigration, Canossa, Batchjobs, MEV, Datenbank, BI Reporting, Feature-Spezifikation, Impact-Analyse, Checkliste, Post-GoLive

## Wann hier suchen?

- **Release-Dokumentation** — Bulletins, Zertifikate, Checklisten pro Produktivsetzung
- **Change Requests** — Feature-Spezifikationen, Impact-Analysen
- **Historische Releases** — Was wurde wann deployt, welche Artefakte gehören dazu
- **Fachbereichs-Releases** — ProvFia, DMV, Canossa, Batchjobs, Schnittstellen, Datenbank, BI

## Prozessablauf

1. **CR erstellt** → Change Request im ADO erfasst
2. **Spezifikation** → Requirements in PBI/Feature dokumentiert
3. **Entwicklung** → FiANTEC DE implementiert
4. **Zertifizierung** → Test auf Teststufen (ST → UAT → PAV)
5. **Produktivsetzung** → Release-Paket wird deployt

## Artefakte pro Phase

| Phase | Artefakte | RAW-Bereich |
|-------|-----------|-------------|
| CR | Change Request, Impact-Analyse | Features/ |
| Spezifikation | PBI, Akzeptanzkriterien | ADO (nicht im RAW) |
| Entwicklung | Release Bulletin, Zertifikat | Produktivsetzungen/ |
| Test | Testfälle, Protokolle | → [04 Testmanagement](04_Testmanagement.md) |
| GoLive | Checkliste, Post-GoLive-Report | Produktivsetzungen/ |

## Dateitypen

| Extension | Anzahl | Beschreibung |
|-----------|--------|--------------|
| .pdf | 292 | PDFs (Zertifikate, Bulletins) |
| .docx | 279 | Word-Dokumente |
| .doc | 145 | Ältere Word-Dateien |
| .xlsx | 80 | Excel-Dateien (Release Bulletins) |
| .xls | 66 | Ältere Excel-Dateien |
| .csv | 28 | CSV-Dateien |
| .txt | 23 | Text-Dateien |
| .pptx | 9 | Präsentationen |
| .png | 8 | Bilddateien |
| .ppt | 8 | Ältere Präsentationen |

## Unterordner

2 lose Dateien auf Ordnerebene:

*   **Change Management FTF.pptx** (3.1 MB) - Change-Management-Prozess CH/FTF
*   **Spec\_Template\_New.docx** (83 KB) - Spezifikationsvorlage (READ-ONLY Referenz)

### Produktivsetzungen (860 Dateien, 218 MB)

Release-Dokumentation gegliedert nach Phasen und Jahrgängen (2011-2026):

| Ordner | Dateien | Beschreibung |
|--------|---------|--------|
| 88 UAT\_Zertifikate | 312 | UAT-Zertifikate nach Jahr |
| 89 PAV\_Zertifikate | 309 | PAV-Zertifikate nach Jahr |
| 90 Release Bulletin | 100 | Release Bulletins |
| 91 Testberichte | 27 | Testberichte |
| Aufgaben Produktivsetzung | 6 | Deployment-Aufgaben |
| Deployment Matrix | 1 | Deployment-Zuordnung |
| Installierte Versionen | 1 | Versionsübersicht |
| \_Archive | 103 | Archivierte Release-Dokumente |

### Features (98 Dateien, 31 MB)

Feature-Spezifikationen, Impact-Analysen, Change Requests.

### Bugs (2 Dateien)

Bug-Dokumentation (historisch, die meisten Bugs sind im ADO).

### 14-tgl. & Mev (1 Datei)

Zeitplan für 14-tägliche Releases und Monatsendverarbeitung.

### Incidents & Reports (leer)

Historischer Ordner, Incidents werden im ADO geführt.

## Siehe auch

- [11 Releases](11_Releases.md) — Namenskonventionen und Release-Typen
