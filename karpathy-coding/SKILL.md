---
name: karpathy-coding
description: Behavioral discipline rules for writing and modifying code. Use when fixing bugs, implementing features, refactoring, adding features, or editing/modifying source code files. Do NOT use for explaining code, reviewing PRs, research, writing docs, or Confluence tasks. Rigid — follow all rules exactly.
---

# Karpathy Coding Guidelines

**Rigid skill. Apply all 9 rules to every write/modify task.**

## Rules

**1. Think Before You Code**
State assumptions. Ask when ambiguous. Present tradeoffs. If request seems mistaken/inefficient/overcomplicated, say so. Recommend simpler solution if one exists. Stop and explain what is unclear. Don't act certain when uncertain.

**2. Keep Solution Simple**
Minimum code. No unasked features, abstractions, configurability, or defensive handling. Prefer simple readable over clever. Ask: smallest change? Over-engineered? If yes, simplify.

**3. Stay Strictly Within Scope**
Only change what task requires. No unrelated refactors, rewrites, or style fixes. Match existing style and conventions. If you notice unrelated problems, mention them separately — don't change them. Every changed line justifiable from the request.

**4. Make Surgical Diffs**
Fewest files. Fewest lines. Targeted fix > broad rewrite. Preserve existing structure. Remove only dead code/imports/variables created by your own changes. Don't delete pre-existing unused code.

**5. Work Toward Verifiable Outcomes**
"Fix bug" → reproduce, fix, verify. "Refactor" → tests still pass. "Add validation" → test invalid input. "Optimize" → improve performance without changing correctness. Multi-step: short plan with verification points. Prefer tests and existing checks over verbal confidence.

**6. Read Before You Write**
Read nearby code first. Identify local conventions before introducing new patterns. Don't infer architecture from one file. If context missing, say so. Don't patch blindly.

**7. Preserve Intent**
Keep comments unless outdated and directly affected. Preserve behavior/interfaces unless changing them is the goal. Call out intentional changes explicitly. Don't make hidden product or design decisions on the user's behalf.

**8. Ask for Help at the Right Time**
Pause when: ambiguous in a way that affects implementation, conflicting patterns, correct behavior is unclear, product/arch decision needed, tradeoffs need user approval. Don't fabricate certainty to stay moving.

**9. Final Check Before Finish**
Confirm: request addressed, change no larger than needed, unrelated code untouched, assumptions surfaced, tests run, final result matches requested scope. Can't verify something? Say so.
