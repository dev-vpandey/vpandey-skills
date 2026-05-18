# Extend drawio-diagram skill — AWS components support

## Context

The existing skill at `/Users/vpandey/.claude/skills/drawio-diagram/SKILL.md` (233 lines) generates `.drawio` XML for generic shapes (rounded rect, ellipse, diamond, cylinder, swimlane). It works well for system design, DSA, LLD, workflow diagrams. It does **not** know about AWS service icons — so any AWS architecture diagram comes out as plain rectangles labelled "EC2", "S3", etc. instead of recognisable AWS iconography.

Goal: extend the skill so that when the user asks for an AWS architecture diagram, the output uses draw.io's native AWS icon library (`mxgraph.aws4`) and AWS container shapes (VPC, Region, Subnet, etc.), producing diagrams that look canonical and authentic. Existing non-AWS behaviour must be preserved — AWS is additive.

User created `v2/` subfolder as the staging area. When this plan is implemented and approved, contents of `v2/` will be copied up one level to overwrite/extend the current skill, then `v2/` is deleted.

## Decisions (locked via grilling)

╔══════════════════════════════════════════╦═════════════════════════════════════════════════════════════╗
║ Question                                 ║ Decision                                                    ║
╠══════════════════════════════════════════╬═════════════════════════════════════════════════════════════╣
║ Packaging                                ║ v2/ holds the complete new skill; promote by overwriting v1 ║
║ AWS icon set                             ║ `mxgraph.aws4` only                                         ║
║ Service scope                            ║ Generative pattern + top-30 cheat sheet                     ║
║ AWS containers                           ║ Core 6: Account, Region, VPC, AZ, Public Subnet, Private    ║
║ Reference format                         ║ One reusable XML template + compact table of `resIcon`/colour║
║ Scripts                                  ║ None — pure XML-inline, consistent with v1                  ║
║ Triggers                                 ║ Extend existing + add "generate architectural diagram using aws", "aws diagram", "aws architecture", "cloud architecture"║
╚══════════════════════════════════════════╩═════════════════════════════════════════════════════════════╝

## Target file layout (inside v2/)

```
v2/
├── SKILL.md                                  # ~100-120 lines — wrapper, core principle, layout, AWS quick-start, pointers
├── REFERENCE.md                              # generic shape reference, colour system, edge routing rules
├── AWS-REFERENCE.md                          # AWS template, 30-service table, 6 container styles, AWS routing notes
├── EXAMPLES.md                               # 2-3 worked AWS diagrams (3-tier, serverless, multi-AZ)
├── plan/
│   └── i-want-to-extend-expressive-kernighan.md   # this plan file, moved here at implementation time
└── docs/
    ├── CONTEXT.md                            # per grill-with-docs: glossary of skill-specific terms
    └── adr/
        ├── 0001-v2-promotes-by-overwriting-v1.md
        └── 0002-standardise-on-mxgraph-aws4-icon-set.md
```

This splits v1's 233-line SKILL.md per the `write-a-skill` standard (<100 lines, factor reference material into separate files). The `plan/` and `docs/` folders capture the grilling output per `grill-with-docs` standards so future readers can see why decisions were made.

### v2/SKILL.md — what stays inline

- YAML frontmatter (name + extended description with new triggers)
- Core principle ("Diagrams Argue")
- File wrapper template (`<mxfile>` skeleton)
- Mandatory rules (the 6 numbered rules)
- Layout strategies table
- AWS quick-start (5-10 lines: when to use AWS shapes, where to find the catalogue)
- Output instructions
- Pointers: "Generic shapes → REFERENCE.md", "AWS shapes → AWS-REFERENCE.md", "Worked examples → EXAMPLES.md"

### v2/REFERENCE.md — what moves out of v1 SKILL.md

- Element reference for generic shapes (rectangle, rounded rect, ellipse, diamond, cylinder, arrow same-layer, arrow cross-layer, swimlane, text label)
- Colour system table
- Layout rules in detail (spacing, node sizes, IDs, container parenting, cross-layer edge waypoints)

### v2/AWS-REFERENCE.md — new content

**1. Service-node template** (one ~10-line XML block with `__RESICON__` and `__FILLCOLOR__` placeholders, plus the standard AWS-shape boilerplate: `sketch=0`, `points=[...]`, `outlineConnect=0`, `fontColor=#232F3E`, `aspect=fixed`, `verticalLabelPosition=bottom`, `shape=mxgraph.aws4.resourceIcon`)

