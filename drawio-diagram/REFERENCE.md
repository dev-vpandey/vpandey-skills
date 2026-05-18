# Draw.io Generic Shape Reference

---

## ELEMENT REFERENCE

### Rectangle (standard node)
```xml
<mxCell id="node-api" value="API Gateway" vertex="1" parent="1"
  style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;fontStyle=1;fontSize=13;">
  <mxGeometry x="100" y="100" width="160" height="60" as="geometry"/>
</mxCell>
```

### Rounded rectangle (process step)
```xml
<mxCell id="node-cache" value="Cache" vertex="1" parent="1"
  style="rounded=1;arcSize=50;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;fontSize=13;">
  <mxGeometry x="320" y="100" width="140" height="60" as="geometry"/>
</mxCell>
```

### Ellipse (start/end/key concept)
```xml
<mxCell id="node-start" value="Start" vertex="1" parent="1"
  style="ellipse;whiteSpace=wrap;html=1;fillColor=#d5e8d4;strokeColor=#82b366;fontStyle=1;fontSize=13;">
  <mxGeometry x="100" y="100" width="140" height="60" as="geometry"/>
</mxCell>
```

### Diamond (decision)
```xml
<mxCell id="decision-1" value="Cache hit?" vertex="1" parent="1"
  style="rhombus;whiteSpace=wrap;html=1;fillColor=#fff2cc;strokeColor=#d6b656;fontSize=12;">
  <mxGeometry x="100" y="200" width="160" height="80" as="geometry"/>
</mxCell>
```

### Cylinder (database/storage)
```xml
<mxCell id="node-db" value="PostgreSQL" vertex="1" parent="1"
  style="shape=cylinder;whiteSpace=wrap;html=1;boundedLbl=1;fillColor=#dae8fc;strokeColor=#6c8ebf;fontSize=13;">
  <mxGeometry x="100" y="200" width="120" height="80" as="geometry"/>
</mxCell>
```

### Container / swimlane (zone grouping)

Always add `pointerEvents=0` to swimlane containers. Without it, draw.io treats the container as a routing obstacle and forces edges around its boundary.

For cross-layer diagrams: declare all nodes with `parent="1"` (not `parent="zone-id"`). Child-parented nodes cause cross-container edge routing bugs.

```xml
<mxCell id="zone-frontend" value="Frontend Layer" vertex="1" parent="1"
  style="swimlane;startSize=30;fillColor=#dae8fc;strokeColor=#6c8ebf;fontStyle=1;fontSize=13;opacity=20;pointerEvents=0;">
  <mxGeometry x="40" y="40" width="520" height="200" as="geometry"/>
</mxCell>
<!-- Cross-layer: keep nodes at parent="1" -->
<!-- Same-layer only: child nodes with parent="zone-frontend" are fine -->
<mxCell id="node-inside" value="React App" vertex="1" parent="1"
  style="rounded=1;whiteSpace=wrap;html=1;fillColor=#ffffff;strokeColor=#6c8ebf;fontSize=13;">
  <mxGeometry x="60" y="100" width="140" height="60" as="geometry"/>
</mxCell>
```

### Standalone text label
```xml
<mxCell id="label-1" value="Phase 1: 0–10k users" vertex="1" parent="1"
  style="text;html=1;align=center;fontStyle=1;fontSize=14;fillColor=none;strokeColor=none;">
  <mxGeometry x="100" y="40" width="200" height="30" as="geometry"/>
</mxCell>
```

---

## COLOUR SYSTEM

Style strings use `fillColor=#hex;strokeColor=#hex;`.

╔══════════════════════════════════╦═════════════╦═════════════╗
║ Role                             ║ fillColor   ║ strokeColor ║
╠══════════════════════════════════╬═════════════╬═════════════╣
║ Primary / input                  ║ #dae8fc     ║ #6c8ebf     ║
║ Success / output / correct       ║ #d5e8d4     ║ #82b366     ║
║ Warning / external / pending     ║ #ffe6cc     ║ #d79b00     ║
║ Error / failure / wrong          ║ #f8cecc     ║ #b85450     ║
║ Decision / note                  ║ #fff2cc     ║ #d6b656     ║
║ Processing / agent               ║ #e1d5e7     ║ #9673a6     ║
║ Neutral / structural             ║ #f5f5f5     ║ #666666     ║
╚══════════════════════════════════╩═════════════╩═════════════╝

Zone containers: use the same fill as the dominant node colour, add `opacity=30;` to keep it subtle.

---

## EDGE REFERENCE

### Same-layer edges
Source and target in the same swimlane / same y-band — auto-routing is fine.

```xml
<mxCell id="edge-1" value="" edge="1" parent="1" source="node-api" target="node-cache"
  style="edgeStyle=orthogonalEdgeStyle;rounded=1;html=1;exitX=1;exitY=0.5;entryX=0;entryY=0.5;">
  <mxGeometry relative="1" as="geometry"/>
</mxCell>
```

### Cross-layer edges
Source and target in different swimlanes, or fan-out spanning multiple layers: ALWAYS add explicit `Array as="points"` waypoints. Without them draw.io routes straight through intervening boxes.

Route through the **corridor gap** between swimlanes (typically 20–30px below the source swimlane's bottom edge). Each fan-out branch uses a distinct y-value in the corridor (stagger by 4–6px per branch) and a distinct `exitX` fraction on the source node.

```xml
<mxCell id="edge-cross" value="label" edge="1" parent="1"
  source="node-a" target="node-b"
  style="edgeStyle=orthogonalEdgeStyle;rounded=1;html=1;exitX=0.5;exitY=1;entryX=0.5;entryY=0;">
  <mxGeometry relative="1" as="geometry">
    <Array as="points">
      <mxPoint x="[source-center-x]" y="[corridor-y]"/>
      <mxPoint x="[target-center-x]" y="[corridor-y]"/>
    </Array>
  </mxGeometry>
</mxCell>

<!-- Fan-out: 3 branches from one source, staggered -->
<!-- branch 1: exitX=0.2  corridor y=482 -->
<!-- branch 2: exitX=0.5  corridor y=486 -->
<!-- branch 3: exitX=0.8  corridor y=490 -->
```

### Arrow style variants
- Default (filled arrowhead): omit `startArrow`/`endArrow`
- No arrowhead: `endArrow=none;`
- Both directions: `startArrow=block;endArrow=block;`
- Dashed: `dashed=1;`
- Inheritance (hollow triangle): `endArrow=open;endFill=0;`

---

## LAYOUT RULES

**Spacing:** 80px between node right-edges and next node left-edge. 60px vertically.

**Node sizes:** Standard `160×60`. Wide labels `200×60`. Diamond `160×80`. Cylinder `120×80`.

**IDs:** Descriptive slugs — `"node-api-gateway"`, `"edge-api-to-cache"`, `"zone-data-layer"`. Never `"1"` or `"2"` (reserved for root cells).

**Containers:** Child cells use `parent="[container-id]"` and x/y coordinates relative to the container's top-left corner.

**Large diagrams (>15 nodes):** Use swimlane containers to group. One swimlane per logical layer.

**Cross-layer edges:** Always use corridor waypoints. Swimlanes always get `pointerEvents=0`. Nodes in cross-layer diagrams always use `parent="1"`.
