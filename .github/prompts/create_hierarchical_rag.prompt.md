---
agent: 'agent'
model: 'Claude Sonnet 4.6'
tools: [vscode, execute, read, agent, edit, search, web, 'memory/*', 'playwright/*', 'sequential-thinking/*', azure-mcp/search, todo]
description: Baut ein neues Hierarchical RAG (DIGEST/RAW) für ein Projekt auf — inkl. Quellen-Download, Digest-Generierung, Freshness Gate und Anonymisierung.
---
parameters:
  - name: projectName
    label: Projekt-/Themenname (wird Ordnername in .assets/context/)
    type: string
    required: true
  - name: sources
    label: "Quellen (eine pro Zeile: Name | URL oder Pfad)"
    type: string
    required: true
  - name: additionalDocs
    label: Zusätzliche lokale Dokumente (Drag & Drop oder Pfade)
    type: string
    required: false
  - name: anonymize
    label: Firmennamen anonymisieren
    type: enum
    options: [ja, nein]
    default: ja
    required: true
  - name: anonymizeTerms
    label: "Zu anonymisierende Begriffe (kommasepariert, z.B. 'Example Corp,SL AG')"
    type: string
    required: false
    default: "Example Corp"
---

Hints (nicht wiederholen):
- Immer zuerst lesen und anwenden:
  - .github/skills/hierarchical-rag/SKILL.md (Kernworkflow, Digest-Standards, Freshness Gate)
  - .github/skills/hierarchical-rag/references/setup-workflow.md (6-Phasen-Setup)
  - .github/skills/hierarchical-rag/references/freshness-gate.md (Quellen-Registry, TTL)
  - .github/instructions/copilot.instructions.md (globale Regeln)
  - .github/instructions/user.copilot.instructions.md (Sprache, Stil, Nutzer-Präferenzen)
- Antwort immer auf Deutsch (Schweizer Variante, kein "ß").
- Kein Gendern. Keine Spekulation.
- Alle temporären Dateien in `.temp/` ablegen.

# Aufgabe

Du baust ein vollständiges Hierarchical RAG für das Projekt **{{projectName}}** auf. 
Folge dem sequential-thinking Workflow und dem hierarchical-rag Skill exakt.

---

# Sequential Thinking Workflow

## Phase 1: Projekt initialisieren

1) Ordnerstruktur anlegen:
```
.assets/context/{{projectName}}/
├── _sources.md
├── digest/
│   ├── 00_catalog.md
│   ├── glossary.md
│   ├── decisions.md
│   ├── 10_topics/
│   └── 20_folders/
└── RAW/
```

2) `.gitignore` aktualisieren — folgendes Pattern hinzufügen (analog zu bestehendem Fiantec-Pattern):
```gitignore
# {{projectName}} Hierarchical RAG
!/.assets/context/{{projectName}}/
!/.assets/context/{{projectName}}/digest/
!/.assets/context/{{projectName}}/digest/**
```
   - NUR `digest/` committen
   - `RAW/` bleibt ignoriert (durch bestehendes `/.assets/**` Pattern)
   - `_sources.md` bleibt ignoriert (durch globale `**/_sources.md` Regel — enthält interne URLs!)

3) Bestätige dem User die erstellte Struktur.

## Phase 2: Quellen verarbeiten

Eingabeformat für {{sources}} — eine Quelle pro Zeile:
```
Name | URL-oder-Pfad
```

Beispiele:
```
FiANTEC Handbuch | https://tenant.sharepoint.com/sites/docs/handbuch
CTRM Wiki | ADO:CTRM/_wiki/wikis/CTRM.wiki/2581
Lokale Specs | C:\Projekte\specs\
Release Notes | https://confluence.example.com/releases
```

Für jede Quelle:
1) **Quelltyp erkennen** (automatisch aus URL/Pfad ableiten):
   - `https://*.sharepoint.com/*` → Quelltyp `SharePoint` (Playwright zum Download)
   - `ADO:*` oder `dev.azure.com` → Quelltyp `ADO Wiki` (MCP ado-Tools)
   - `https://*` → Quelltyp `Web-URL` (fetch_webpage)
   - Lokaler Pfad → Quelltyp `Lokales File` (Dateisystem-Kopie)
   - Kein URL → Quelltyp `Manuell`

