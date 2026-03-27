---
name: skill_AI_d
description: A three-skill kit for adding built-in contextual help to any Claude skill system. Produces a [SYSTEM]_help.md file that responds to HELP! and HELP! TRIGGER at runtime. Covers initial authoring (cdc_help, help) and incremental updates (update_help). No code required.
tags: [documentation, help-system, skills, claude-desktop, authoring]
version: 1.0
---

# skill_AI_d — Built-in help for Claude skill systems

> *Turn any system of skills into a self-documenting one.*

You build a system of skills. It works well. Then nobody — including future-you — can remember which trigger does what. **skill_AI_d** solves this by producing a single `[SYSTEM]_help.md` file that Claude can serve on demand, directly in chat.

**No code. No external tools. Just guided skill execution and Markdown output.**

---

## For humans — getting started

→ Full documentation: [README.md](README.md)
→ Source & install: [github.com/playtosor/skill_AI_d](https://github.com/playtosor/skill_AI_d)

Requirements: Claude Desktop with filesystem MCP access.

Typical workflow:
1. Run `cdc_help.md` → produces a validated specification (CDC)
2. Run `help.md` → produces `[SYSTEM]_help.md`
3. Ship your system with `[SYSTEM]_help.md` included
4. When skills evolve, run `update_help.md` → delta applied

---

## For agents — quick reference

Load the relevant skill from your skill_AI_d folder:

- `cdc_help.md` — guided procedure to produce the specification before authoring
- `help.md` — structure convention + authoring of `[SYSTEM]_help.md`
- `update_help.md` — delta detection and surgical update after system changes

### Triggers

| Trigger | Skill | What it does |
|---------|-------|--------------|
| `UPDATE HELP?` | update_help.md | Quick scan — signals what has changed, no file written |
| `UPDATE HELP!` | update_help.md | Full delta analysis, validation, and update |

*cdc_help.md is guided — no fixed trigger. Run it by reading the skill and following the steps.*

---

## Output

Running skill_AI_d on a system produces one file: `[SYSTEM]_help.md`.
Reference implementation: [`m_AI_l_help.md`](https://github.com/playtosor/m_AI_l) — the help file for the m_AI_l messaging system.
<!-- END -->
