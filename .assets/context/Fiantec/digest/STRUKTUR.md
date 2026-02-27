# FiANTEC Context — Ordnerstruktur

**Stand:** 2026-02-27 | **Dateien:** 9'651 | **Grösse:** ~9.7 GB

```
.assets/context/Fiantec/
│
├── digest/                              ── INDEX (12 Dateien, ~29 KB)
│   │
│   ├── STRUKTUR.md                      ← Diese Datei
│   │
│   │  ── Meta ──────────────────────────────────────────────
│   ├── 00_catalog.md                    Einstiegspunkt & Schnellnavigation
│   ├── 00_glossary.md                   160 Abkürzungen, 53 Fachbegriffe
│   ├── 00_decisions.md                  Entscheide, Constraints, Do-Not-Dos
│   │
│   │  ── Pro RAW-Ordner ───────────────────────────────────
│   ├── 01_Handbücher.md                 → RAW/01_Handbücher
│   ├── 02_Change_Management.md          → RAW/02_Change Management
│   ├── 03_Reglemente.md                 → RAW/03_Reglemente ⚠️ Fundament
│   ├── 04_Testmanagement.md             → RAW/04_Testmanagement
│   ├── 05_FiANTEC_v4_Handbuch.md        → RAW/05_FiANTEC_v4_Handbuch
│   ├── 06_test_mit_bildern.md           → RAW/06_test_mit_bildern
│   │
│   │  ── Querschnitt ──────────────────────────────────────
│   ├── 10_Prozesse.md                   Fachprozesse (aus 01_Handbücher)
│   └── 11_Releases.md                   Namenskonventionen & Release-Typen
│
└── RAW/                                 ── ORIGINALDATEIEN (9'651 Dateien, ~9.7 GB)
    │
    ├── 01_Handbücher/                   1'953 Dateien, ~8.6 GB
    │   ├── Fachprozesse & Anleitungen/      675 Dateien
    │   ├── FiANTEC Systemdoku/            1'230 Dateien
    │   ├── Verträge & SLA FiANTEC/           43 Dateien
    │   └── Vorlagen & Templates/              5 Dateien
    │
    ├── 02_Change Management/            963 Dateien, ~252.8 MB
    │   ├── Change Management FTF.pptx         (lose, 3.1 MB)
    │   ├── Spec_Template_New.docx             (lose, 83 KB)
    │   ├── 14.-tgl. & Mev/                   1 Datei
    │   ├── Bugs/                              2 Dateien
    │   ├── Dokumente zu Produktivsetzungen/ 860 Dateien
    │   ├── Features/                         98 Dateien
    │   ├── Incidents/                         — (leer)
    │   └── Reports/                           — (leer)
    │
    ├── 03_Reglemente/                   41 Dateien, ~10.3 MB ⚠️ FUNDAMENT
    │   ├── 2026 02 04_Aussendienst Vergütungsreglemente/  24 Dateien
    │   ├── 2026 02 24_Broker Vergütungsreglemente/        10 Dateien
    │   ├── 2026 02 26_Swiss Life Select Vergütungsreglemente/  5 Dateien
    │   └── 2026 02 26_Vaudoise Vergütungsreglemente/       2 Dateien
    │
    ├── 04_Testmanagement/               6'692 Dateien, ~769.3 MB
    │   ├── Prompt-Testfaelle.docx             (lose, 27 KB)
    │   ├── Anleitungen Testing/              56 Dateien
    │   ├── Azure DevOps/                      6 Dateien
    │   ├── Release-Dokumentation/             1 Datei
    │   ├── SOAP UI/                       3'106 Dateien
    │   ├── SpecFlow/                      3'522 Dateien
    │   └── XMLInterface/                      — (leer)
    │
    ├── 05_FiANTEC_v4_Handbuch/          1 Datei, ~32 MB
    │   └── SLCH-Handbuch20240305.pdf
    │
    └── 06_test_mit_bildern/             1 Datei, dynamisch
        └── FiANTEC Startseite.png             (112 KB)
```

## Legende

| Symbol | Bedeutung |
| --- | --- |
| `→` | Digest verweist auf diesen RAW-Ordner |
| `⚠️` | Kritisch — immer zuerst prüfen |
| `—` | Leer (noch keine Dateien) |

## Nummern-Logik

| Bereich | Zweck |
| --- | --- |
| `00_*` | Meta-Dateien (Katalog, Glossar, Entscheide) |
| `01–06_*` | Je eine Digest-Datei pro RAW-Ordner |
| `07–09` | Reserviert für zukünftige RAW-Ordner |
| `10–11_*` | Querschnittsthemen (ordnerübergreifend) |