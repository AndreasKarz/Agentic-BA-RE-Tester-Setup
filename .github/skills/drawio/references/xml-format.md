# Draw.io XML Format Referenz

## Inhaltsverzeichnis
1. [Vollständige Dateistruktur](#struktur)
2. [mxGraphModel Attribute](#mxgraphmodel)
3. [mxCell Attribute](#mxcell)
4. [mxGeometry Attribute](#mxgeometry)
5. [Style-Strings](#styles)
6. [Mehrere Seiten](#mehrere-seiten)
7. [Gruppen und Container](#gruppen)
8. [Vollständige Beispiele](#beispiele)

---

## Vollständige Dateistruktur {#struktur}

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mxfile host="app.diagrams.net" 
        modified="2025-01-01T00:00:00.000Z" 
        agent="draw.io" 
        version="24.0.0" 
        type="device">
  <diagram name="Seite 1" id="diagram-001">
    <mxGraphModel ...>
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>
        <!-- Alle Shapes/Nodes hier mit parent="1" -->
        <!-- Container-Kinder verweisen auf Container-ID -->
      </root>
    </mxGraphModel>
  </diagram>
  <!-- Weitere Diagramme (Seiten) möglich -->
</mxfile>
```

**Wichtig:**
- `id="0"` und `id="1"` sind **immer pflicht** (Root-Cells)
- Alle anderen Shapes haben `parent="1"` (oder Container-ID)
- IDs müssen unique sein – verwende UUIDs oder sprechende Keys

---

## mxGraphModel Attribute {#mxgraphmodel}

```xml
<mxGraphModel 
  dx="1422"          <!-- Viewport X-Offset -->
  dy="762"           <!-- Viewport Y-Offset -->
  grid="1"           <!-- Raster anzeigen (0/1) -->
  gridSize="10"      <!-- Rasterabstand in px -->
  guides="1"         <!-- Hilfslinien (0/1) -->
  tooltips="1"       <!-- Tooltips (0/1) -->
  connect="1"        <!-- Auto-Connect (0/1) -->
  arrows="1"         <!-- Pfeile (0/1) -->
  fold="1"           <!-- Fold/Collapse (0/1) -->
  page="1"           <!-- Seiten-Modus (0/1) -->
  pageScale="1"      <!-- Skalierung -->
  pageWidth="1654"   <!-- Seitenbreite (A4L=1654, A4P=1169) -->
  pageHeight="1169"  <!-- Seitenhöhe -->
  math="0"           <!-- LaTeX-Formeln (0/1) -->
  shadow="0"         <!-- Schatten (0/1) -->
  libs="azure2;kubernetes;bpmn2;er;mscae"  <!-- Libraries -->
>
```

### Standard-Seitenformate

| Format | pageWidth | pageHeight |
|--------|-----------|------------|
| A4 Landscape | 1654 | 1169 |
| A4 Portrait | 1169 | 1654 |
| A3 Landscape | 2339 | 1654 |
| Infinite | (weglassen) | (weglassen) |

---

## mxCell Attribute {#mxcell}

### Shape (Vertex)
```xml
<mxCell 
  id="unique-id"          <!-- Pflicht, eindeutig -->
  value="Label Text"      <!-- Angezeigter Text (HTML möglich) -->
  style="style=string;"   <!-- Style-Definition -->
  vertex="1"              <!-- Markiert als Shape (nicht Edge) -->
  parent="1"              <!-- Parent-ID (1=Root, Container-ID=Child) -->
>
  <mxGeometry x="100" y="200" width="120" height="60" as="geometry"/>
</mxCell>
```

### Connector (Edge)
```xml
<mxCell 
  id="edge-id"
  value=""                <!-- Label auf dem Pfeil (optional) -->
  style="edgeStyle=orthogonalEdgeStyle;"
  edge="1"                <!-- Markiert als Pfeil -->
  source="shape-id-a"     <!-- Quell-Shape ID -->
  target="shape-id-b"     <!-- Ziel-Shape ID -->
  parent="1"
>
  <mxGeometry 
    relative="1"          <!-- Pflicht für Edges -->
    as="geometry"
  />
</mxCell>
```

### Swimlane/Container
```xml
<mxCell 
  id="container-1"
  value="Container-Titel"
  style="swimlane;startSize=30;..."
  vertex="1"
  parent="1"
>
  <mxGeometry x="50" y="50" width="400" height="300" as="geometry"/>
</mxCell>

<!-- Kind-Shape im Container: parent=Container-ID, Koordinaten relativ! -->
<mxCell id="child-1" value="Child" style="rounded=1;" vertex="1" parent="container-1">
  <mxGeometry x="50" y="80" width="100" height="50" as="geometry"/>
</mxCell>
```

---

## mxGeometry Attribute {#mxgeometry}

### Für Shapes (Vertices)
```xml
<mxGeometry 
  x="100"       <!-- X-Position (0 = links) -->
  y="100"       <!-- Y-Position (0 = oben) -->
  width="120"   <!-- Breite in px -->
  height="60"   <!-- Höhe in px -->
  as="geometry"
/>
```

### Für Connectors (Edges)
```xml
<mxGeometry 
  relative="1"  <!-- Pflicht! -->
  as="geometry"
>
  <!-- Optional: Waypoints für manuelle Pfad-Steuerung -->
  <Array as="points">
    <mxPoint x="300" y="150"/>
    <mxPoint x="300" y="250"/>
  </Array>
</mxGeometry>
```

---

## Style-Strings {#styles}

### Aufbau
Style-Strings sind semikolon-separierte `key=value`-Paare:
```
key1=value1;key2=value2;key3=value3;
```

### Universelle Properties

| Property | Werte | Beschreibung |
|---|---|---|
| `fillColor` | `#hex` / `none` | Füllfarbe |
| `strokeColor` | `#hex` / `none` | Linienfarbe |
| `strokeWidth` | `1`–`10` | Linienstärke |
| `fontColor` | `#hex` | Textfarbe |
| `fontSize` | `8`–`48` | Schriftgröße |
| `fontStyle` | `0/1/2/4` | Normal/Bold/Italic/Underline (addierbar: 3=Bold+Italic) |
| `align` | `left/center/right` | Horizontale Ausrichtung |
| `verticalAlign` | `top/middle/bottom` | Vertikale Ausrichtung |
| `spacingTop/Bottom/Left/Right` | px | Innen-Abstände |
| `rounded` | `0/1` | Abgerundete Ecken |
| `arcSize` | `0`–`50` | Rundungsgrad (%) |
| `shadow` | `0/1` | Schatten |
| `dashed` | `0/1` | Gestrichelt |
| `opacity` | `0`–`100` | Transparenz |
| `html` | `0/1` | HTML in Labels (immer `1`) |
| `whiteSpace` | `wrap` | Text-Umbruch |

### Shape-spezifische Properties

| Property | Beschreibung |
|---|---|
| `shape=mxgraph.*` | Stencil-Shape aus Library |
| `verticalLabelPosition=bottom` | Label unterhalb des Icons |
| `verticalAlign=top` | Label-Ausrichtung (wenn unten) |
| `pointerEvents=1` | Klick-Events auf Shape |
| `outlineConnect=0` | Kein automatischer Outline-Connect |
| `perimeter=mxPerimeter.ellipsePerimeter` | Ellipsen-Perimeter (BPMN) |
| `perimeter=mxPerimeter.rhombusPerimeter` | Rauten-Perimeter (Gateway) |

### Edge/Connector Properties

| Property | Beschreibung |
|---|---|
| `edgeStyle=orthogonalEdgeStyle` | Rechtwinklige Kanten |
| `edgeStyle=elbowEdgeStyle` | Elbow-Kanten |
| `edgeStyle=entityRelationEdgeStyle` | ER-Linien |
| `curved=1` | Gerundete Kurven |
| `exitX=1;exitY=0.5` | Ausgangs-Anschluss-Punkt (1=rechts, 0.5=Mitte) |
| `entryX=0;entryY=0.5` | Eingang-Anschluss-Punkt |
| `endArrow=block` | Pfeiltyp am Ende |
| `startArrow=none` | Pfeiltyp am Start |
| `endFill=1` | Gefüllter Pfeil |

### Pfeil-Typen (endArrow/startArrow)

| Wert | Beschreibung |
|---|---|
| `block` | Dreieck (gefüllt wenn `endFill=1`) |
| `open` | Offener Pfeil |
| `classic` | Klassischer Pfeil |
| `none` | Kein Pfeil |
| `ERone` | Crow's Foot "eins" |
| `ERmany` | Crow's Foot "viele" |
| `ERmandOne` | Crow's Foot "genau eins" |
| `ERmanyToMany` | Crow's Foot "N:M" |

---

## Mehrere Seiten {#mehrere-seiten}

```xml
<mxfile host="app.diagrams.net" version="24.0.0">
  <diagram name="Übersicht" id="page-001">
    <mxGraphModel ...>
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>
        <!-- Seite 1 Inhalte -->
      </root>
    </mxGraphModel>
  </diagram>
  
  <diagram name="Detail Azure" id="page-002">
    <mxGraphModel ...>
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>
        <!-- Seite 2 Inhalte -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

---

## Gruppen und Container {#gruppen}

### Logische Gruppe (unsichtbarer Container)
```xml
<!-- Gruppe: vertex=1, keine Borders -->
<mxCell id="grp1" value="" style="group" vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="300" height="200" as="geometry"/>
</mxCell>
<!-- Mitglieder: parent=Gruppenid -->
<mxCell id="m1" value="A" style="rounded=1;" vertex="1" parent="grp1">
  <mxGeometry x="0" y="0" width="100" height="60" as="geometry"/>
</mxCell>
```

### Swimlane-Container (sichtbarer Rahmen mit Titel)
```xml
<mxCell id="sw1" value="DMZ" 
  style="swimlane;startSize=30;horizontal=1;
         fillColor=#f8cecc;strokeColor=#b85450;
         rounded=1;arcSize=5;fontStyle=1;fontSize=14;" 
  vertex="1" parent="1">
  <mxGeometry x="50" y="100" width="500" height="200" as="geometry"/>
</mxCell>
```

---

## Vollständige Beispiele {#beispiele}

### Einfaches 3-Tier Web-Architektur-Diagramm

```xml
<mxfile host="app.diagrams.net" version="24.0.0">
  <diagram name="3-Tier Architecture" id="ex1">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" 
                  pageWidth="1654" pageHeight="1169" libs="azure2">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>
        
        <!-- Web Tier -->
        <mxCell id="tier-web" value="Web Tier" 
          style="swimlane;startSize=30;fillColor=#dae8fc;strokeColor=#6c8ebf;fontStyle=1;" 
          vertex="1" parent="1">
          <mxGeometry x="100" y="100" width="400" height="120" as="geometry"/>
        </mxCell>
        
        <mxCell id="app-svc" value="App Service" 
          style="shape=mxgraph.azure2.app_service;fillColor=#0078d4;strokeColor=none;
                 verticalLabelPosition=bottom;verticalAlign=top;align=center;" 
          vertex="1" parent="tier-web">
          <mxGeometry x="160" y="30" width="80" height="60" as="geometry"/>
        </mxCell>

        <!-- App Tier -->
        <mxCell id="tier-app" value="App Tier" 
          style="swimlane;startSize=30;fillColor=#d5e8d4;strokeColor=#82b366;fontStyle=1;" 
          vertex="1" parent="1">
          <mxGeometry x="100" y="270" width="400" height="120" as="geometry"/>
        </mxCell>
        
        <mxCell id="func1" value="Function App" 
          style="shape=mxgraph.azure2.function_apps;fillColor=#0078d4;strokeColor=none;
                 verticalLabelPosition=bottom;verticalAlign=top;align=center;" 
          vertex="1" parent="tier-app">
          <mxGeometry x="160" y="30" width="80" height="60" as="geometry"/>
        </mxCell>

        <!-- Data Tier -->
        <mxCell id="tier-data" value="Data Tier" 
          style="swimlane;startSize=30;fillColor=#fff2cc;strokeColor=#d6b656;fontStyle=1;" 
          vertex="1" parent="1">
          <mxGeometry x="100" y="440" width="400" height="120" as="geometry"/>
        </mxCell>
        
        <mxCell id="db1" value="SQL Database" 
          style="shape=mxgraph.azure2.sql_database;fillColor=#0078d4;strokeColor=none;
                 verticalLabelPosition=bottom;verticalAlign=top;align=center;" 
          vertex="1" parent="tier-data">
          <mxGeometry x="160" y="30" width="80" height="60" as="geometry"/>
        </mxCell>

        <!-- Connectors -->
        <mxCell id="e1" value="HTTPS" style="edgeStyle=orthogonalEdgeStyle;dashed=0;" 
          edge="1" source="app-svc" target="func1" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>
        <mxCell id="e2" value="SQL/TLS" style="edgeStyle=orthogonalEdgeStyle;dashed=0;" 
          edge="1" source="func1" target="db1" parent="1">
          <mxGeometry relative="1" as="geometry"/>
        </mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```
