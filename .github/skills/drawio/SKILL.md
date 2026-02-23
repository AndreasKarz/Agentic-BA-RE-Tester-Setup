---
name: drawio
description: "Erstelle, bearbeite und überarbeite Draw.io / diagrams.net Diagramme als natives XML. Unterstützt Shape-Libraries: azure2, mscae, kubernetes, bpmn2, er. Nutzt @drawio/mcp (open_drawio_xml, open_drawio_csv, open_drawio_mermaid) um Diagramme sofort im Browser zu öffnen. Trigger: Draw.io, diagrams.net, .drawio-Datei erstellen/öffnen, Architekturdiagramm, Azure-Architektur, Kubernetes-Diagramm, BPMN-Prozess, ER-Diagramm, Netzwerkdiagramm, Flussdiagramm, Swimlane, UML, Organigramm, Diagramm überarbeiten/anpassen."
---

# Draw.io Skill

## Überblick

Dieser Skill generiert **natives Draw.io XML** und öffnet Diagramme sofort im Browser via `@drawio/mcp`.

## Workflow

```
1. Anforderungen verstehen (Diagrammtyp, Shapes, Layout)
2. Draw.io XML generieren
3. Diagramm via open_drawio_xml öffnen → User sieht es live im Editor
4. Bei Bedarf iterieren (XML anpassen → erneut öffnen)
5. Optional: .drawio-Datei in den Workspace schreiben
```

## MCP-Tools verwenden

Der `@drawio/mcp` MCP-Server stellt drei Tools bereit:

| Tool | Verwendet für |
|------|---------------|
| `open_drawio_xml` | Native Draw.io XML öffnen (Standard) |
| `open_drawio_csv` | CSV-Daten zu Diagramm konvertieren |
| `open_drawio_mermaid` | Mermaid-Syntax importieren |

**Bevorzuge immer `open_drawio_xml`** – es liefert die beste Kontrolle über Layout und Styles.

> Wenn `@drawio/mcp` nicht verfügbar ist: XML in eine `.drawio`-Datei im Workspace schreiben und den Pfad nennen.

## Shape-Libraries

Für Details zu Shape-Styles, Stencil-Namen und Beispielen → [references/shapes.md](references/shapes.md)

| Library-Key | Beschreibung | Wann verwenden |
|-------------|-------------|----------------|
| `azure2` | Microsoft Azure (neuestes Icon-Set) | Cloud-Architektur, Azure-Services |
| `mscae` | Microsoft Cloud Architecture Elements | Älteres MS-Architektur-Set, mixed Cloud |
| `kubernetes` | Kubernetes / K8s Icons | Container-Orchestrierung, K8s-Cluster |
| `bpmn2` | BPMN 2.0 Standard | Geschäftsprozesse, Prozessmodellierung |
| `er` | Entity Relationship | Datenbankmodelle, Datenstrukturen |

**Shape aktivieren im XML:**
```xml
<mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" 
  libs="azure2;kubernetes;bpmn2;er;mscae">
```

Der `libs`-Parameter aktiviert die Sidebar-Libraries im Editor.

## XML-Grundstruktur

Für vollständige Referenz → [references/xml-format.md](references/xml-format.md)

**Minimales Gerüst:**
```xml
<mxfile host="app.diagrams.net" version="24.0.0">
  <diagram name="Seite-1" id="unique-id">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" 
                  guides="1" tooltips="1" connect="1" arrows="1" 
                  fold="1" page="1" pageScale="1" 
                  pageWidth="1654" pageHeight="1169"
                  math="0" shadow="0">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>
        <!-- Shapes hier -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

**Shape-Zelle (Basis):**
```xml
<mxCell id="2" value="Label" style="STYLE_STRING" vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="120" height="60" as="geometry"/>
</mxCell>
```

**Connector (Pfeil):**
```xml
<mxCell id="10" value="" style="edgeStyle=orthogonalEdgeStyle;rounded=0;" 
        edge="1" source="2" target="3" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

## Layout-Prinzipien

- **Raster:** 10px Grid, Abstände als Vielfache von 10/20px
- **Richtung:** Top → Bottom (Prozesse), Left → Right (Datenfluss)
- **Gruppen:** Container (`swimlane`) für logische Gruppierungen
- **Farben:** Konsistente Farbgebung pro Layer/Komponenten-Typ (→ [references/design-guide.md](references/design-guide.md))
- **Labels:** Kurz und prägnant, bei Connectors Beschriftung über `value=`

## Diagramm-Typen und empfohlene Shapes

| Typ | Empfohlene Library | Einstiegspunkt |
|-----|-------------------|----------------|
| Azure-Architektur | `azure2` | `shape=mxgraph.azure2.azure` |
| Kubernetes-Cluster | `kubernetes` | `shape=mxgraph.kubernetes.group` |
| BPMN-Prozess | `bpmn2` | Start: `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.ellipsePerimeter` |
| ER-Diagramm | `er` | `shape=mxgraph.er.entity` |
| Allgemein / Flowchart | Built-in | `rounded=1;whiteSpace=wrap;html=1` |
| Swimlane / Prozess | Built-in | `swimlane;startSize=30` |

## Bestehende Diagramme überarbeiten

1. `.drawio`-Datei lesen (`read_file`)
2. XML analysieren: IDs, Style-Strings, bestehende Libraries
3. Zielsetzung verstehen: Neue Shapes? Layout? Farben? Labels?
4. Gezielt XML-Knoten mutieren (IDs beibehalten!)
5. Via `open_drawio_xml` validieren, dann Datei überschreiben

> IDs **niemals ändern** – sie werden von Connectors als `source`/`target` referenziert.

## Qualitäts-Checkliste

- [ ] Alle `id`-Attribute eindeutig (keine Duplikate)
- [ ] `parent="1"` für Root-Shapes, Container-Kinder auf Container-ID
- [ ] `libs=` enthält alle verwendeten Libraries
- [ ] Kein Shape überlappt ohne Absicht
- [ ] Connectors haben gültige `source`/`target`-Referenzen
- [ ] Labels sind HTML-escaped (`&amp;`, `&lt;`, `&gt;`)

## Setup

Wenn `@drawio/mcp` nicht konfiguriert ist → [references/mcp-setup.md](references/mcp-setup.md)
