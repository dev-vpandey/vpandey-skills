---
name: drawio-diagram
description: Generate draw.io diagrams (.drawio files) for any visual — system design, DSA patterns, LLD class diagrams, Claude Code workflows, interview frameworks, architecture flows, and AWS cloud architectures with native AWS iconography (EC2, S3, Lambda, etc. via mxgraph.aws4). No render pipeline needed. Triggered by "generate diagram", "draw this", "diagram for", "visualise", "aws diagram", "aws architecture", "cloud architecture", "generate architectural diagram using aws", or proactively when a visual would sharpen an explanation.
---

# Draw.io Diagram Skill

Generate a `.drawio` XML file. Open by dragging onto **app.diagrams.net** or in VS Code (Draw.io Integration extension). No setup, no dependencies, no render loop.

## CORE PRINCIPLE: DIAGRAMS ARGUE

The shape IS the meaning. Before writing XML, answer:
1. What is the core argument this diagram makes?
2. What visual structure mirrors it — layers, fan-out, convergence, timeline, comparison?

The Isomorphism Test: remove all labels. Does the structure still communicate the concept?

## FILE STRUCTURE

Every `.drawio` file follows this wrapper. Never omit the two root cells.

```xml
<mxfile host="app.diagrams.net">
  <diagram name="Page-1" id="diagram-1">
    <mxGraphModel grid="0" gridSize="10" pageWidth="1169" pageHeight="827">
      <root>
        <mxCell id="0"/>
        <mxCell id="1" parent="0"/>
        <!-- all diagram elements go here, parent="1" -->
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

## MANDATORY RULES

1. Always include `<mxCell id="0"/>` and `<mxCell id="1" parent="0"/>` — diagram breaks without them
2. Every vertex needs `vertex="1"`, every edge needs `edge="1"` — never mix
3. Every element needs `parent="1"` (or a container ID for child elements)
4. Edges connecting shapes use `source="id"` and `target="id"` — never hardcoded coordinates
5. IDs must be unique across the entire file
6. `whiteSpace=wrap;html=1;` on all vertex styles to enable text wrapping

## LAYOUT STRATEGIES

╔══════════════════════════╦══════════════════════════════════════════════════════════════╗
║ Strategy                 ║ Use for                                                      ║
╠══════════════════════════╬══════════════════════════════════════════════════════════════╣
║ Vertical layers          ║ System architecture, request flow — swimlanes top to bottom  ║
║ Horizontal timeline      ║ Workflows, sequences, interview frameworks                   ║
║ Fan-out                  ║ One-to-many — single source, arrows to N targets below       ║
║ Convergence              ║ Aggregation, map-reduce — N inputs to one output             ║
║ Two-column comparison    ║ Wrong vs right, senior vs staff — left red, right green      ║
║ Tree                     ║ Class hierarchy, decision trees — root at top                ║
╚══════════════════════════╩══════════════════════════════════════════════════════════════╝

## AWS QUICK-START

When a diagram includes AWS services, keep the existing layout unchanged — same swimlanes, zones, edges, and edge labels. Only swap each AWS service box for a 60×60 icon at the same centre position; all non-AWS nodes stay as plain rectangles. Label icons with service name only (sub-details overlap edge labels).

**Icon style:** `sketch=0;outlineConnect=0;fontColor=#232F3E;gradientColor=none;strokeColor=none;fillColor=__COLOR__;labelBackgroundColor=#ffffff;align=left;labelPosition=right;verticalLabelPosition=middle;verticalAlign=middle;html=1;fontSize=11;fontStyle=0;aspect=fixed;shape=mxgraph.aws4.__SHAPE__;whiteSpace=wrap;` — size `width="60" height="60"`.

Label to the right keeps the icon's top/bottom connection points clear of edge labels. Include sub-details as a second line: `<b>Service Name</b><br/><font style='font-size:9px;color:#555;'>sub detail</font>`.

**S3 exception:** use `shape=mxgraph.aws4.resourceIcon;resIcon=mxgraph.aws4.s3;fillColor=#7AA116` — the direct S3 shape renders as a plain coloured box.

Shapes and category colours → **AWS-REFERENCE.md**

## OUTPUT INSTRUCTIONS

1. Answer: what is the argument? what structure mirrors it?
2. For AWS diagrams: identify which nodes are AWS services; consult AWS-REFERENCE.md for shape name and colour
3. Follow ROUTING.md — determines delivery (MCP live editor or file write) and XML rules
4. One line on what the diagram argues

Do not print raw XML in the conversation unless asked — write the file directly.

## REFERENCES

- Routing decision (MCP vs file, tier selection, XML rules per path) → **ROUTING.md**
- Generic shapes, colour system, layout rules → **REFERENCE.md**
- AWS service icons and cheat sheet → **AWS-REFERENCE.md**
- Worked examples → **EXAMPLES.md**
