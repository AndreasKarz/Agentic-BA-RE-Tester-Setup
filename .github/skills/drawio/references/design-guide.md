# Draw.io Design Guide – Schöne Diagramme

## Inhaltsverzeichnis
1. [Farbpaletten](#farben)
2. [Layout-Prinzipien](#layout)
3. [Typografie & Labels](#labels)
4. [Diagramm-Typ spezifische Regeln](#diagrammtypen)
5. [Anti-Patterns](#antipatterns)

---

## Farbpaletten {#farben}

### Azure-Architektur
Konsistente Azure-Farben nach Microsoft-Vorgabe:

| Layer / Typ | Fill | Stroke | Verwendung |
|---|---|---|---|
| Azure-Icons | `#0078d4` / `none` | `none` | Service-Icons |
| Web-Tier | `#dae8fc` | `#6c8ebf` | Frontend-Container |
| App-Tier | `#d5e8d4` | `#82b366` | Middleware-Container |
| Data-Tier | `#fff2cc` | `#d6b656` | Datenbank-Container |
| Security | `#f8cecc` | `#b85450` | DMZ, Firewall |
| Extern | `#f5f5f5` | `#666666` | On-Premise, Internet |

### BPMN-Prozessdiagramme

| Element | Fill | Stroke |
|---|---|---|
| Start-Event | `#CCFFCC` | `#00CC00` |
| End-Event | `#FFCCCC` | `#CC0000` |
| Task (User) | `#dae8fc` | `#6c8ebf` |
| Task (Service) | `#d5e8d4` | `#82b366` |
| Gateway | `#fff2cc` | `#d6b656` |
| Intermediate Event | `#FFFACD` | `#DAA520` |
| Pool/Lane | `#f5f5f5` | `#999999` |

### Kubernetes-Diagramme

| Element | Fill | Stroke |
|---|---|---|
| Cluster | `#E8F4FD` | `#326CE5` |
| Node | `#f5f5f5` | `#666666` |
| Namespace | `#EAF2FF` | `#326CE5` |
| Pod | Icons: `#326CE5` | `none` |
| Service | Icons: `#326CE5` | `none` |
| Ingress | Icons: `#326CE5` | `none` |

### ER-Diagramme

| Element | Fill | Stroke |
|---|---|---|
| Entity/Tabelle | `#dae8fc` | `#6c8ebf` |
| Primärschlüssel | `#d5e8d4` | `#82b366` |
| Fremdschlüssel | `#fff2cc` | `#d6b656` |
| Relation | `none` | `#666666` |

### Standard Farbsystem (universell)
```
Schwerpunkt:  #0078d4 (Microsoft Blau)
Erfolg/Grün:  #107C10
Warnung/Gelb: #FFB900  
Fehler/Rot:   #D83B01
Neutral:      #605E5C
Hell:         #F3F2F1
```

---

## Layout-Prinzipien {#layout}

### Grid und Positionierung
- **Grid:** 10px, Shapes auf Gridlinien ausrichten
- **Mindestabstand** zwischen Shapes: 20–30px
- **Container-Padding:** 30–40px innen (top: 50px wegen Titlebar)
- **Konsistente Größen:** Icons 65×65px, Tasks 120×60px, Gateways 50×50px

### Richtungsfluss

| Diagrammtyp | Richtung | Begründung |
|---|---|---|
| Prozesse / Workflows | Links → Rechts | Natürlicher Lesefluss |
| Hierarchien / Bäume | Oben → Unten | Klassische Ablaufdarstellung |
| Architektur-Layer | Oben → Unten | Internet → DMZ → App → DB |
| Netzwerkflüsse | Links → Rechts | Datenfluss-Konvention |

### Abstände und Gruppen
- **Logisch zusammengehöriges** in Swimlane/Container fassen
- **Max. 7 Elemente** pro Container (Kognitiver Limit)
- **Hierarchie-Tiefe** max. 3 Ebenen (Container → Gruppe → Shape)
- **Weiß-Raum** bewusst einsetzen – nicht jede Fläche füllen

### Connector-Design
- **Orthogonale Pfeile** (`edgeStyle=orthogonalEdgeStyle`) als Standard
- **Keine Kreuzungen** wenn vermeidbar – Shapes umpositionieren
- **Pfeil-Labels** nur wenn wirklich notwendig (Protokoll, Port)
- **Einheitlicher Stil:** entweder `dashed=1` für alle optionalen, `dashed=0` für alle Pflicht-Verbindungen

---

## Typografie & Labels {#labels}

### Textregeln
- **Container-Titel:** `fontStyle=1;fontSize=14` (Bold, 14px)
- **Shape-Labels:** `fontSize=12`, max. 2 Zeilen
- **Icons mit Label darunter:** `verticalLabelPosition=bottom;verticalAlign=top;fontSize=11`
- **Connector-Labels:** `fontSize=10`, kursiv für Protokoll (`fontStyle=2`)

### HTML in Labels
Draw.io unterstützt HTML in `value`:
```xml
value="&lt;b&gt;API Gateway&lt;/b&gt;&lt;br/&gt;Port 443"
```
→ Wird als: **API Gateway**  
Port 443

**HTML-Escaping-Regeln:**
| Zeichen | XML-escaped |
|---|---|
| `<` | `&lt;` |
| `>` | `&gt;` |
| `&` | `&amp;` |
| `"` | `&quot;` |
| Zeilenumbruch | `&lt;br/&gt;` oder `&#xa;` |

### Tooltip
```xml
<mxCell ... tooltip="Beschreibung erscheint beim Hover" ...>
```

---

## Diagramm-Typ spezifische Regeln {#diagrammtypen}

### Azure-Architekturdiagramm

**Empfohlene Struktur:**
```
[Internet/User]
       ↓
[Frontdoor / App Gateway]
       ↓
[Web Tier - App Services / WAF]
       ↓
[App Tier - Functions / AKS]
       ↓
[Data Tier - SQL / Cosmos / Storage]
       ↓
[Security - Key Vault / Entra ID]
```

**Regeln:**
- Icons immer `65×65px`, Label darunter
- Nie unterschiedliche Icon-Sets mischen (`azure2` + `mscae` nur bewusst)
- Subscription/Resource Group als Container
- VNets als Swimlane mit Subnetz-Kinder

### BPMN-Prozessdiagramm

**Regeln:**
- **Immer** mit Start-Event beginnen, mit End-Event enden
- Pool pro Organisationseinheit / System
- Lane pro Rolle/Abteilung
- Sequenzflüsse innerhalb des Pools, Message-Flows zwischen Pools
- Gateways **beschriften** (Bedingung auf Sequence Flow)
- Max. 15 Elemente pro Lane (sonst Subprocess auslagern)

### ER-Diagramm

**Regeln:**
- Primärschlüssel-Spalte immer als erste Zeile, farblich hervorheben
- FK-Verbindungen mit Crow's Foot-Notation (`ERone`/`ERmany`)
- Tabellen alphabetisch sortiert oder nach Fachdomäne gruppiert
- Junction-Tables (N:M) deutlich kleiner darstellen

### Kubernetes-Diagramm

**Regeln:**
- Cluster = äußerer Container
- Namespaces als farbige Swimlanes innerhalb des Clusters
- Pods mit Deployment/ReplicaSet gruppieren
- Services mit gestrichelter Linie zu Pods
- Ingress am oberen Rand des Clusters

---

## Anti-Patterns {#antipatterns}

### ❌ Zu vermeiden

| Anti-Pattern | Problem | Lösung |
|---|---|---|
| Shapes überlappen | Unlesbar | Positions überprüfen, Grid nutzen |
| Zu viele Farben | Visuelles Chaos | Max. 5 Farben pro Diagramm |
| Connector-Knäuel | Unverständlich | Shapes umpositionieren |
| Fehlende Labels | Bedeutungslos | Jedes Shape beschriften |
| Zu kleine Schrift | Unleserlich | Min. 11px für Labels |
| Keine Gruppierung | Kein Kontext | Swimlanes für logische Gruppen |
| Inkonsistente Shapes | Verwirrend | Eine Shape-Library pro Diagramm statt mischen |
| Zu detailliert | Überwältigend | Abstraktionsebene wählen, Details in Sub-Diagramm |

### ✅ Best Practices

- **Eine Story** pro Diagramm – klare Aussage definieren
- **Abstraktionsebene** konsistent halten
- **Legende** hinzufügen wenn nicht-Standard-Shapes verwendet
- **Version/Datum** im Diagramm-Titel oder als Text-Box
- **Mehrere Seiten** für verschiedene Sichten (Übersicht + Detail)
- **Export:** SVG für Dokumentation, PNG für Präsentationen
