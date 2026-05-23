# Plan Mode Behavior

**Applies when:**
- Operating in plan mode (EnterPlanMode was called this session), OR
- `superpowers:writing-plans` skill has just completed drafting a plan

---

## Steps

### 1. Derive filename
Auto-derive from plan title in kebab-case (e.g. `auth-token-refresh.md`).
Ask only if title is genuinely ambiguous.

### 2. Detect save location
Run: `git rev-parse --show-toplevel` to find project root.

- If no git root detected → save to `~/.claude/plans/<feature-name>.md` directly
- If `plans/` exists at project root → save to `<root>/plans/<feature-name>.md`
- If `plans/` does not exist at project root → ask:
  > "No `plans/` folder found. Create `plans/` here and save, or save to `~/.claude/plans/`?"

### 3. Save the plan file
Write full plan content to chosen path.
Announce: `Plan saved to <path>`

### 4. Offer execution options

> **Execution options:**
> 1. **Subagent-Driven** *(recommended)* — fresh subagent per task, 2-stage review (spec + quality). Uses `superpowers:subagent-driven-development`
> 2. **Inline Execution** — executing-plans in this session with checkpoints. Uses `superpowers:executing-plans`
> 3. **Fresh Session** — start a new session, invoke `superpowers:executing-plans` with plan: `<saved-path>`

### 5. Exit plan mode *(plan mode only)*
If EnterPlanMode was called this session, call ExitPlanMode after offering options.
Do NOT wait for user to choose an execution option before exiting.
