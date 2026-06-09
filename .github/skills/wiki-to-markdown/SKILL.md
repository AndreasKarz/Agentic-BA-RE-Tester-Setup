---
name: wiki-to-markdown
description: >
  Konvertiert bildlastige ADO-Wiki-Seiten (z. B. CTRM Manual) Seite für Seite in saubere
  Markdown-Dateien. Echte UI-Screenshots bleiben als Bilder mit reichem Alt-Text und
  "Screenshot-Erläuterung". PowerPoint- und Flow-Diagramme werden zu Mermaid umgewandelt.
  Goldstandard: wiki-md/CTRM/ctrm-feature-process.md. Triggers: wiki to markdown,
  ADO Wiki Seite umwandeln, wiki-seite konvertieren, ctrm manual markdown,
  convert wiki page, wiki page zu markdown, convert_wiki_page.
---

# Wiki → Markdown Skill

Konvertiert eine ADO-Wiki-Seite in eine strukturierte Markdown-Datei nach dem
Goldstandard [`wiki-md/CTRM/ctrm-feature-process.md`](../../../wiki-md/CTRM/ctrm-feature-process.md).

---

## Grafik-Klassifikation (Pflicht für jedes Bild)

Jedes Bild via `view_image` ansehen — dann nach dieser Tabelle entscheiden:

| Typ | Erkennung | Behandlung |
|-----|-----------|-----------|
| **UI-Screenshot** | Echtes ADO-Formular, Felder, Buttons, Status-Dropdown | **Bild behalten** + Alt-Text + `**Screenshot-Erläuterung:**` + Aktivitäten-Tabelle |
| **Annotierter Screenshot** | UI-Screenshot mit Callout-Nummern ①②③ eingebettet | **Bild behalten** + JEDE Annotation ①..⑭ → Feld+Aktion im Alt-Text + Tabelle |
| **State-Diagramm / einfacher Flow** | Einzelne Linie von Zustand zu Zustand, KEIN Swimlane | Als **Mermaid `stateDiagram-v2`** oder **`graph LR`** (ohne subgraph-Links) |
| **Swimlane-Diagramm** | Mehrere parallele Spuren nebeneinander (Rollen/Teams), mit Querverbindungen | Als **HTML-Tabelle** mit `<td>`, `rowspan`, inline CSS — **KEIN Mermaid** |
| **Tabellen-Grafik** | Tabelleninhalt als Bild (z. B. aus PowerPoint-Export) | Als **Markdown-Tabelle rekonstruieren** — kein Bild behalten |
| **Informationsgrafik** | Icons, Konzept-Überblick, rein symbolisch | Als **Text/Liste rekonstruieren** — kein Bild behalten |

> **Warum kein Mermaid für Swimlanes?** ADO Wiki unterstützt offiziell **keine Links to/from `subgraph`**
> und keinen `flowchart`-Typ. `graph LR` mit `subgraph` rendert korrumpiert (gestapelt statt horizontal).
> HTML-Tabellen mit `rowspan`/`colspan` + inline CSS sind die einzige zuverlässige Darstellung für
> Swimlane-Diagramme in ADO Wiki.

### Mermaid-Orientierungsregel (Pflicht)

**Mermaid-Diagramme müssen möglichst die Orientierung des Originals spiegeln:**

| Original-Diagramm | Mermaid-Richtung | Syntax |
|-------------------|-----------------|--------|
| Horizontal (links → rechts, Prozessfluss) | **Landscape** `LR` | `stateDiagram-v2` mit `direction LR` oder `graph LR` |
| Vertikal (oben → unten, Hierarchie) | Portrait `TD` | `graph TD` oder `stateDiagram-v2` (default) |
| Unklar / keine Vorlage | **Landscape** `LR` | Standard-Fallback: immer `LR` wählen |

> **Regel:** Im Zweifel immer **Landscape (`LR`)** — Prozessdiagramme lesen sich horizontal
> natürlicher und nutzen die Seitenbreite im ADO Wiki besser.

**Für `stateDiagram-v2` Landscape:**
```
::: mermaid
stateDiagram-v2
    direction LR
    [*] --> Zustand1 : Trigger
    Zustand1 --> Zustand2 : Bedingung
:::
```

**Für `graph` Landscape:**
```
::: mermaid
graph LR
    A[Schritt 1] --> B[Schritt 2] --> C[Schritt 3]
:::
```

### Swimlane → HTML-Tabelle (Muster)

Für Swimlane-Diagramme mit horizontalen Prozess-Spuren und einer vertikalen CAB/Gate-Spalte:

