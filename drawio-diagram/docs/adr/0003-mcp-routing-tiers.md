# Three-tier routing with path-specific XML rules

The drawio-diagram skill routes diagram generation across three tiers — Mermaid via MCP (Tier 1), XML via MCP (Tier 2), and file write (Tier 3) — and applies different XML layout rules depending on whether MCP is in use.

## Decision

When the draw.io MCP server is available, the skill uses it for live-editor delivery. When it is not available (or the user passes `-no-mcp`), the skill falls back to writing a `.drawio` file. Within the MCP path, Mermaid is used for diagram types it handles well (sequence, class, ER, simple flowcharts); XML is used for everything else (swimlane workflows, fan-out, convergence, AWS diagrams).

Critically, the XML rules differ by path:
- **MCP XML path**: containers and nodes require explicit x/y/width/height — draw.io does not auto-size swimlane containers (blank page results without geometry). Edges only are routing-free: no `Array as="points"` waypoints, no exitX/exitY overrides.
- **File write path**: REFERENCE.md rules apply in full — coordinates, corridor waypoints, and exit/entry overrides are required because there is no layout engine to fall back on.

A second discovered constraint: draw.io's Mermaid renderer ignores all colour directives — both `sequenceDiagram` themeVariables and `flowchart` `classDef` render grey. Tier 1 (Mermaid) is therefore structure-only. Any diagram requiring colour coding routes to Tier 2 (XML with explicit `fillColor=` per node).

## Why this is surprising without context

A future reader editing the skill will see REFERENCE.md say "always add corridor waypoints for cross-layer edges" and the MCP XML path say "never add waypoints." This looks contradictory. The reason: the MCP server hands the XML to draw.io's live layout engine, which re-routes edges automatically. The file-write path has no such engine — the XML is rendered exactly as written, so precise geometry is mandatory.

## Alternatives rejected

**Single XML ruleset for both paths**: would either produce over-specified XML on the MCP path (fighting the layout engine, producing worse output) or under-specified XML on the file path (edges routing through boxes, broken cross-layer connections).

**Mermaid for complex generic workflows**: quality degrades — no semantic colour system, `subgraph` is visually weaker than swimlanes, fan-out/convergence metaphors collapse under auto-layout. Mermaid is confined to diagram types it genuinely handles well.

**Separate sibling skill for MCP**: ruled out for the same reason as the v2 approach (ADR 0001) — skill discovery only enumerates top-level directories, and splitting would duplicate all generic shape and layout rules.

## Trade-offs accepted

- Skill logic is more complex: the routing decision tree adds cognitive load for future editors.
- XML generation on the MCP path still requires container geometry — the "simpler XML" benefit applies only to edges, not to layout overall.
- Mermaid's colour limitation means Tier 1 is narrower than originally scoped: it suits structure-only diagrams (sequence, class, ER) but cannot deliver colour-coded output. Colour requirements push the diagram to Tier 2 regardless of structural complexity.
- The `-no-mcp` flag is a power-user escape hatch and must be documented; users who don't know it exists will always get MCP delivery when the server is running.
