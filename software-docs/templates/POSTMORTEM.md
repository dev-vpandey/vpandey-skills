# Postmortem: [Incident Title]

**Date of incident:** [YYYY-MM-DD]
**Duration:** [HH:MM] — [start time] to [end time] [timezone]
**Severity:** P0 | P1 | P2 | P3
> P0 = total outage, P1 = major degradation, P2 = partial impact, P3 = minor
**Status:** Draft | In Review | Complete
**Author(s):** [Names]
**Audience:** Engineering, Leadership

> **Blameless principle:** This document focuses on systems and processes, not individuals. The goal is to learn and prevent, not assign fault.

---

## TL;DR

> Exec-readable. 3 bullets max. Business framing.

- **What happened:** [One sentence]
- **Impact:** [Users/systems affected, duration]
- **Root cause:** [One sentence — the system failure, not the person]

---

## Timeline

All times in [timezone].

| Time | Event |
|------|-------|
| HH:MM | [First signal — alert / report] |
| HH:MM | [On-call paged / investigation started] |
| HH:MM | [Root cause identified] |
| HH:MM | [Mitigation applied] |
| HH:MM | [Incident resolved / service restored] |
| HH:MM | [Post-incident monitoring period ended] |

---

## Impact

**Users affected:** [Number or percentage]
**Systems affected:** [List]
**Data affected:** [None | describe]
**Duration:** [Total downtime or degradation window]
**Revenue / SLA impact:** [Estimate if applicable]
**Detected by:** Alert | User report | Manual check | External report
**Detection lag:** [Time from incident start to first detection]

---

## Root Cause

One paragraph. What was the actual technical cause? Be precise — name the component, the condition, the failure mode.

> **Assumption:** [If root cause is not fully confirmed, flag it here]

---

## Contributing Factors

What made this incident more likely or harder to detect?

- [Factor 1 — e.g., monitoring gap]
- [Factor 2 — e.g., deployment without staged rollout]
- [Factor 3 — e.g., config change not reviewed]

---

## What Went Well

- [Detection was fast because X]
- [Rollback procedure worked as expected]
- [Team communication was clear]

---

## Near Misses

What could have been worse — and why it wasn't?

- [Near miss 1 — what almost happened and what prevented it]

---

## What Could Have Gone Better

- [Alert threshold too high — delayed detection by 20 min]
- [Runbook was outdated — eng had to improvise]
- [No staging environment for this service]

---

## Action Items

> Every action item needs an owner, a due date, and a trackable ticket.

| Action | Owner | Due | Ticket | Status |
|--------|-------|-----|--------|--------|
| [Specific fix] | [Name] | [YYYY-MM-DD] | [link] | Open |
| [Add alert for X] | [Name] | [YYYY-MM-DD] | [link] | Open |
| [Update runbook] | [Name] | [YYYY-MM-DD] | [link] | Open |

---

## Lessons Learned

2–3 takeaways that apply beyond this specific incident.

1. [Lesson]
2. [Lesson]

---

## References

- [Incident ticket](link)
- [Related runbook](link)
- [Monitoring dashboard](link)

---

### Interview Questions (remove before publishing)

> 1. When did the incident start and end? What was the first signal?
> 2. What was the actual root cause — what broke and why?
> 3. What was the business/user impact (scope, duration)?
> 4. What contributed to the incident or slowed the response?
> 5. What are the top 3 concrete action items with owners?