```html
<table style="border-collapse:collapse;font-family:Segoe UI,Arial,sans-serif;font-size:12px;width:100%;">
  <tr>
    <td style="width:150px;font-weight:bold;padding:6px 10px;">Spur 1</td>
    <td style="background:#7b2d8b;color:white;padding:6px 10px;">Schritt A</td>
    <td style="background:#7b2d8b;color:white;padding:6px 10px;">Schritt B</td>
    <td style="background:#2ecc71;color:white;padding:6px 10px;writing-mode:vertical-lr;
               text-align:center;font-size:11px;" rowspan="N">Gate / CAB</td>
    <td style="background:#7b2d8b;color:white;padding:6px 10px;">Schritt C</td>
  </tr>
  <tr>
    <td style="font-weight:bold;padding:6px 10px;">Spur 2</td>
    <td colspan="2" style="background:#999;color:white;padding:6px 10px;">Schritt 1-2</td>
    <td style="background:#999;color:white;padding:6px 10px;">Schritt 3</td>
  </tr>
</table>
```

Regeln für ADO-Wiki-HTML-Tabellen:
- `border-collapse:collapse` ist Pflicht (sonst Doppelrahmen)
- `writing-mode:vertical-lr` für rotierten Text in `rowspan`-Spalten
- Farben als Hex-Werte oder benannte Farben (kein `var()`, kein CSS-Grid)
- Kein `<style>`-Block — nur inline `style="..."`

---

## 6-Phasen-Workflow

### Phase 0 — Setup

1. ADO-Wiki-URL parsen → `org / project / wiki / pageId / slug` extrahieren.
2. `slug` = kebab-case-lowercase aus Seitentitel (`CTRM-PBI-Process` → `ctrm-pbi-process`).
3. Ausgabepfade definieren:
   - Markdown: `.temp/wiki-converted/<slug>.md`
   - Assets: `.temp/wiki-converted/assets/<slug>/NN-name.png`
   - Download-Temp: `.temp/wiki-import/<slug>/`
4. Ordner anlegen: `.temp/wiki-converted/assets/<slug>/` und `.temp/wiki-import/<slug>/`.

> **Hinweis:** `.temp/` ist gitignoriert — generierte Artefakte tauchen nicht im Repository auf.
> Die Dateien sind für Review und als Quelle für das ADO-Wiki-Upload bestimmt.

### Phase 1 — Seite laden

1. `ado` MCP → `wiki_get_page` (org, project, wikiId, pageId) → Roh-Markdown + Metadaten.
2. Alle `/.attachments/Filename-GUID.ext`-Referenzen im Roh-Markdown sammeln.
3. Seitenstruktur (H1, H2, H3) und Bild-Positionen kartieren.

> Technische Details zu ADO-Toolaufruf und Attachment-URLs: **[extraction.md](references/extraction.md)**

### Phase 2 — Bilder laden

1. Jeden Attachment-Dateinamen nach `.temp/wiki-import/<slug>/` laden.
2. **Smoke-Test** mit erstem Bild: Datei >0 Bytes? → ja: Batch fortsetzen; nein: Playwright-Fallback.
3. Geladene Bilder nach `.temp/wiki-converted/assets/<slug>/NN-name.png` kopieren.

> Auth-Strategie (az rest + Access Token) und Playwright-SAML-Fallback: **[extraction.md](references/extraction.md)**

### Phase 3 — Klassifizieren und verstehen

1. Jedes Bild via `view_image` ansehen und nach obiger Tabelle klassifizieren.
2. ADO-Konzepte (Work-Item-Form, PBI-Workflow-States, Area Path, Iteration Path, DoR, DoD)
   via `microsoft-docs` MCP grounden — für fachlich präzise Alt-Texte.
3. Pro UI-Screenshot: Alt-Text-Gerüst (5 Punkte, siehe unten) vorläufig notieren.
4. Pro Diagramm: Knoten + Kanten + Labels für Mermaid erfassen.

### Phase 4 — Markdown schreiben

Exakte Struktur nach [output-template.md](references/output-template.md). Kurzübersicht:

- **Sprache:** Deutsch (Schweiz), kein ß, kein Gendern, Datum DD.MM.YYYY
- **H1:** Seitentitel
- **Meta-Blockquote** nach H1: Accountable-Zeile
- **Intro-Absatz:** Zweck (1–3 Sätze)
- **Überblick-Diagramm:** Prozessdiagramm → `Mermaid stateDiagram-v2` (mit `direction LR`) oder `graph LR` — **immer Landscape, ausser das Original ist explizit vertikal**
- **Pro Sektion:**
  1. `## Phase N — Titel` (ggf. Meta-Blockquote wiederholen)
  2. `![Alt-Text](assets/<slug>/NN-name.png)` — lokaler Pfad relativ zur `.md`-Datei
  3. `**Screenshot-Erläuterung:**` Legende
  4. Aktivitäten-Tabelle `| Nr. | Pflicht | Aktivität |`
  5. Hinweis-Blockquotes

