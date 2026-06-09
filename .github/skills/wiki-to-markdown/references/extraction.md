# Extraction Reference — ADO Wiki → Lokal

## 1. URL-Parsing

ADO-Wiki-URL-Muster:
```
https://dev.azure.com/{org}/{project}/_wiki/wikis/{wikiId}/{pageId}/{page-slug}
```

Beispiel:
```
https://dev.azure.com/swisslife/CTRM/_wiki/wikis/CTRM.wiki/10949/CTRM-PBI-Process
```

Extrahierte Variablen:
| Variable | Wert |
|----------|------|
| `org` | `swisslife` |
| `project` | `CTRM` |
| `wikiId` | `CTRM.wiki` |
| `pageId` | `10949` |
| `slug` (Output) | `ctrm-pbi-process` (kebab-case-lowercase aus letztem URL-Segment) |

---

## 2. ADO MCP — Seite laden

```
Tool: mcp_ado_wiki_get_page
Parameter:
  organization: {org}
  project:      {project}
  wikiIdentifier: {wikiId}
  id:           {pageId}        ← bevorzugt (stabil)
  -- oder --
  path:         /{page-slug}    ← Fallback
  includeContent: true
```

Die Antwort enthält:
- `content` — Roh-Markdown der Seite
- `path` — Pfad innerhalb der Wiki-Hierarchie
- `gitItemPath` — Git-Pfad (für Metadaten)
- `lastPublishedDate` / `lastAuthor` — Metadaten für den Markdown-Header

Alle Bild-Referenzen im `content` folgen dem Muster:
```markdown
![Beschriftung](/.attachments/DateiName-GUID.png)
```

Alle solche Referenzen mit einer Regex extrahieren:
```
\(\/\.attachments\/([^)]+)\)
```

---

## 3. Attachments herunterladen

### 3a — ADO REST API (bevorzugt)

> **⚠ Wichtig:** Der `_apis/wiki/wikis/{wikiId}/attachments`-Endpoint unterstützt nur `PUT` (Upload), **nicht** `GET` (Download).
> Attachments werden über die **Git-Items-API** heruntergeladen.

**Endpunkt (korrekt für Download):**
```
GET https://dev.azure.com/{org}/{project}/_apis/git/repositories/{repoId}/items
    ?path=/.attachments/{filename}
    &download=true
    &api-version=7.1
```

`{repoId}` = Wiki-Repository-ID (z. B. `1c7aeb5a-6801-4397-851d-48801d9a9207` für CTRM.wiki).
Diese ID ist identisch mit der Wiki-ID aus `wiki_get_wiki` (`id`-Feld).

Dabei ist `{filename}` der vollständige Dateiname inkl. GUID und Extension,
z. B. `image-7bccd6bf-214c-4362-93c3-a3d3669ce5ac.png`.

**Access Token holen (Azure DevOps Resource):**
```powershell
$token = az account get-access-token `
    --resource 499b84ac-1321-427f-aa17-267ca6975798 `
    --query accessToken --output tsv
```

**Einzelnes Bild herunterladen (PowerShell):**
```powershell
$outPath = ".temp/wiki-import/{slug}/{filename}"
Invoke-WebRequest `
    -Uri "https://dev.azure.com/{org}/{project}/_apis/wiki/wikis/{wikiId}/attachments?name={filename}&api-version=7.1" `
    -Headers @{ Authorization = "Bearer $token"; Accept = "application/octet-stream" } `
    -OutFile $outPath
```

**Smoke-Test nach erstem Bild:**
```powershell
$fileInfo = Get-Item $outPath
if ($fileInfo.Length -eq 0) {
    Write-Warning "Smoke-Test fehlgeschlagen — Playwright-Fallback starten"
}
```

**Batch (alle Attachments in Schleife):**
```powershell
foreach ($filename in $attachmentNames) {
    Invoke-WebRequest `
        -Uri "https://dev.azure.com/{org}/{project}/_apis/wiki/wikis/{wikiId}/attachments?name=$filename&api-version=7.1" `
        -Headers @{ Authorization = "Bearer $token"; Accept = "application/octet-stream" } `
        -OutFile ".temp/wiki-import/{slug}/$filename"
    Start-Sleep -Milliseconds 200   # Rate-Limiting vermeiden
}
```

> Token-Lebensdauer: ~1 Stunde. Bei `401` Token erneuern und Batch fortsetzen.

---

### 3b — Playwright-Fallback (bei Auth-Fehler oder leerem Bild)

Ablauf nach `copilot.instructions.md` SAML-Login-Flow:

```
1. mcp_playwright_browser_navigate → Wiki-Seiten-URL
2. mcp_playwright_browser_snapshot → auf Microsoft-Login prüfen
3. Account-Button mit Domain swisslife.ch klicken
4. Warten bis ADO-Seite vollständig geladen
5. Alle <img>-Elemente innerhalb .wiki-content-container sammeln
   → src-Attribut enthält authentifizierten CDN-URL mit SAS-Token
6. Jeden URL via Invoke-WebRequest herunterladen (kein zusätzliches Auth-Header nötig)
7. Datei speichern als .temp/wiki-import/{slug}/{NN}-{originalName}
```

**Alternative — Element-Screenshot:**
Wenn CDN-URL nicht zuverlässig ist:
```
mcp_playwright_browser_screenshot
    → Bereich um das <img>-Element
    → als .temp/wiki-import/{slug}/NN-screenshot.png speichern
```

Element-Screenshots haben geringere Qualität als Original-Attachments.
Nur als letzten Ausweg verwenden.

---

## 4. Asset-Umbenennung und Kopieren

Nach erfolgreichem Download aus `.temp/wiki-import/{slug}/`:

> **Zielordner:** `.temp/wiki-converted/assets/{slug}/` — gitignoriert, kein Commit nötig.

```powershell
# Bilder nummeriert in Lesereihenfolge umbenennen
$counter = 1
foreach ($imageRef in $orderedImageRefs) {
    $source = ".temp/wiki-import/{slug}/{imageRef.OriginalName}"
    $dest   = ".temp/wiki-converted/assets/{slug}/{counter:D2}-{shortname}.png"
    Copy-Item $source $dest
    $counter++
}
```

Naming-Regeln:
- `NN` = zweistellig (01, 02a, 02b, 03 …)
- `shortname` = kurzes, beschreibendes Kürzel auf Englisch oder Deutsch
  (z. B. `funneling`, `analyzing-1`, `ready-for-flight`)
- Sonderfall Unter-Schritte: `02a-analyzing-1-2.png`, `02b-analyzing-2-2.png`

### Ordner-Struktur nach Abschluss

```
.temp/
├── wiki-import/
│   └── {slug}/          ← Originale (werden geleert nach Copy)
└── wiki-converted/
    ├── {slug}.md         ← Fertiges Markdown (lokale Referenz-Ansicht)
    └── assets/
        └── {slug}/
            ├── 01-name.png
            ├── 02-name.png
            └── ...
```
