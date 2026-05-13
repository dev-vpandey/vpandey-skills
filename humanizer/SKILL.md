---
name: humanizer
version: 1.0.0
description: |
  Remove AI-generated writing patterns from text or files to make them sound natural and human-written.
  Use when user says "humanize this", "humanise this", "make it more readable", "no AI talk",
  "remove AI patterns", "make this sound human", or pastes text that sounds AI-generated.
  Loads full 29-pattern reference + examples on every humanize task for best output quality.
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - AskUserQuestion
---

# Humanizer

## On every humanize task

**First:** Read the full reference:
```
Read: `~/.claude/skills/humanizer/humaniser_ref.md`
```

Then run the two-pass process below.

## Input modes

- **Inline text:** user pastes text directly
- **File path:** read the file, humanize, ask overwrite or new file
- **Voice sample provided:** analyze sample first (see Voice Calibration in reference), then rewrite matching their patterns

## Content preservation rule (non-negotiable)

**Never omit, merge away, or silently drop any section, table, field, or data point from the original.**

- Every section heading must appear in the output
- Every table must appear with all its columns and rows intact
- Every field in headers/metadata must be preserved (e.g. Audience, Version, Status)
- Specific named values — owners, dates, URLs, file paths, code snippets, metric targets — must not be paraphrased away
- Structured data (tables, lists with items) must stay structured; do not collapse into prose
- The only exception: decorative placeholders with no content (e.g. an image tag with no caption)

If you catch yourself merging two sections or dropping a table "for brevity" — stop. Humanize the words, not the structure.

## Two-pass process

### Pass 1 — Rewrite

Fix all patterns from the reference (29 documented + behavioral tells below).

Also inject personality: vary sentence rhythm, use opinions, acknowledge complexity, use "I" when it fits, be specific about feelings.

**While rewriting, keep a mental checklist:** every section from the original must appear in the draft before moving to Pass 2.

### Pass 2 — Self-audit

After draft, ask: *"What makes this so obviously AI generated?"*
List remaining tells. Revise to fix them. Present final version.

Also ask: *"Did I drop or merge any section, table, or field from the original?"*
If yes, restore it before presenting the final.

## Output format

```
**Draft:**
[rewritten text]

**Still AI-sounding because:**
- [tell 1]
- [tell 2]

**Final:**
[revised text]
```

Skip audit section if no remaining tells.

## Diff table (mandatory, always last)

After presenting the final version, always output a diff table showing every change made. Use Unicode box-drawing characters.

```
╔══════════════════════╦══════════════════════════════════╦══════════════════════════════════╗
║ Location             ║ Original                         ║ Humanized                        ║
╠══════════════════════╬══════════════════════════════════╬══════════════════════════════════╣
║ [section — context]  ║ [exact original phrase/value]    ║ [what it became]                 ║
╚══════════════════════╩══════════════════════════════════╩══════════════════════════════════╝
```

Rules:
- One row per distinct change — phrase swap, word removal, structural change, case change
- "Location" = section name + brief context (e.g. "TL;DR — Why now bullet")
- "Original" = exact words from source, not a summary
- "Humanized" = exact words in final output, not a summary
- If a value was preserved unchanged, do not list it
- End the table with a one-line summary: total change count and characterization (e.g. "12 changes — all minor phrasing, no content altered")

## Behavioral tells (not in the 29)

These are subtler patterns the 29 don't cover — check for all of them too:

- **No contractions** — AI writes "it is" / "do not" in casual text. Use "it's" / "don't" where natural.
- **Exhaustive coverage** — AI lists every angle even when 2-3 suffice. Cut to what matters.
- **Transition spam** — "Furthermore", "Moreover", "In addition" chaining. Cut or vary.
- **Artificial balance** — weak "other side" added just to seem fair. Remove if hollow.
- **Perfect grammar paralysis** — no self-corrections, no mid-thought pivots. Let some mess in.
- **Sandwich structure** — intro restates what body says, conclusion restates body again. Cut the restatements.
