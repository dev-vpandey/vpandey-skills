# drawio-mcp-integration-plan

## Context

The drawio-diagram skill writes `.drawio` XML files to disk and instructs the user to drag them onto app.diagrams.net. The draw.io MCP server (`mcp__drawio__open_drawio_xml`, `mcp__drawio__open_drawio_mermaid`) is available and opens a live editor directly — no drag-and-drop, lower token cost, simpler XML generation. This plan integrates the MCP into the skill via a three-tier routing system while leaving the existing file-write path completely unchanged.

## Decisions

| Decision | Outcome |
|---|---|
| Primary goals | Live preview + token efficiency |
| MCP detection | Check if `mcp__drawio__open_drawio_xml` is in available tools |
| MCP not configured | Silent fallback to Tier 3 (file write) |
| MCP configured, `-no-mcp` flag | Force Tier 3 (file write) |
| `-no-mcp` position | Start or end of user prompt, exact token |
| XML rules on MCP path (Tier 2) | Containers and nodes need explicit x/y/width/height — draw.io does not auto-size containers. Edges only: no `Array as="points"` waypoints, no exitX/exitY overrides |
| Mermaid colour support | draw.io's Mermaid renderer ignores `themeVariables` and `classDef` — all Mermaid output is grey. Colour-coded diagrams must use Tier 2 (XML with `fillColor=`) |
| XML rules on file path | REFERENCE.md unchanged |
| Mermaid for complex workflows | No — quality degrades. Mermaid only for sequence/class/ER/simple flowcharts with no colour requirement |
| AWS diagrams | Always Tier 2 (MCP XML) or Tier 3 (file write) — never Mermaid |

## Routing tiers

```
-no-mcp flag present?      → YES → Tier 3
MCP available?             → NO  → Tier 3
                           → YES ↓
Mermaid-suitable?          → YES → Tier 1
                           → NO  → Tier 2
```

**Tier 1 — Mermaid** (`open_drawio_mermaid`)
Sequence, class, ER, simple linear flowchart — structure only, no colour requirement.
draw.io's Mermaid renderer ignores all colour directives; diagrams render grey.

**Tier 2 — MCP XML** (`open_drawio_xml`)
Multi-layer workflows, swimlanes, fan-out/convergence, colour system, AWS diagrams, any diagram requiring colour coding.
Containers and nodes need explicit geometry; edges only are routing-free (no waypoints, no exitX/exitY).

**Tier 3 — File write** (unchanged)
REFERENCE.md rules apply in full. Write `.drawio`, tell user to drag to app.diagrams.net.

## Files to create or change

### CREATE `ROUTING.md`
`/Users/vpandey/.claude/skills/drawio-diagram/ROUTING.md`

Full tier decision logic, tier-specific XML rules, and delivery instructions.
Content:
```markdown
# Routing: MCP vs File Write

## Decision order

1. If prompt starts or ends with `-no-mcp` → **Tier 3**
2. If `mcp__drawio__open_drawio_xml` is not in available tools → **Tier 3**
3. Choose by diagram type:

| Diagram type                                           | Tier |
|--------------------------------------------------------|------|
| Sequence, class, ER, simple linear flowchart           | 1    |
| Swimlane workflow, fan-out, convergence, colour system | 2    |
| Any diagram with AWS service icons                     | 2    |

## Tier 1 — Mermaid (`open_drawio_mermaid`)
Generate Mermaid syntax. Call `open_drawio_mermaid`. Do not write a file.
Tell Vicky: "Diagram open in draw.io — [one line on what it argues]"

## Tier 2 — MCP XML (`open_drawio_xml`)
Generate draw.io XML with logical structure only:
- Do NOT compute x/y coordinates
- Do NOT add `Array as="points"` waypoints
- Do NOT set exitX/exitY/entryX/entryY overrides
- The mxfile/mxGraphModel/root wrapper and both root cells are still required
- Consult AWS-REFERENCE.md for icon styles when AWS services are present

Call `open_drawio_xml`. Do not write a file.
Tell Vicky: "Diagram open in draw.io — [one line on what it argues]"

## Tier 3 — File write
Follow REFERENCE.md rules in full (corridor waypoints, coordinates, exitX/exitY).
Write the `.drawio` file. Tell Vicky: "`[filename].drawio` ready — drag onto app.diagrams.net or open in VS Code."
```

### CREATE `EXAMPLES.md`
`/Users/vpandey/.claude/skills/drawio-diagram/EXAMPLES.md`

Three worked examples — one per tier — showing the contrast between approaches:
- Tier 1: login flow sequence diagram (Mermaid)
- Tier 2: order processing workflow (MCP XML, no coordinates)
- Tier 3: same order processing workflow (file write, full coordinates + corridor waypoints)

### EDIT `SKILL.md`
`/Users/vpandey/.claude/skills/drawio-diagram/SKILL.md`

Two targeted changes only (keep SKILL.md ≤ 100 lines):

**Change 1** — collapse OUTPUT INSTRUCTIONS steps 3–5 into one line:
```
# Before (steps 3-5):
3. Write the complete `.drawio` XML — file wrapper, all cells, all edges
4. Save as `[topic-slug].drawio`
5. Tell Vicky: "`[filename].drawio` ready — drag onto app.diagrams.net or open in VS Code"

# After:
3. Follow ROUTING.md — determines delivery (MCP live editor or file write) and XML rules
```

**Change 2** — add to REFERENCES section:
```
- Routing decision (MCP vs file, tier selection, XML rules per path) → **ROUTING.md**
```

### EDIT `docs/CONTEXT.md`
`/Users/vpandey/.claude/skills/drawio-diagram/docs/CONTEXT.md`

Add three terms after the Corridor entry:

- **MCP path**: Tier 1 or Tier 2 delivery — diagram opened in the live draw.io editor via MCP tools, no file written to disk. _Avoid_: online path, browser path.
- **File path**: Tier 3 delivery — `.drawio` file written to disk, opened manually by dragging onto app.diagrams.net or via VS Code. _Avoid_: offline path, local path.
- **`-no-mcp` flag**: Exact token placed at the start or end of a user prompt to force Tier 3 (file write) even when MCP is configured. _Avoid_: no-mcp option, MCP disable flag.

### CREATE `docs/adr/0003-mcp-routing-tiers.md`
Three-tier routing with different XML rules per path — hard to reverse, surprising without context (why does the same skill ignore REFERENCE.md on Tier 2?), result of real trade-offs.

## Verification

1. **Tier 1**: "a sequence diagram of a login flow" → `open_drawio_mermaid` called, no file written, live editor opens
2. **Tier 2**: "a swimlane workflow for order processing" → `open_drawio_xml` called, XML has explicit container/node geometry but no edge waypoints or exitX/exitY, live editor opens
3. **AWS**: "AWS architecture with Lambda and S3" → `open_drawio_xml` called, `mxgraph.aws4` styles in XML
4. **Flag**: "draw a flowchart -no-mcp" → `.drawio` file written, coordinates present in XML
5. **Fallback**: MCP tools absent → file write triggers silently, no error
6. **Regression**: Tier 3 output quality unchanged from pre-MCP skill
