# @drawio/mcp – Setup & Konfiguration

## Warum @drawio/mcp?

Das `@drawio/mcp` Package ist der **offizielle MCP-Server von Draw.io (JGraph)**. Er ermöglicht:

- Direkte Öffnung von Diagrammen im Browser (kein Copy-Paste nötig)
- Sofortige Vorschau während der Erstellung
- Unterstützung für XML, CSV und Mermaid-Format
- Lightbox (read-only) und Dark-Mode-Optionen

## Installation

### Variante 1: Via `npx` (empfohlen, kein Install nötig)
```json
{
  "mcpServers": {
    "drawio": {
      "command": "npx",
      "args": ["-y", "@drawio/mcp"]
    }
  }
}
```

### Variante 2: Global installiert
```bash
npm install -g @drawio/mcp
```

```json
{
  "mcpServers": {
    "drawio": {
      "command": "drawio-mcp"
    }
  }
}
```

## Konfiguration in VS Code (mcp.json)

Datei: `.vscode/mcp.json`

```json
{
  "servers": {
    "drawio": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@drawio/mcp"]
    }
  }
}
```

## Konfiguration für Claude Desktop

Datei (Windows): `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "drawio": {
      "command": "npx",
      "args": ["-y", "@drawio/mcp"]
    }
  }
}
```

## Verfügbare Tools

### `open_drawio_xml`
Öffnet natives Draw.io XML im Browser-Editor.

```json
{
  "content": "<mxfile>...</mxfile>",
  "lightbox": false,
  "dark": "auto"
}
```

| Parameter | Typ | Default | Beschreibung |
|---|---|---|---|
| `content` | string | **Pflicht** | Draw.io XML oder URL zu XML |
| `lightbox` | boolean | `false` | Read-only Ansicht |
| `dark` | string | `"auto"` | `"auto"`, `"true"`, `"false"` |

### `open_drawio_csv`
Konvertiert CSV-Daten in ein Diagramm.

```json
{
  "content": "id,label,type\n1,Server,server\n2,DB,database",
  "lightbox": false
}
```

### `open_drawio_mermaid`
Importiert Mermaid-Syntax als Draw.io-Diagramm.

```json
{
  "content": "graph TD\n  A[Client] --> B[Server]\n  B --> C[(DB)]"
}
```

## Wie es funktioniert

1. MCP-Server empfängt Diagram-Content (XML/CSV/Mermaid)
2. Komprimiert mit `pako deflateRaw` + Base64
3. Generiert `https://app.diagrams.net/#create=<base64>`-URL
4. URL wird zurückgegeben → Claude/VS Code öffnet sie im Browser
5. User sieht das editierbare Diagramm direkt in Draw.io

## Workflow-Best-Practices

```
1. XML generieren
2. open_drawio_xml aufrufen
3. User gibt Feedback ("mehr Abstand links", "Farbe ändern")
4. XML anpassen
5. open_drawio_xml erneut aufrufen
6. Bei Zufriedenheit: XML in .drawio-Datei speichern
```

## Datei speichern

Nach dem Erstellen/Überarbeiten die `.drawio`-Datei im Workspace ablegen:

```
.temp/diagrams/architecture.drawio      ← temporär während Erstellung
docs/architecture/azure-overview.drawio ← finale Ablage
```

## Troubleshooting

| Problem | Lösung |
|---|---|
| `npx: command not found` | Node.js installieren: https://nodejs.org |
| Server startet nicht | `npx @drawio/mcp --version` zum Testen |
| Browser öffnet sich nicht | URL manuell in `app.diagrams.net` öffnen |
| XML zu lang für URL | Diagramm auf mehrere Seiten aufteilen |

## Alternative: MCP App Server (kein Install)

Für Claude.ai oder MCP-Apps-kompatible Hosts steht ein gehosteter Server bereit:

```
https://mcp.draw.io/mcp
```

Dieser rendert Diagramme **inline im Chat** (kein Browser-Tab nötig).
