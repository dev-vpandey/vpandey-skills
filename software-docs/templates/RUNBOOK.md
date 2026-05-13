# Runbook: [Operation Name]

**Owner:** [Team / Name]
**Last tested:** [YYYY-MM-DD]
**Last verified by:** [Name]
**Audience:** Engineering (on-call)
**Estimated time:** [X minutes]
**Trigger:** [When to run this — alert name, schedule, or incident condition]
**Risk level:** Low | Medium | High
**Requires approval:** Yes (from [role]) | No

---

## Purpose

One sentence. What does this runbook accomplish?

---

## Prerequisites

- [ ] Access to [system]
- [ ] [Tool] installed (`brew install x`)
- [ ] [Permission or role required]

---

## ⚠ Do NOT

- Do not run step X without completing step Y first
- Do not run in production before validating in staging
- [Other footgun specific to this operation]

---

## Steps

> Run each step in order. Do not skip steps.

### Step 1: [Name]

```bash
# Command to run
```

Expected output:
```
[What you should see]
```

If you see `[error]` instead: → Go to [Troubleshooting > Error X](#error-x)

---

### Step 2: [Name]

```bash
# Command to run
```

Expected output:
```
[What you should see]
```

---

### Step 3: Verify

```bash
# Verification command
```

Success condition: [What indicates the operation succeeded]

---

## Rollback

If anything goes wrong, undo with:

```bash
# Rollback command
```

---

## Troubleshooting

### Error: [Error message or symptom] {#error-x}

**Cause:** [Why this happens]

**Fix:**
```bash
# Fix command
```

### Error: [Another error]

**Cause:** [Why this happens]

**Fix:** [What to do]

---

## Post-Run Checklist

| Item | Where | Done? |
|------|-------|-------|
| Log run result | [Confluence / Jira link] | [ ] |
| Notify channel | [#ops-alerts or relevant channel] | [ ] |
| Update incident ticket | [Link or template] | [ ] |
| Notify team if incident response | [Team / Slack] | [ ] |

---

## Escalation

If this runbook doesn't resolve the issue:

1. Page [on-call rotation / person]
2. Post in [#channel]
3. Open ticket: [link or template]

---

### Interview Questions (remove before publishing)

> 1. What operation or incident does this cover?
> 2. Who runs this — on-call eng, any eng, specific team?
> 3. What are the exact commands involved?
> 4. What does success look like vs. failure?
> 5. What is the rollback procedure?