> Für das **ADO-Wiki-Upload** (Phase 4b) werden Bildpfade zu `/.attachments/<GUID>.png` umgeschrieben.

### Phase 4b — ADO-Wiki-Upload (IMMER automatisch, ohne Rückfrage)

**Ziel-Parent:** `/CTRM Manual migrated` (pageId 28309, org swisslife, project CTRM, wikiId CTRM.wiki)

1. ADO-Markdown aus `.temp/wiki-converted/<slug>.md` herstellen:
   - ` ```mermaid ` ... ` ``` ` → `::: mermaid` ... `:::`
   - Lokale Bildpfade `assets/<slug>/NN-name.png` → `/.attachments/<originalGUID>.png`
     (Mapping: NN-name.png → GUID aus Phase 1 Attachment-Liste)
   - `\n`-Escapes in Mermaid-Node-Labels vereinfachen (ADO verarbeitet keine `\n` innerhalb von Labels)
   - Mermaid-Richtung prüfen: `direction LR` bei `stateDiagram-v2`, `graph LR` bei Flowcharts — ausser das Original ist explizit vertikal
2. Seiten-Pfad ableiten: `/CTRM Manual migrated/<Originaltitel der Wiki-Seite>`
   - Originaltitel = exakter Seitentitel aus dem ADO-Wiki (nicht URL-slug)
3. `mcp_azure_devops__wiki_create_or_update_page` aufrufen:
   - `wikiIdentifier`: `CTRM.wiki`
   - `project`: `CTRM`
   - `path`: `/CTRM Manual migrated/<Originaltitel>`
   - `content`: ADO-Markdown
4. Ergebnis-ID und URL im Report ausgeben.

### Phase 5 — Validieren und Cleanup

| Check | Kriterium |
|-------|-----------|
| Asset-Referenzen | Jedes `![…](assets/…)` zeigt auf existierende Datei in `.temp/wiki-converted/assets/<slug>/` |
| Alt-Text | Jedes Bild: Alt-Text ≥3 Sätze + `**Screenshot-Erläuterung:**` |
| Kein Bild für Diagramme | Kein Prozessdiagramm mehr als PNG vorhanden |
| Sprache | Kein ß, kein Gendern |
| Download-Temp | `.temp/wiki-import/<slug>/` nach erfolgreichem Copy leeren |
| Report | Kurze Zusammenfassung: Bilder behalten / umgewandelt / gelöscht; Pfad zu `.temp/wiki-converted/<slug>.md` |

---

## Alt-Text-Standard

**In `![Alt-Text]` — für AI und Accessibility (3–6 Sätze):**

Schema (5 Punkte, alle relevant abarbeiten):
1. Welcher Screen/Tool und aktueller Status (z. B. *ADO Feature-Formular, Status «1 Funneling»*)
2. Räumliches Layout: was ist links/rechts/oben/unten sichtbar
3. Jede Annotation ①②③ → konkreter Feldname + Aktion (*① Titel eingeben, ② Self-assign*)
4. Sichtbare Seiten-Sektionen rechts/aussen namentlich aufführen
5. Besondere UI-Elemente: hervorgehobene Box, Rahmenfarbe, Dropdown-Optionen, Checklisten

**`**Screenshot-Erläuterung:**` — für Menschen (kompakt, unter dem Bild):**

Kompakte Legende: *Annotiert: ① Status setzen, ② Assigned to, ③ Save.*
Nur wirklich relevante Annotationen; max. 2–3 Sätze.

> **Beide Elemente sind Pflicht** für jeden UI-Screenshot.

---

## Fehlerbehandlung

| Problem | Massnahme |
|---------|-----------|
| `wiki_get_page` → leer | pageId + path kombinieren; Playwright-Fallback |
| Attachment-Download 401/403 | `az account get-access-token` neu holen (Resource-ID in extraction.md) |
| Bild nicht klassifizierbar | Als UI-Screenshot behandeln (sicherer Fallback) |
| Mermaid-Syntax-Fehler | `stateDiagram-v2` gegen `flowchart LR` tauschen; Node-Labels vereinfachen |
| Wiki-Seite hat keine klare Phasen-Struktur | H2/H3-Gliederung aus dem Roh-Markdown übernehmen; Aktivitäten-Tabellen ggf. weglassen |
