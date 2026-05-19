# Routing: MCP vs File Write

## Decision order

1. If prompt starts or ends with `-no-mcp` → **Tier 3**
2. If `mcp__drawio__open_drawio_xml` is not in available tools → **Tier 3**
3. Choose by diagram type:

╔══════════════════════════════════════════════════════════════╦══════╗
║ Diagram type                                                 ║ Tier ║
╠══════════════════════════════════════════════════════════════╬══════╣
║ Sequence, class, ER, simple flowchart — no colour needed     ║  1   ║
║ Swimlane workflow, fan-out, convergence, colour system       ║  2   ║
║ Any diagram with AWS service icons                           ║  2   ║
║ Any diagram where colour coding is required                  ║  2   ║
╚══════════════════════════════════════════════════════════════╩══════╝

## Tier 1 — Mermaid (`open_drawio_mermaid`)
Generate Mermaid syntax. Call `open_drawio_mermaid`. Do not write a file.
Tell Vicky: "Diagram open in draw.io — [one line on what it argues]"

**Colour limitation:** draw.io's Mermaid renderer ignores both `sequenceDiagram` themeVariables and `flowchart` `classDef`. Mermaid diagrams render in the default grey palette — no per-node colour control. If colour coding is required, use Tier 2 (XML with explicit `fillColor=` per node) instead.

## Tier 2 — MCP XML (`open_drawio_xml`)
Generate draw.io XML with logical structure — containers and nodes need explicit geometry, edges do not:
- DO provide x/y/width/height on swimlane containers and child nodes (draw.io does not auto-size containers)
- Do NOT add `Array as="points"` waypoints on edges
- Do NOT set exitX/exitY/entryX/entryY overrides on edges
- The mxfile/mxGraphModel/root wrapper and both root cells are still required
- Consult AWS-REFERENCE.md for icon styles when AWS services are present

Call `open_drawio_xml`. Do not write a file.
Tell Vicky: "Diagram open in draw.io — [one line on what it argues]"

## Tier 3 — File write
Follow REFERENCE.md rules in full (corridor waypoints, coordinates, exitX/exitY).
Write the `.drawio` file. Tell Vicky: "`[filename].drawio` ready — drag onto app.diagrams.net or open in VS Code."