**2. Top-30 service table** — columns: Common name | `resIcon=` value | category fill colour

The 30 services:
- **Compute**: EC2, Lambda, ECS, EKS, Fargate
- **Storage**: S3, EBS, EFS
- **Database**: RDS, DynamoDB, ElastiCache, Redshift, Aurora
- **Networking**: VPC, ELB, Route53, CloudFront, API Gateway
- **Messaging**: SQS, SNS, EventBridge, Kinesis, Step Functions
- **Identity**: IAM, Cognito, KMS
- **Observability**: CloudWatch, CloudTrail
- **Analytics**: Glue, Athena

Each row gives the `resIcon=mxgraph.aws4.<exact_name>` value (the non-obvious ones — `simple_storage_service_s3`, `lambda`, `elastic_container_service`, etc.) and the AWS service-category fill colour (compute=orange #ED7100, storage=green #7AA116, database=blue #C925D1, networking=purple #8C4FFF, etc.).

**3. Six container shapes** — Account, Region, VPC, AZ, Public Subnet, Private Subnet. Each shown with its `grIcon=` value, container style string, and a nesting example (Region > VPC > AZ > Subnet > service).

**4. AWS-specific routing notes** — AWS containers nest deeply. Same `pointerEvents=0` rule applies. AWS service nodes are `vertex="1"` like any other; edges use the same orthogonal/waypoint rules.

### v2/EXAMPLES.md — 2-3 complete diagrams

1. **3-tier web app**: Route53 → CloudFront → ALB → ECS Fargate → RDS (multi-AZ inside VPC inside Region)
2. **Serverless API**: API Gateway → Lambda → DynamoDB + SNS → SQS → Lambda
3. **Multi-AZ HA**: Region containing 2 AZs, each with public/private subnets, ALB spanning AZs

Each example: full `.drawio` file content the user can copy/save and open directly.

## Description string (within 1024 chars)

```
Generate draw.io diagrams (.drawio files) for any visual — system design, DSA patterns, LLD class diagrams, Claude Code workflows, interview frameworks, architecture flows, and AWS cloud architectures with native AWS iconography (EC2, S3, Lambda, VPC, etc. via mxgraph.aws4). No render pipeline needed. Triggered by "generate diagram", "draw this", "diagram for", "visualise", "aws diagram", "aws architecture", "cloud architecture", "generate architectural diagram using aws", "generate design diagram using aws", or proactively when a visual would sharpen an explanation.
```

## Files to be created (inside v2/)

Skill content:
- `/Users/vpandey/.claude/skills/drawio-diagram/v2/SKILL.md`
- `/Users/vpandey/.claude/skills/drawio-diagram/v2/REFERENCE.md`
- `/Users/vpandey/.claude/skills/drawio-diagram/v2/AWS-REFERENCE.md`
- `/Users/vpandey/.claude/skills/drawio-diagram/v2/EXAMPLES.md`

Plan + grilling artifacts (per `grill-with-docs`):
- `/Users/vpandey/.claude/skills/drawio-diagram/v2/plan/i-want-to-extend-expressive-kernighan.md` — moved from `/Users/vpandey/.claude/plans/` after plan exits
- `/Users/vpandey/.claude/skills/drawio-diagram/v2/docs/CONTEXT.md` — skill-domain glossary (see content sketch below)
- `/Users/vpandey/.claude/skills/drawio-diagram/v2/docs/adr/0001-v2-promotes-by-overwriting-v1.md`
- `/Users/vpandey/.claude/skills/drawio-diagram/v2/docs/adr/0002-standardise-on-mxgraph-aws4-icon-set.md`

## docs/CONTEXT.md — content sketch

Skill-specific terms (not general programming concepts):

- **Skill** — a Claude agent capability lived in a top-level directory under `~/.claude/skills/` with a `SKILL.md` at its root. _Avoid_: plugin, extension.
- **Service node** — a single draw.io `mxCell` representing one AWS service, using `shape=mxgraph.aws4.resourceIcon` and a `resIcon=` selector. _Avoid_: AWS box, service shape.
- **Group container** — a draw.io `mxCell` representing an AWS scope boundary (Region, VPC, AZ, Subnet, Account) using `shape=mxgraph.aws4.group` and a `grIcon=` selector. _Avoid_: container, zone, swimlane (those are generic; group container is AWS-specific).
- **`resIcon`** — the style attribute selecting the AWS service icon (e.g., `resIcon=mxgraph.aws4.simple_storage_service_s3`).
- **`grIcon`** — the style attribute selecting the AWS group-container variant.
- **Corridor** — the empty band (~20-30px) between adjacent swimlanes/containers where cross-layer edges route via `Array as="points"` waypoints. _Avoid_: gutter, gap.
- **Promote** — the act of copying `v2/*` up one directory level, overwriting the v1 SKILL.md and deleting `v2/`. Only the user does this, after plan approval.

Relationships:
- A **Group container** can nest other **Group containers** (Region > VPC > AZ > Subnet) and contain **Service nodes**.
- A **Service node** is always parented at `parent="1"` in cross-layer diagrams (not the container ID) to avoid edge-routing bugs inherited from v1's findings.

## docs/adr/0001-v2-promotes-by-overwriting-v1.md — content sketch

> AWS support is added by drafting the new skill inside `v2/` and later copying its contents up one level to overwrite the existing v1 SKILL.md. The alternative — creating a sibling skill `drawio-aws-diagram` — was rejected because Claude's skill discovery only sees top-level directories under `~/.claude/skills/`, two sibling skills would duplicate the wrapper/colour/layout rules, and the user explicitly wanted "an extension of the current skill", not a parallel one. The `v2/` folder is throwaway; once promoted it does not persist.

## docs/adr/0002-standardise-on-mxgraph-aws4-icon-set.md — content sketch

> All AWS service and container shapes use the `mxgraph.aws4` icon library exclusively. draw.io ships several alternative sets (`aws3` legacy, yearly snapshots `aws17`-`aws23`), but `aws4` has the widest service coverage, the most stable style-string format, and matches what nearly every draw.io AWS template online uses. Supporting multiple sets would force every cheat-sheet row to be duplicated and risk visual inconsistency within a single diagram. If AWS publishes a future iconography refresh that draw.io ships as `aws25+`, that would be a new ADR superseding this one.

## Files to be modified

None during this plan execution. After approval the user can promote v2/ contents to overwrite the v1 SKILL.md.

## Existing assets reused

- All generic element XML snippets, colour table, layout rules, mandatory rules → carried over verbatim from `drawio-diagram/SKILL.md` (mostly into REFERENCE.md, some staying in SKILL.md)
- File wrapper, core principle, output instructions → unchanged
- Edge routing rules (corridor waypoints, cross-layer routing) → unchanged; equally applicable to AWS diagrams

## Verification

After implementation:

1. Inspect file sizes: `wc -l v2/*.md` — SKILL.md should be near 100 lines, references larger.
2. Generate a sanity diagram using the new AWS-REFERENCE.md: a 3-tier web app on AWS. Save as `aws-sanity.drawio`.
3. Open `aws-sanity.drawio` in app.diagrams.net or VS Code Draw.io Integration. Confirm:
   - AWS service icons render correctly (S3 bucket icon, Lambda icon, etc. — not plain rectangles)
   - Container shapes (VPC, AZ, Subnet) render with the correct AWS dashed-boundary look
   - Edges route cleanly through corridors (no boxes crossed)
4. Regenerate a non-AWS diagram (e.g., DSA two-pointer pattern) using the same skill — confirm generic behaviour is unchanged.
5. Inspect description string in SKILL.md frontmatter — confirm all new trigger phrases are present and total length < 1024 chars.

## Promote (post-approval, by user)

```
# Move only the skill content up (SKILL.md, REFERENCE.md, AWS-REFERENCE.md, EXAMPLES.md)
cp /Users/vpandey/.claude/skills/drawio-diagram/v2/SKILL.md \
   /Users/vpandey/.claude/skills/drawio-diagram/v2/REFERENCE.md \
   /Users/vpandey/.claude/skills/drawio-diagram/v2/AWS-REFERENCE.md \
   /Users/vpandey/.claude/skills/drawio-diagram/v2/EXAMPLES.md \
   /Users/vpandey/.claude/skills/drawio-diagram/

# Optional: keep the plan/ and docs/ folders at top level for posterity (decision: yes — they're historical record)
mv /Users/vpandey/.claude/skills/drawio-diagram/v2/plan \
   /Users/vpandey/.claude/skills/drawio-diagram/plan
mv /Users/vpandey/.claude/skills/drawio-diagram/v2/docs \
   /Users/vpandey/.claude/skills/drawio-diagram/docs

rm -rf /Users/vpandey/.claude/skills/drawio-diagram/v2/
```

This replaces the existing SKILL.md, adds the three new reference files, and preserves the plan + grilling docs at the skill root. Top-level skill discovery picks up the new description + content immediately.
