# skill_AI_d

> *A three-skill kit for adding built-in contextual help to any Claude skill system.*

---

## The problem

You build a system of skills for Claude. It works well. Then someone — maybe you, a few weeks later — can't remember which trigger does what, how to start the documentation process, or whether `SEND!` and `SEND?` behave the same way.

The skills are the source of truth. But reading three `.md` files to answer a simple question is friction nobody needs.

**skill_AI_d** solves this by turning your skills into a self-documenting system — one where Claude can answer `HELP!` on the spot, without you leaving the chat.

---

## What it does

skill_AI_d is a kit of three Claude skills that work together:

**1. Define the help structure** — `help.md` establishes the convention: what a `[SYSTEM]_help.md` file looks like, how `HELP!` and `HELP! TRIGGER` behave at runtime, and how to handle coexistence when multiple help systems are loaded simultaneously.

**2. Build the help file** — `cdc_help.md` guides you through producing a specification (CDC) for any system, extracting all triggers, detecting anomalies, and getting your approval before anything is written.

**3. Keep it up to date** — `update_help.md` detects what has changed since the last documentation pass and applies a surgical, validated delta — without rewriting from scratch.

---

## The output

Running skill_AI_d on a system produces a single `[SYSTEM]_help.md` file. Once loaded into a Claude session, it responds to two universal triggers:

| Trigger | Behaviour |
|---|---|
| `HELP!` | Displays system overview and full trigger list |
| `HELP! TRIGGER` | Displays the detailed entry for a specific trigger |

No file paths. No internal jargon. End-user language — readable by both humans and Claude.

---

## Who is this for?

**skill_AI_d is designed for developers who build systems of skills for Claude** — whether for personal use, team tooling, or public release.

If you've ever shipped a skill system and realised users (including future-you) had no way to discover what it could do without reading the source, this kit is for you.

*skill_AI_d was used to document [m_AI_l](https://github.com/playtosor/m_AI_l) — an asynchronous messaging system for multi-thread Claude projects. `m_AI_l_help.md` is the reference implementation of what this kit produces.*

---

## How it works

```
skill_AI_d/
├── help.md          ← Convention: structure + runtime behaviour of [SYSTEM]_help.md
├── cdc_help.md      ← Guided procedure: produce the specification before writing
└── update_help.md   ← Delta detection and surgical update after system changes
```

Typical workflow:

```
1. Run cdc_help.md → produces CDC_[SYSTEM]_help.md (your specification)
2. Run help.md     → produces [SYSTEM]_help.md (your help file)
3. Ship your system with [SYSTEM]_help.md included
4. When skills evolve, run update_help.md → delta applied, CDC regenerated
```

---

## The skills

| Skill | Trigger(s) | What it does |
|---|---|---|
| `help.md` | `HELP!` / `HELP! TRIGGER` | Runtime convention — defines how help files are structured and served |
| `cdc_help.md` | *(guided, no fixed trigger)* | Produces a validated specification before authoring begins |
| `update_help.md` | `UPDATE HELP?` / `UPDATE HELP!` | Scans for changes and applies a delta to an existing help file |

---

## Key design decisions

**Validation before writing.** Every skill in this kit requires explicit user approval before touching any file. The CDC is shown for approval before authoring. The delta is shown for approval before applying. Nothing is written silently.

**Surgical updates.** `update_help.md` never rewrites unchanged entries. It identifies added, removed, and modified triggers precisely — and applies only what changed.

**Separation of specification and authoring.** `cdc_help.md` produces a CDC. `help.md` consumes it. Two separate agents can handle documentation and implementation independently.

**End-user language enforced.** All `[SYSTEM]_help.md` output is written in plain language — no file paths, no variable names, no internal architecture. The help file is for users, not maintainers.

---

## Getting started

> **Requires Claude Desktop** with filesystem MCP access — skill_AI_d reads and writes local `.md` files.

Load the kit into your Claude session:

```
Read [PATH]/skill_AI_d/cdc_help.md and [PATH]/skill_AI_d/help.md
```

Then point it at your system:

```
I want to document my system. The source skills are in [PATH]/my_system/skills/
```

Claude will walk you through the rest.

To update an existing help file after changes:

```
Read [PATH]/skill_AI_d/update_help.md
UPDATE HELP?
```

---

## License

Licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — use it, adapt it, build on it. Just keep the credit.

---

*A project by a one-person multinational.* 🌍
