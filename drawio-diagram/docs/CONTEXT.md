# drawio-diagram skill

A Claude agent skill that generates `.drawio` XML files for any visual, including AWS cloud architectures with native AWS iconography. This document captures the language used inside the skill so future edits stay coherent.

## Language

**Skill**:
A Claude agent capability that lives in a top-level directory under `~/.claude/skills/` and is auto-discovered via its `SKILL.md`. Subdirectories are not discovered as skills.
_Avoid_: plugin, extension, agent.

**Service node**:
A single draw.io `mxCell` representing one AWS service. Uses `shape=mxgraph.aws4.resourceIcon` with a `resIcon=` selector and a category fill colour.
_Avoid_: AWS box, service shape, AWS rectangle.

**Group container**:
A draw.io `mxCell` representing an AWS scope boundary (Region, VPC, AZ, Subnet, Account). Uses `shape=mxgraph.aws4.group` with a `grIcon=` selector. AWS-specific; visually distinct from a generic swimlane.
_Avoid_: zone, swimlane (those mean the generic non-AWS container).

**`resIcon`**:
The style attribute selecting the AWS service icon. Example: `resIcon=mxgraph.aws4.simple_storage_service_s3`. The shape name often differs from the common service name.

**`grIcon`**:
The style attribute selecting the AWS group-container variant. Example: `grIcon=mxgraph.aws4.group_vpc`.

**Corridor**:
The empty horizontal band (≈20–30px) between adjacent swimlanes or group containers, used as the routing channel for cross-layer edges. Edges that need to traverse the corridor declare explicit `Array as="points"` waypoints inside it.
_Avoid_: gutter, gap, margin.

**Promote**:
The one-time act of copying `v2/SKILL.md`, `v2/REFERENCE.md`, `v2/AWS-REFERENCE.md`, `v2/EXAMPLES.md` up one directory level (overwriting the v1 SKILL.md), moving `v2/plan/` and `v2/docs/` up alongside them, then deleting the `v2/` folder. Performed only by the user, after plan approval.

## Relationships

- A **Group container** may nest other **Group containers** (Region > VPC > AZ > Subnet) and contain **Service nodes**.
- A **Service node** in a cross-layer diagram always sets `parent="1"`, never the container ID — child-parented nodes cause draw.io to snap edges to the container boundary instead of the node centre.
- An edge that stays within one **Group container** (or one swimlane) uses auto-routing via `exitX/exitY/entryX/entryY`. An edge that crosses containers routes through the **Corridor** with explicit waypoints.

## Example dialogue

> **User:** "Draw the AWS architecture for a 3-tier web app."
> **Skill:** Places a **Group container** with `grIcon=group_region`, nests a `group_vpc` inside, then two `group_availability_zone` containers, each with `group_public_subnet` + `group_private_subnet`. Drops **Service nodes** (CloudFront, ALB, ECS Fargate, RDS) at `parent="1"` with x/y coordinates placing them visually inside the containers. Routes edges across the **Corridor**.

## Flagged ambiguities

- "Container" was overloaded — could mean a generic swimlane or an AWS group container. Resolved: **Group container** is AWS-specific (uses `mxgraph.aws4.group`); the generic kind stays "swimlane".
- "AWS shape" was vague — could mean a service icon or a scope boundary. Resolved: **Service node** for services, **Group container** for scope boundaries.
