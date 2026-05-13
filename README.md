# vpandey-skills

A collection of Claude Code skills (slash commands) for daily engineering, planning, and writing workflows.

If this saves you time, a ⭐ on the repo helps others find it.

---

## What are skills?

Skills are markdown instruction files that Claude Code loads on demand. Each skill lives in its own folder with a `SKILL.md` file. When invoked, the skill's content is injected into the Claude session to guide behavior.

---

## Setup

### 1. Prerequisites

- [Claude Code CLI](https://docs.anthropic.com/claude-code) installed and configured

### 2. Clone this repo

```bash
git clone <repo-url> ~/Documents/git_personal/vpandey-skills
```

### 3. Register skills

Symlink each skill folder into `~/.claude/skills/`:

```bash
mkdir -p ~/.claude/skills

for skill_dir in ~/Documents/git_personal/vpandey-skills/*/; do
  skill_name=$(basename "$skill_dir")
  ln -sf "$skill_dir" ~/.claude/skills/"$skill_name"
done
```

### 4. Verify

```bash
ls ~/.claude/skills/
# caveman  drawio-diagram  forge  grill-me  humanizer  software-docs  write-a-skill  zoom-out
```

---

## Skills

### `caveman`
Ultra-compressed communication. Cuts token usage ~75% by stripping filler while keeping technical accuracy.

```
/caveman          # default: lite
/caveman full     # classic caveman fragments
/caveman ultra    # maximum compression, arrows for causality
```

---

### `drawio-diagram`
Generates `.drawio` XML files for any visual — system design, class diagrams, flowcharts, architecture. Open the output by dragging onto [app.diagrams.net](https://app.diagrams.net) or in VS Code with the Draw.io Integration extension.

---

### `forge`
Life-coach planning skill. Auto-detects mode from the current date.

```
/forge        # auto: day / week / month
/forge day    # daily review — wins, drains, 3 priorities
/forge week   # Sunday week plan — DSA, system design, article
/forge month  # last-Sunday month plan — campaign check, burnout audit
```

---

### `grill-me`
Relentlessly interviews you about a plan or design, walking every branch of the decision tree. Recommended answer included per question.

---

### `humanizer`
Removes AI-generated writing patterns. Runs a two-pass rewrite + self-audit + diff table. Accepts inline text, file paths, or a voice sample for calibration.

---

### `software-docs`
Writes high-quality technical and product documents. Interviews you before drafting, surfaces open questions before finalizing.

Supported types: PRD · Design Doc · RFC · ADR · User Guide · Runbook · Release Notes · Changelog · Postmortem

---

### `write-a-skill`
Scaffolds a new skill — `SKILL.md`, optional reference files, optional utility scripts.

---

### `zoom-out`
Tells Claude to go up a layer of abstraction and map all relevant modules, callers, and domain vocabulary. Use when unfamiliar with a section of code.

---

## Adding a new skill

```
vpandey-skills/
└── my-skill/
    └── SKILL.md
```

Minimal `SKILL.md`:

```markdown
---
name: my-skill
description: What it does. Use when [specific trigger phrase].
---

# My Skill

[instructions here]
```

Then symlink it:

```bash
ln -sf ~/Documents/git_personal/vpandey-skills/my-skill ~/.claude/skills/my-skill
```
