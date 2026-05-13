---
name: software-docs
description: Expert skill for writing high-quality software documentation including PRDs, Design Docs, RFCs, ADRs, User Guides, Runbooks, Changelogs, Release Notes, and Postmortems. Use when user asks to write, create, or draft any technical or product document. Triggers: "write a PRD", "create design doc", "draft RFC", "write ADR", "create runbook", "write release notes", "create changelog", "write postmortem", "write user guide", "document this feature", "create incident report".
---

# Software Docs Skill

## Guiding Principles

- **Max 1 page / ~6 min read** — trim ruthlessly, link to details
- **Audience-first** — tune every doc to its primary reader (see [AUDIENCES.md](AUDIENCES.md))
- **Interview before writing** — gather context, write rough draft, iterate
- **No filler** — every sentence earns its place
- **Markdown only** — use headers, tables, code blocks; no emojis unless audience is internal-casual

---

## Workflow

```
1. Identify doc type → load template from templates/
2. Interview user (round 1, 3–5 targeted questions)
3. Write rough draft
4. Ask open questions BEFORE finalising
5. Resolve open questions with user
6. Iterate with user until approved
```

### Step 1 — Identify doc type

Ask: "What are you building/shipping/explaining?" then map to one of:

| Type | When to use |
|------|-------------|
| PRD | New feature or product initiative — defines *what* and *why* |
| Design Doc | Technical approach — defines *how* |
| RFC | Cross-team proposal needing community consensus |
| ADR | Record of a one-way architectural decision |
| User Guide | End-user instructions (internal or external) |
| Runbook | Ops procedures for recurring tasks or incidents |
| Release Notes | What changed in a version, for users |
| Changelog | Developer-facing version history |
| Postmortem | Incident retrospective — blameless, action-focused |

### Step 2 — Interview (round 1)

Ask only what's missing. Standard questions per type are in each template.
3–5 questions max per round. Don't ask what you can infer.

### Step 3 — Write rough draft

Use the matching template. Fill every section — write `TBD` rather than omitting.
Flag assumptions with `> **Assumption:** ...`

### Step 4 — Ask open questions BEFORE finalising

**MANDATORY: Before presenting the doc as complete, surface every open question to the user and get answers.**

- Identify all `[ ]` open questions in the draft
- Present each question with a brief explanation of *why* it matters and what the tradeoffs are
- Wait for the user to answer all questions
- Only then mark them resolved with `[x]` and include the rationale in the answer

Do NOT skip this step or leave open questions unresolved. A doc with unresolved `[ ]` items is not done.

### Step 5 — Resolve and finalise

Update each resolved question with `[x]` + the decision + the reason (1–2 sentences). This gives future readers the context behind the decision, not just the outcome.

### Step 6 — Iterate

Present diff of changes. Stop when user says "looks good" or "ship it".

---

## Templates

| Type | File |
|------|------|
| PRD | [templates/PRD.md](templates/PRD.md) |
| Design Doc | [templates/DESIGN-DOC.md](templates/DESIGN-DOC.md) |
| RFC | [templates/RFC.md](templates/RFC.md) |
| ADR | [templates/ADR.md](templates/ADR.md) |
| User Guide | [templates/USER-GUIDE.md](templates/USER-GUIDE.md) |
| Runbook | [templates/RUNBOOK.md](templates/RUNBOOK.md) |
| Release Notes | [templates/RELEASE-NOTES.md](templates/RELEASE-NOTES.md) |
| Changelog | [templates/CHANGELOG.md](templates/CHANGELOG.md) |
| Postmortem | [templates/POSTMORTEM.md](templates/POSTMORTEM.md) |

---

## Audience Tone Guide

See [AUDIENCES.md](AUDIENCES.md) for full guide. Quick reference:

| Reader | Tone | What they care about |
|--------|------|----------------------|
| Eng (internal) | Technical, precise, terse | Architecture, edge cases, tradeoffs |
| Data Analyst | Plain language, examples | What changed, how it affects their work |
| Exec | Business framing, top-line only | Impact, cost, risk, timeline |
| External user | Clear, friendly, task-focused | How to use it, what to do when it breaks |

---

## Quality Checklist (run before marking done)

- [ ] Under 6 min read (~1200 words)
- [ ] Audience is explicit in doc header
- [ ] No section is vacuous ("we will monitor the situation")
- [ ] Every action item has an owner + date
- [ ] No jargon without definition for non-eng audience
- [ ] Links to supporting docs rather than inlining