2) **RAW-Ordner anlegen** — numerisch präfixed:
   - Erste Quelle → `RAW/01_<Name>/`
   - Zweite Quelle → `RAW/02_<Name>/`
   - usw.

3) **Inhalte herunterladen / kopieren:**
   | Quelltyp | Methode |
   |----------|---------|
   | SharePoint | Playwright: Seite navigieren, Login abwarten, Inhalte extrahieren, Dokumente herunterladen |
   | ADO Wiki | MCP `mcp_ado_wiki_get_page` / `mcp_ado_wiki_list_pages` → Markdown exportieren |
   | Web-URL | `fetch_webpage` → als Markdown speichern |
   | Lokales File | `Copy-Item` in den RAW-Ordner |
   | Manuell | User informieren: "Bitte Dokumente manuell in RAW/<ordner>/ ablegen" |

4) **Binärdateien konvertieren** (wo möglich):
   - PDF → Markdown (via pdf-Skill oder Textextraktion)
   - DOCX → Markdown
   - HTML → Markdown (Grundstruktur beibehalten)
   - Originale behalten, Markdown-Version daneben ablegen

5) **`_sources.md` befüllen** — eine Zeile pro Quelle:
```markdown
| S-001 | RAW/01_<Name>/ | <Quelltyp> | <URL> | <heute> | <heute> | ✅ aktuell |
```

## Phase 3: Digest aufbauen

Folge exakt dem Setup-Workflow aus `references/setup-workflow.md`:

1) **Folder-Digests** (`20_folders/`):
   - Für jeden RAW-Ordner ein `<ordner>.digest.md` erstellen
   - Tabelle mit allen Dateien, Typ, Beschreibung, Schlüsselinfo
   - "Wichtigste Erkenntnisse" als Bullets

2) **Topic-Summaries** (`10_topics/`):
   - Themenübergreifende Zusammenfassungen erkennen und erstellen
   - Mindestens 3 Topics identifizieren
   - Jedes Topic: Übersicht + Schlüsseldokumente + optional FAQ

3) **Glossar** (`glossary.md`):
   - Alle Fachbegriffe und Abkürzungen aus den RAW-Dokumenten extrahieren
   - Nach Kategorie strukturieren

4) **Decisions** (`decisions.md`):
   - Scope-Entscheide dokumentieren (was ist drin, was nicht)
   - Bekannte Lücken aufführen

5) **Catalog** (`00_catalog.md`):
   - Statistiken (Anzahl Dateien, Reduktion)
   - Quick Navigation (Ordner-Tabelle + Topic-Links + Referenz-Links)

## Phase 4: Zusätzliche Dokumente integrieren

Falls {{additionalDocs}} angegeben:
1) User fragen: "In welchen RAW-Ordner sollen die Dokumente? Bestehend oder neuer Ordner?"
2) Dokumente kopieren/verschieben
3) Betroffene Digest-Files aktualisieren
4) `_sources.md` ergänzen (Quelltyp: `Manuell`)

Falls nicht angegeben:
- User fragen: "Möchtest du noch zusätzliche Dokumente hinzufügen? Du kannst Dateien per Drag & Drop anhängen oder Pfade angeben."
- Bei "Nein" → weiter zu Phase 5

## Phase 5: Anonymisierung & Validierung

### Anonymisierung (wenn {{anonymize}} == "ja")

Zu anonymisierende Begriffe: {{anonymizeTerms}}

1) **Alle Digest-Files scannen** (nicht RAW — dort bleiben Originale):
   ```
   digest/00_catalog.md
   digest/glossary.md
   digest/decisions.md
   digest/10_topics/*.md
   digest/20_folders/*.md
   _sources.md
   ```

2) **Für jeden zu anonymisierenden Begriff:**
   - Exakte Treffer ersetzen (case-insensitive)
   - Zusammengesetzte Formen erkennen (z.B. "Example Corp AG", "Example Corp Ltd", "Example Corp", "SL AG")
   - Ersetzen durch neutralen Platzhalter: `[Unternehmen]` oder `[Organisation]`
   - Bei Personen: `[Mitarbeiter]`, `[Kontakt]`
   - Bei internen URLs: Domain durch `[intern]` ersetzen

