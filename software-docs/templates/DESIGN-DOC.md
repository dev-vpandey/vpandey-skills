# Design Doc: [Feature / System Name]

**Author:** [Name]
**Date:** [YYYY-MM-DD]
**Status:** Draft | In Review | Approved
**Audience:** Engineering
**PRD:** [link]
**Reviewers:** [Names]
**Approved by:** [Name, Date]

---

## Version History

| Version | Date | Author | Summary of changes |
|---------|------|--------|--------------------|
| 0.1 | [YYYY-MM-DD] | [Name] | Initial draft |

---

## Problem

One paragraph. What doesn't work today and why does it matter technically?

---

## Goal

One sentence. What does this design achieve?

## Non-Goals

- [Explicitly out of scope]

---

## Architecture

High-level diagram or ASCII flow. Link to draw.io if complex.

```
[Source] → [Component A] → [Component B] → [Sink]
```

**Deployment:** [where — K8s, Lambda, on-prem, etc.]
**Data flow:** sync | async | event-driven
**External dependencies:** [list]

### Components

Brief description of each new or changed component. One paragraph max per component.

**Component A**
What it does, what it owns, what it calls.

---

## Key Decisions

For each non-obvious choice, explain what was considered and why this was picked.

### Decision 1: [Short title]

**Options considered:**
- Option A — [pros / cons]
- Option B — [pros / cons]

**Chosen:** Option A. Reason: [1–2 sentences].

---

## Data Model

Only if schema changes. Show before/after or new schema.

```sql
-- New table / field
```

---

## API / Interface Changes

Endpoint, function signature, or CLI flag changes. Before/after if modifying existing.

---

## Security & Access

- **Auth mechanism:** [JWT / API key / IAM / none]
- **PII / sensitive data:** [yes/no — what]
- **Threat surface changes:** [yes/no — what]

---

## Observability

3 bullets max per sub-section.

- **Metrics:** [what to instrument]
- **Logs:** [key events to log]
- **Alerts:** [thresholds / on-call implications]

---

## Performance Envelope

- **Expected throughput:** [req/s, events/day, etc.]
- **Latency SLO:** [p99 target]
- **Scale ceiling:** [max before re-design needed]

---

## Testing Strategy

- **Unit:** [what is unit-tested]
- **Integration:** [what systems are tested together]
- **Load / stress:** [yes/no — tool, threshold]
- **Rollout gate:** [criteria to proceed from phase 1 → 2]

---

## Error Handling

| Scenario | Behavior |
|----------|----------|
| [Failure case] | [What happens] |

---

## Rollout Plan

- Phase 1: [What, who, when]
- Rollback: [How to revert]

---

## Open Questions

- [ ] [Question] — owner: [Name], needed by: [Date]

---

## References

- [PRD](link)
- [Related ADR](link)

---

### Interview Questions (remove before publishing)

> 1. What are the main components or systems involved?
> 2. What are the 1–2 hardest technical problems to solve?
> 3. Any non-obvious constraints (latency, cost, compliance)?
> 4. What did you consider and rule out?
> 5. How will you roll this out and roll back if needed?
