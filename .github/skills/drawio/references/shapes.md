# Draw.io Shape-Library Referenz

## Inhaltsverzeichnis
1. [azure2 – Microsoft Azure](#azure2)
2. [mscae – Microsoft Cloud Architecture Elements](#mscae)
3. [kubernetes – Kubernetes/K8s](#kubernetes)
4. [bpmn2 – BPMN 2.0](#bpmn2)
5. [er – Entity Relationship](#er)
6. [Built-in Shapes (Standard)](#built-in)

---

## azure2 – Microsoft Azure {#azure2}

Das neueste offizielle Azure Icon-Set. Verwendet `shape=mxgraph.azure2.*`.

### Aktivierung im XML
```xml
<mxGraphModel libs="azure2">
```

### Häufige Shapes

| Shape-Name | Style-String | Beschreibung |
|---|---|---|
| Azure (Cloud) | `shape=mxgraph.azure2.azure` | Azure-Wolke allgemein |
| Virtual Machine | `shape=mxgraph.azure2.virtual_machine` | VM |
| SQL Database | `shape=mxgraph.azure2.sql_database` | Azure SQL DB |
| App Service | `shape=mxgraph.azure2.app_service` | Web App / Function App |
| Storage Account | `shape=mxgraph.azure2.storage` | Blob / Storage Account |
| Virtual Network | `shape=mxgraph.azure2.virtual_networks` | VNet |
| Subnet | `shape=mxgraph.azure2.subnet` | Subnetz |
| Load Balancer | `shape=mxgraph.azure2.load_balancer` | LB |
| Application Gateway | `shape=mxgraph.azure2.application_gateway` | App GW |
| Key Vault | `shape=mxgraph.azure2.key_vault` | Key Vault |
| Container Registry | `shape=mxgraph.azure2.container_registry` | ACR |
| Kubernetes Service | `shape=mxgraph.azure2.kubernetes_services` | AKS |
| Service Bus | `shape=mxgraph.azure2.service_bus` | Service Bus |
| API Management | `shape=mxgraph.azure2.api_managment` | APIM |
| Azure Active Directory | `shape=mxgraph.azure2.azure_active_directory` | AAD/Entra ID |
| Function App | `shape=mxgraph.azure2.function_apps` | Functions |
| Cosmos DB | `shape=mxgraph.azure2.azure_cosmos_db` | Cosmos |
| Event Hub | `shape=mxgraph.azure2.event_hubs` | Event Hubs |
| Redis Cache | `shape=mxgraph.azure2.azure_cache_redis` | Redis |
| Monitor | `shape=mxgraph.azure2.monitor` | Azure Monitor |

### Beispiel-Zelle
```xml
<mxCell id="vm1" value="Web Server" 
  style="shape=mxgraph.azure2.virtual_machine;sketch=0;html=1;pointerEvents=1;
         dashed=0;fillColor=#0078d4;strokeColor=none;strokeWidth=2;
         verticalLabelPosition=bottom;verticalAlign=top;align=center;
         outlineConnect=0;" 
  vertex="1" parent="1">
  <mxGeometry x="200" y="100" width="65" height="65" as="geometry"/>
</mxCell>
```

### Farb-Konventionen azure2
- **Fill:** `#0078d4` (Azure Blau), `none` für transparente Icons
- **Stroke:** meist `none` oder `#0078d4`
- **Größe:** 65×65px für Icons, Labels darunter via `verticalLabelPosition=bottom`

---

## mscae – Microsoft Cloud Architecture Elements {#mscae}

Älteres Microsoft-Set, gut kombinierbar mit azure2 für Legacy-Architektur-Diagramme.

### Aktivierung im XML
```xml
<mxGraphModel libs="mscae">
```

### Häufige Shapes

| Shape-Name | Style-String | Beschreibung |
|---|---|---|
| Server | `shape=mxgraph.mscae.server` | Server-Icon |
| Web Service | `shape=mxgraph.mscae.web_roles` | Web-Rolle |
| Worker Role | `shape=mxgraph.mscae.worker_roles` | Worker-Rolle |
| SQL Azure | `shape=mxgraph.mscae.sql_azure` | SQL Azure |
| Storage | `shape=mxgraph.mscae.cloud_storage` | Cloud Storage |
| VPN Gateway | `shape=mxgraph.mscae.vpn_gateway` | VPN GW |
| Service Bus Queue | `shape=mxgraph.mscae.service_bus_queues` | SB Queue |
| CDN | `shape=mxgraph.mscae.cdn` | CDN |
| Cloud | `shape=mxgraph.mscae.azure` | Azure Cloud |
| User | `shape=mxgraph.mscae.user` | Benutzer |
| On-Premise Network | `shape=mxgraph.mscae.on_premises_network` | On-Prem |

### Beispiel-Zelle
```xml
<mxCell id="svc1" value="Web Role"
  style="shape=mxgraph.mscae.web_roles;html=1;dashed=0;
         fillColor=#00BCD4;strokeColor=#006EAF;strokeWidth=2;
         verticalLabelPosition=bottom;verticalAlign=top;align=center;"
  vertex="1" parent="1">
  <mxGeometry x="300" y="150" width="65" height="65" as="geometry"/>
</mxCell>
```

---

## kubernetes – Kubernetes/K8s {#kubernetes}

Offizielle Kubernetes-Icons für Cluster-, Pod- und Netzwerk-Diagramme.

### Aktivierung im XML
```xml
<mxGraphModel libs="kubernetes">
```

### Häufige Shapes

| Shape-Name | Style-String | Beschreibung |
|---|---|---|
| Cluster | `shape=mxgraph.kubernetes.group` | K8s Cluster (Container) |
| Node | `shape=mxgraph.kubernetes.node` | Worker/Master Node |
| Pod | `shape=mxgraph.kubernetes.pod` | Pod |
| Deployment | `shape=mxgraph.kubernetes.deploy` | Deployment |
| ReplicaSet | `shape=mxgraph.kubernetes.rs` | ReplicaSet |
| Service | `shape=mxgraph.kubernetes.svc` | Service |
| Ingress | `shape=mxgraph.kubernetes.ing` | Ingress |
| ConfigMap | `shape=mxgraph.kubernetes.cm` | ConfigMap |
| Secret | `shape=mxgraph.kubernetes.secret` | Secret |
| Namespace | `shape=mxgraph.kubernetes.ns` | Namespace |
| PersistentVolume | `shape=mxgraph.kubernetes.pv` | PV |
| PersistentVolumeClaim | `shape=mxgraph.kubernetes.pvc` | PVC |
| StatefulSet | `shape=mxgraph.kubernetes.sts` | StatefulSet |
| DaemonSet | `shape=mxgraph.kubernetes.ds` | DaemonSet |
| CronJob | `shape=mxgraph.kubernetes.cronjob` | CronJob |
| HPA | `shape=mxgraph.kubernetes.hpa` | HorizontalPodAutoscaler |

### Namespace-Container (Swimlane mit K8s-Optik)
```xml
<!-- Namespace als Container -->
<mxCell id="ns1" value="production" 
  style="swimlane;startSize=30;fillColor=#dae8fc;strokeColor=#326CE5;
         fontSize=14;fontStyle=1;rounded=1;arcSize=5;" 
  vertex="1" parent="1">
  <mxGeometry x="50" y="50" width="600" height="400" as="geometry"/>
</mxCell>

<!-- Pod innerhalb des Namespace -->
<mxCell id="pod1" value="api-pod"
  style="shape=mxgraph.kubernetes.pod;html=1;pointerEvents=1;dashed=0;
         fillColor=#326CE5;strokeColor=none;
         verticalLabelPosition=bottom;verticalAlign=top;align=center;"
  vertex="1" parent="ns1">
  <mxGeometry x="100" y="100" width="65" height="65" as="geometry"/>
</mxCell>
```

### Farb-Konventionen kubernetes
- **K8s Blau:** `#326CE5` (offizielle Kubernetes-Farbe)
- **Node Fill:** `#f5f5f5`
- **Icons:** `fillColor=#326CE5;strokeColor=none`

---

## bpmn2 – BPMN 2.0 {#bpmn2}

Vollständige BPMN 2.0 Implementation für Geschäftsprozessmodellierung.

### Aktivierung im XML
```xml
<mxGraphModel libs="bpmn2">
```

### Wichtiger Hinweis
BPMN-Shapes nutzen einen zusammengesetzten Style mit `shape=mxgraph.bpmn.shape` und einem `perimeter`-Parameter.

### Events (Ereignisse)

| Typ | Style-Fragment | Beschreibung |
|---|---|---|
| Start (None) | `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.ellipsePerimeter;symbol=general;isLooping=0;isSequential=0;isCompensation=0;marker=none;` | Leeres Start-Event |
| End (None) | `...symbol=terminate;strokeWidth=3;` | Leeres End-Event |
| Start Timer | `...symbol=general;marker=timer;` | Timer Start |
| Intermediate | `...symbol=general;strokeWidth=2;` | Intermediate Event |
| Error End | `...symbol=terminate;marker=error;` | Fehler-End-Event |
| Message Start | `...symbol=message;` | Nachricht empfangen |

### Gateways

| Typ | Style-Fragment | Beschreibung |
|---|---|---|
| Exclusive (XOR) | `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.rhombusPerimeter;symbol=exclusiveGw;` | XOR-Gateway |
| Parallel (AND) | `...symbol=parallelGw;` | AND-Gateway |
| Inclusive (OR) | `...symbol=inclusiveGw;` | OR-Gateway |
| Event-Based | `...symbol=eventGateway;` | Event-basiert |

### Tasks / Activities

| Typ | Style-Fragment | Beschreibung |
|---|---|---|
| Task (None) | `shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.rectanglePerimeter;symbol=task;` | Standard Task |
| User Task | `...symbol=userTask;` | Benutzer-Aufgabe |
| Service Task | `...symbol=serviceTask;` | Service-Aufruf |
| Script Task | `...symbol=scriptTask;` | Skript |
| Subprocess | `...symbol=subProcess;` | Teilprozess |
| Call Activity | `...symbol=callActivity;strokeWidth=4;` | Call Activity |

### Pool und Lane
```xml
<!-- Pool (Participant) -->
<mxCell id="pool1" value="Kunde" 
  style="pool;startSize=30;horizontal=1;" 
  vertex="1" parent="1">
  <mxGeometry x="50" y="100" width="800" height="150" as="geometry"/>
</mxCell>

<!-- Lane innerhalb Pool -->
<mxCell id="lane1" value="Online-Kanal"
  style="swimlane;startSize=30;horizontal=0;"
  vertex="1" parent="pool1">
  <mxGeometry y="0" width="800" height="150" as="geometry"/>
</mxCell>
```

### Vollständiges Start-Event Beispiel
```xml
<mxCell id="start1" value="Bestellung&#xa;eingegangen" 
  style="shape=mxgraph.bpmn.shape;perimeter=mxPerimeter.ellipsePerimeter;
         symbol=general;isLooping=0;isSequential=0;isCompensation=0;
         marker=none;strokeColor=#00CC00;fillColor=#CCFFCC;
         verticalLabelPosition=bottom;verticalAlign=top;align=center;"
  vertex="1" parent="lane1">
  <mxGeometry x="50" y="40" width="50" height="50" as="geometry"/>
</mxCell>
```

### Sequence Flow (BPMN-Pfeil)
```xml
<mxCell id="flow1" value="" 
  style="edgeStyle=orthogonalEdgeStyle;html=1;exitX=1;exitY=0.5;entryX=0;entryY=0.5;"
  edge="1" source="start1" target="task1" parent="lane1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### Farb-Konventionen bpmn2
- **Start-Events:** `strokeColor=#00CC00;fillColor=#CCFFCC`
- **End-Events:** `strokeColor=#CC0000;fillColor=#FFCCCC`
- **Tasks:** `fillColor=#dae8fc;strokeColor=#326CE5` oder `fillColor=#fff2cc;strokeColor=#d6b656`
- **Gateways:** `fillColor=#fff2cc;strokeColor=#d6b656`

---

## er – Entity Relationship {#er}

Chen/Crow's Foot Notation für Datenbankmodelle.

### Aktivierung im XML
```xml
<mxGraphModel libs="er">
```

### Entity (Entität)
```xml
<mxCell id="ent1" value="Kunde" 
  style="shape=mxgraph.er.entity;whiteSpace=wrap;html=1;
         fillColor=#dae8fc;strokeColor=#6c8ebf;fontStyle=1;fontSize=14;" 
  vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="120" height="60" as="geometry"/>
</mxCell>
```

### Attribute (als Child-Zellen oder separate Shapes)
```xml
<!-- Attribut-Shape -->
<mxCell id="attr1" value="KundenID" 
  style="ellipse;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;" 
  vertex="1" parent="1">
  <mxGeometry x="80" y="220" width="80" height="40" as="geometry"/>
</mxCell>

<!-- Primärschlüssel (unterstrichen) -->
<mxCell id="pk1" value="&lt;u&gt;KundenID&lt;/u&gt;" 
  style="ellipse;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;" 
  vertex="1" parent="1">
  <mxGeometry x="80" y="220" width="80" height="40" as="geometry"/>
</mxCell>
```

### Crow's Foot Relations (moderneres ER)
```xml
<!-- 1:N Relation über Connector-Style -->
<mxCell id="rel1" value="hat"
  style="endArrow=ERmany;startArrow=ERone;html=1;exitX=1;exitY=0.5;
         entryX=0;entryY=0.5;edgeStyle=orthogonalEdgeStyle;"
  edge="1" source="ent1" target="ent2" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>

<!-- N:M Relation -->
<mxCell id="rel2" value=""
  style="endArrow=ERmanyToMany;startArrow=ERmanyToMany;html=1;
         edgeStyle=orthogonalEdgeStyle;"
  edge="1" source="ent2" target="ent3" parent="1">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### ER-Tabellen (modernere Darstellung mit Spalten)
```xml
<!-- Tabellen-Entity mit Zeilen -->
<mxCell id="tbl1" value="Produkt" 
  style="swimlane;startSize=30;fontStyle=1;fontSize=14;
         fillColor=#f5f5f5;strokeColor=#666;fontColor=#333;" 
  vertex="1" parent="1">
  <mxGeometry x="300" y="100" width="200" height="150" as="geometry"/>
</mxCell>

<mxCell id="col1" value="PK | ProduktID | INT" 
  style="text;strokeColor=none;fillColor=#dae8fc;align=left;
         verticalAlign=middle;spacingLeft=4;spacingRight=4;
         overflow=hidden;rotatable=0;fontStyle=1;" 
  vertex="1" parent="tbl1">
  <mxGeometry y="30" width="200" height="30" as="geometry"/>
</mxCell>

<mxCell id="col2" value="   | Name | VARCHAR(100)" 
  style="text;strokeColor=none;fillColor=none;align=left;
         verticalAlign=middle;spacingLeft=4;spacingRight=4;overflow=hidden;" 
  vertex="1" parent="tbl1">
  <mxGeometry y="60" width="200" height="30" as="geometry"/>
</mxCell>
```

---

## Built-in Shapes (Standard) {#built-in}

Diese Shapes brauchen keine Library-Aktivierung.

### Grundformen

| Shape | Style | Beschreibung |
|---|---|---|
| Rechteck | `rounded=0;whiteSpace=wrap;html=1;` | Standard Box |
| Gerundetes Rect | `rounded=1;whiteSpace=wrap;html=1;arcSize=10;` | Rounded Box |
| Ellipse/Kreis | `ellipse;whiteSpace=wrap;html=1;` | Kreis/Oval |
| Raute | `rhombus;whiteSpace=wrap;html=1;` | Entscheidung |
| Zylinder | `shape=mxgraph.flowchart.database;whiteSpace=wrap;html=1;` | Datenbank |
| Cloud | `shape=mxgraph.cisco.sites.generic_building;` | Gebäude |
| Dokument | `shape=mxgraph.flowchart.document;whiteSpace=wrap;html=1;` | Dokument |
| Parallelogramm | `shape=mxgraph.flowchart.io;whiteSpace=wrap;html=1;` | I/O |

### Swimlane / Container
```xml
<mxCell id="sw1" value="Internet-Zone" 
  style="swimlane;startSize=30;fillColor=#f0f0f0;strokeColor=#aaaaaa;
         fontSize=14;fontStyle=1;rounded=1;arcSize=5;" 
  vertex="1" parent="1">
  <mxGeometry x="50" y="50" width="400" height="300" as="geometry"/>
</mxCell>
```

### Connector-Styles

| Style | Beschreibung |
|---|---|
| `edgeStyle=orthogonalEdgeStyle;` | Rechtwinklige Pfeile (Standard) |
| `edgeStyle=elbowEdgeStyle;` | Elbow-Pfeile |
| `edgeStyle=entityRelationEdgeStyle;` | ER-Linien |
| `dashed=1;` | Gestrichelt |
| `endArrow=block;endFill=1;` | Gefüllter Pfeilkopf |
| `endArrow=open;` | Offener Pfeil |
| `startArrow=ERone;endArrow=ERmany;` | Crow's Foot 1:N |