3) **Validierungslauf:**
   - Jeden Digest-File nach ALLEN Begriffen aus {{anonymizeTerms}} durchsuchen
   - Auch Teiltreffer prüfen (z.B. "Swiss" allein, wenn "Example Corp" anonymisiert werden soll)
   - Ergebnis als Tabelle ausgeben:

   ```
   | Datei | Treffer | Status |
   |-------|---------|--------|
   | 00_catalog.md | 0 | ✅ Sauber |
   | 10_topics/Prozesse.md | 0 | ✅ Sauber |
   ```

4) **Falls Treffer gefunden**: sofort anonymisieren und erneut validieren.

5) **Abschlussmeldung**: "Alle Digest-Files wurden auf die Begriffe [{{anonymizeTerms}}] geprüft. Ergebnis: X Files sauber, Y Files anonymisiert."

### Allgemeine Validierung

6) **Digest-Qualität prüfen:**
   - Jeder Folder-Digest hat eine Tabelle mit allen RAW-Dateien? ✅/❌
   - Jedes Topic hat eine Übersicht + Schlüsseldokumente? ✅/❌
   - Glossar enthält mindestens 10 Begriffe? ✅/❌
   - Catalog hat Statistiken und Navigation? ✅/❌
   - `_sources.md` hat alle Quellen mit TTL? ✅/❌

7) **Token-Effizienz berechnen:**
   - Gesamtgrösse RAW/ (Markdown-Files)
   - Gesamtgrösse digest/
   - Reduktion in %
   - Ziel: ≥ 60%

---

# Ausgabeformat

Am Ende des Workflows folgende Zusammenfassung ausgeben:

```markdown
## Hierarchical RAG — {{projectName}} ✅

### Struktur
| Schicht | Dateien | Grösse |
|---------|---------|--------|
| RAW | X Dateien | ~Y KB |
| Digest | Z Dateien | ~W KB |
| **Reduktion** | | **X%** |

### Quellen-Registry (_sources.md)
| ID | Quelle | Quelltyp | Status |
|----|--------|----------|--------|
| S-001 | ... | ... | ✅ |

### Anonymisierung
| Geprüfte Begriffe | Ersetzte Vorkommen | Status |
|--------------------|--------------------|--------|
| {{anonymizeTerms}} | X | ✅ Sauber |

### Nächste Schritte
1. `digest/` in Git committen
2. RAW-Ordner lokal behalten (nicht committen)
3. Bei Bedarf: Freshness-Check konfigurieren (TTLs anpassen in `_sources.md`)
4. Projekt-Instructions um Context-First-Workflow ergänzen
```

---

# Sicherheitsregeln

- **Niemals** Passwörter, Tokens oder Credentials in `_sources.md` oder Digest-Files speichern
- **Niemals** RAW-Inhalte mit sensiblen Daten in den Digest übernehmen ohne Anonymisierung
- Bei SharePoint/ADO-Zugriff: Nur öffentlich zugängliche oder für den User autorisierte Inhalte
- Alle Downloads in `.temp/` zwischenspeichern, dann nach `RAW/` verschieben
- `.temp/` nach Abschluss aufräumen

---

# Fehlerbehandlung

| Fehler | Aktion |
|--------|--------|
| Quelle nicht erreichbar | Status `❌ nicht erreichbar` in `_sources.md`, User informieren, weiter mit nächster Quelle |
| Login erforderlich (SharePoint) | Playwright Login-Flow starten, bei Misserfolg → User informieren |
| Binärdatei nicht konvertierbar | Original behalten, in Digest als "Binärdatei (nicht konvertiert)" markieren |
| Weniger als 3 Topics identifizierbar | User fragen: "Ich habe nur X Topics gefunden. Soll ich weniger erstellen oder kannst du Themen vorschlagen?" |
| Anonymisierung findet versteckte Treffer | Kontext anzeigen, User fragen ob anonymisieren oder belassen |
