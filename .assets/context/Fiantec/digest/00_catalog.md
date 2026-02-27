# FiANTEC Context Catalog

**Last Updated:** 2026-02-27  
**Source:** `.assets/context/Fiantec/RAW`  
**Ansatz:** Originaldateien (keine Konvertierung zu Markdown)

## Zusammenfassung

| Metrik | Wert |
| --- | --- |
| **Originaldateien** | 3'023 |
| **Gesamtgrösse** | ~9.7 GB |
| **RAW-Ordner** | 6 |
| **Digest-Dateien** | 11 (+1 Meta-File: STRUKTUR.md) |

## Schnellnavigation

### Nach Ordner (RAW-Spiegel)

| Ordner | Beschreibung | Dateien | Grösse | Digest |
| --- | --- | --- | --- | --- |
| 01\_Handbücher | Handbücher, Systemdoku, Anleitungen | 1'953 | 8.6 GB | [→](01_Handb%C3%BCcher.md) |
| 02\_Change Management | Releases, Zertifikate, CRs | 963 | 252.8 MB | [→](02_Change_Management.md) |
| 03\_Reglemente | Vergütungsreglemente (KRITISCH) | 41 | 10.3 MB | [→](03_Reglemente.md) |
| 04\_Testmanagement | Testdokumentation & Testcode | 64 | 32.8 MB | [→](04_Testmanagement.md) |
| 05\_FiANTEC\_v4\_Handbuch | Offizielles Benutzerhandbuch (PDF) | 1 | 31.5 MB | [→](05_FiANTEC_v4_Handbuch.md) |
| 06\_test\_mit\_bildern | FiANTEC GUI-Screenshots (laufend befüllt) | 1+ | dynamisch | [→](06_test_mit_bildern.md) |

### Querschnittsthemen

*   [10 Prozesse](10_Prozesse.md) — Fachprozesse quer über alle Bereiche
*   [11 Releases](11_Releases.md) — Namenskonventionen und Release-Typen

### Referenz

*   [Glossar](00_glossary.md) — 160 Abkürzungen, 53 Fachbegriffe
*   [Entscheidungen](00_decisions.md) — Dokumentierte Entscheide und Constraints

## Dateiformate im RAW

| Format | Anzahl | Vorkommen |
| --- | --- | --- |
| java | 2'036 | 04_Testmanagement (SOAP UI) |
| feature | 1'025 | 04_Testmanagement (SpecFlow) |
| cs | 795 | 04_Testmanagement (SpecFlow) |
| dll | 711 | 04_Testmanagement (SpecFlow) |
| docx | 601 | 01_Handbücher, 02_Change Management |
| xlsx | 564 | 01_Handbücher, 02_Change Management |
| xls | 463 | 01_Handbücher |
| pdf | 403 | Alle Ordner |
| doc | 320 | 01_Handbücher, 02_Change Management |
| vsd | 178 | 01_Handbücher (Visio-Diagramme) |

> **Hinweis:** Code-Artefakte (java, feature, cs, dll) machen ~50% aller 9'651 Dateien aus (davon 6'628 in SOAP UI/SpecFlow), sind aber für die ARE-Arbeit weniger relevant. Die Metrik **Originaldateien** (3'023) in der Zusammenfassung zählt deshalb nur business-relevante Dokumente. Der vollständige Verzeichnisbaum steht in [STRUKTUR.md](STRUKTUR.md).

## Häufige Aufgaben → Relevante Quellen

| Aufgabe | Digest-Start | Primäre RAW-Quellen |
| --- | --- | --- |
| Provisionsberechnung verstehen | `03_Reglemente` → `01_Handbücher` | Vergütungsreglemente (AD/IB), V4 Handbuch |
| Neue Schnittstelle spezifizieren | `01_Handbücher` → FIA040 | WSDL/XSD in `RAW/01_Handbücher/.../04 Schnittstellen/` |
| Testfall erstellen | `04_Testmanagement` → `03_Reglemente` | Testanleitungen, SpecFlow-Beispiele, Reglemente als Basis |
| Feature/PBI schreiben | `10_Prozesse` → `03_Reglemente` → `01_Handbücher` | Prozessbeschrieb, Reglement, Systemdoku |
| Bug analysieren | `01_Handbücher` → `04_Testmanagement` | Systemdoku, bestehende Testfälle, ADO Work Items |
| Release-Kontext prüfen | `11_Releases` → `02_Change_Management` | CR-Dokumente, Release Notes, Zertifikate |
| Vertriebsorganisation klären | `01_Handbücher` → `00_glossary` | VORP-Doku, Aufbauorga-Handbuch |
| Konfiguration/Referenzdaten | `01_Handbücher` → `10_Prozesse` | Konfigurationshandbuch, Codetabellen-Doku |

## Zugriff auf Originaldateien

| Format | Skill | Tool |
| --- | --- | --- |
| PDF | pdf | pdfplumber |
| DOCX | docx | unpack.py (XML-basiert) |
| XLSX | xlsx | pandas + openpyxl |
| PPTX | pptx | markitdown |

> Für .doc, .xls, .vsd und andere Legacy-Formate steht kein direkter Skill bereit.