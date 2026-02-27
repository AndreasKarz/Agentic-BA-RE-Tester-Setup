# FiANTEC Decisions & Constraints

**Updated:** 2026-02-26

## Entscheidungen

### Dokumentenformat

- **Originaldateien behalten** — keine automatische Konvertierung nach Markdown
- RAW enthält die Originaldateien (PDF, DOCX, XLSX, PPTX, VSD, XLS etc.)
- Zugriff auf Inhalte erfolgt über ARE-Skills (pdf, docx, xlsx, pptx)

### Digest-Strategie

- Digest ist ein **Index** — zeigt WO relevante Infos im RAW liegen
- Digest reicht NIE als alleinige Quelle — immer im RAW nachschlagen
- Flache Struktur: eine Datei pro RAW-Ordner (Fachwissen + Dateistruktur kombiniert)

### Ordnerstruktur

- Originale SharePoint-Hierarchie wird beibehalten
- 6 Hauptordner: 01\_Handbücher, 02\_Change Management, 03\_Reglemente, 04\_Testmanagement, 05\_FiANTEC\_v4\_Handbuch, 06\_test\_mit\_bildern

### Glossar

- Angereichert aus XLSX-Quelle (160 Abkürzungen, 53 Fachbegriffe)
- Kategorisiert nach Themengebieten

## Constraints

- Digest-Dateien sind git-tracked — nicht random löschen, sauber überarbeiten
- Reglemente (03\_Reglemente) sind das **Fundament** — immer zuerst prüfen
- 06\_test\_mit\_bildern wird **schrittweise** mit FiANTEC-GUI-Screenshots befüllt — dynamischer Lernprozess für die KI
- Nur FiANTEC-Perspektive — nur systemspezifisch, nicht konzernweit
- **Keine Firmennamen im Digest** — weder Auftraggeber noch Partner namentlich erwähnen, ausser in technischer Systemdokumentation (Schnittstellen, Systemgrenzen), wo Systemnamen zur eindeutigen Identifikation nötig sind (Dateinamen aus RAW ausgenommen)
- **Keine Redundanzen** — jede Information hat genau einen Ort im Digest
- Quellenangaben nur im Chat, **nie** in Artefakten (Feature Descriptions, Acceptance Criteria)

## Sync

- SharePoint → Lokal via `scripts/sync-raw.ps1` (Manifest-basierte Änderungserkennung)
- Sync aktiv seit 18.02.2026 — inkl. Rename-Detection und automatischem Digest-Update (Zähler, Grössen, Datum)

## Do Not Do

- Keine automatische Konvertierung von Originaldateien
- Keine Annahmen ohne Quellenbeleg aus RAW-Dokumenten
- Keine RAW-Änderungen ohne explizite Anweisung
- Keine ADO-Änderungen ohne explizite Bestätigung

## Do Not Assume

- **Kein SAFe / Sprints / PIs** - CH/FTF arbeitet release-basiert (6 Major Releases/Jahr), nicht in SAFe-Zyklen. Keine Sprint-Planung, kein Program Increment
- **Keine Minor Releases** - FiANTEC kennt nur Major Releases und Patches. Keine dritte Kategorie erfinden
- **Keine generischen "häufige Fallen"** - Stolpersteine variieren von BA zu BA, keine pauschalen Fehlerlisten erstellen. Nur dokumentierte, konkret vom User bestätigte Erkenntnisse festhalten
- **Kein Bug-Template nötig** - Bug-Handling läuft über ADO Built-in-Felder + `/analyze_bug` Prompt. Kein separates Template in den Project Instructions
