---
agent: 'agent'
model: 'Claude Sonnet 4.6'
tools: [vscode, execute, read, agent, edit, search, web, 'memory/*', 'playwright/*', 'sequential-thinking/*', 'ado/*', 'microsoft-docs/*', todo]
description: >
  Konvertiert eine ADO-Wiki-Seite Seite für Seite in eine saubere Markdown-Datei nach CTRM-Goldstandard.
  Echte UI-Screenshots bleiben als Bilder mit Alt-Text und Screenshot-Erläuterung.
  PowerPoint/Flow-Diagramme werden zu Mermaid. Output: wiki-md/{space}/{slug}.md + Assets.
---
parameters:
  - name: wikiPageUrl
    label: "ADO-Wiki-Seiten-URL (z. B. https://dev.azure.com/swisslife/CTRM/_wiki/wikis/CTRM.wiki/10949/CTRM-PBI-Process)"
    type: string
    required: true
  - name: outputSpace
    label: "Output-Namespace (bestimmt wiki-md/<space>/). Standard: CTRM"
    type: string
    required: false
    default: "CTRM"
  - name: pageSlug
    label: "Zieldateiname ohne .md (leer = automatisch aus Seitentitel ableiten)"
    type: string
    required: false

Hints (nicht wiederholen):
- Zuerst lesen und anwenden:
  - .github/skills/wiki-to-markdown/SKILL.md (Hauptworkflow, Grafik-Klassifikation, Alt-Text-Standard)
  - .github/skills/wiki-to-markdown/references/extraction.md (ADO-Toolaufruf, Attachment-Download, Playwright-Fallback)
  - .github/skills/wiki-to-markdown/references/output-template.md (MD-Skelett, Mermaid-Rezepte, Asset-Naming)
  - .github/instructions/copilot.instructions.md (globale Regeln, .temp-Pflicht, Playwright SAML-Login-Flow)
  - .github/instructions/user.copilot.instructions.md (Sprache DE-CH, kein ß, kein Gendern)
- Goldstandard für Look & Feel: wiki-md/CTRM/ctrm-feature-process.md — bei Zweifelsfällen dort nachschauen.
- Antwort immer auf Deutsch (Schweizer Variante, kein ß). Keine Spekulation.
- Alle generierten Artefakte (Markdown + Bilder) in `.temp/wiki-converted/` ablegen — gitignoriert.
- Download-Temp in `.temp/wiki-import/{slug}/` ablegen; nach Abschluss leeren.
- **Phase 4b (ADO-Upload) IMMER automatisch ausführen, ohne Rückfrage.** Ziel-Parent: pageId 28309 (`/CTRM Manual migrated`).
- ADO-Konzepte (Work-Item-Form, PBI-States, DoR, DoD) via microsoft-docs MCP grounden.

# Aufgabe

Konvertiere die ADO-Wiki-Seite unter **{{wikiPageUrl}}** in eine Markdown-Datei.

- Output-Space: **{{outputSpace}}** (→ `wiki-md/{{outputSpace|lower}}/`)
- Slug (Dateiname): **{{pageSlug}}** (leer = automatisch ableiten)

Folge dem 6-Phasen-Workflow aus `.github/skills/wiki-to-markdown/SKILL.md` Schritt für Schritt.

## Erwartetes Ergebnis

Nach Abschluss:
1. `.temp/wiki-converted/{slug}.md` — vollständige Markdown-Datei (gitignoriert)
2. `.temp/wiki-converted/assets/{slug}/NN-name.png` — alle UI-Screenshots (gitignoriert)
3. ADO-Wiki-Seite erstellt unter `/CTRM Manual migrated/<Originaltitel>` (Phase 4b, automatisch)
4. Kurzer Report: Bilder behalten / umgewandelt (Mermaid) / als Text extrahiert + ADO-Seiten-ID und URL

> Die Artefakte liegen in `.temp/` und tauchen nicht im Git auf. Sie dienen als
> lokale Vorschau und als Quelle für den ADO-Wiki-Upload (Mermaid + /.attachments/-Pfade).
