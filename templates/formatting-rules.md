# Formatting Rules

## Tables — Unicode box-drawing only, never ASCII

- All tables in any output must use Unicode box-drawing characters, never ASCII pipes (`|`, `-`, `+`).
- Headers must always be **inside** the box using a `╠═╬═╣` separator row — never a separate markdown pipe row above the box.

```
╔══════════════╦══════════╗
║ Column A     ║ Column B ║
╠══════════════╬══════════╣
║ value        ║ value    ║
╚══════════════╩══════════╝
```

This applies to all tables: Mental Model, Complexity, Dry Run, comparisons, and any ad-hoc tables in explanations.
