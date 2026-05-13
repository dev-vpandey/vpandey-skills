# Audience Guide

## Detecting Audience

If not stated, infer from doc type:

| Doc type | Default audience |
|----------|-----------------|
| PRD | Eng + Data Analyst + Exec |
| Design Doc | Eng |
| RFC | Eng |
| ADR | Eng |
| User Guide | Depends — ask |
| Runbook | Eng (on-call) |
| Release Notes | All |
| Changelog | Eng |
| Postmortem | Eng + Exec |

Always state audience at top of doc: `**Audience:** Engineering, Data Analysts`

---

## Tone by Audience

### Engineering (internal)
- Terse, precise
- Technical terms fine without definition
- Show tradeoffs explicitly
- Link to code/PRs/tickets

### Data Analysts (internal, non-eng)
- Plain English
- Avoid acronyms — or define on first use
- Lead with "what changed for you" before the why
- Use concrete examples with real column names / pipeline names they know

### Exec / Leadership
- Lead with business impact (cost, revenue, risk, timeline)
- One sentence per point
- No implementation details unless asked
- Status should be unambiguous: on track / at risk / blocked

### External Users
- Task-oriented ("To do X, click Y")
- Friendly but not casual
- Assume no prior context
- Always include "what to do if this doesn't work"

---

## Multi-Audience Docs (PRD, Release Notes, Postmortem)

Structure the doc with a **TL;DR** at top (exec-readable) followed by detail sections for eng.

Pattern:
```
## TL;DR
2–3 bullet points. Business framing. No jargon.

## Detail
Full technical content for eng.
```
