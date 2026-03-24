---
name: cdc_help
description: Guided procedure for producing a CDC (Cahier des Charges) for any [SYSTEM]_help.md file. Run before help.md to ensure the documentation triad is complete. Produces a standardised CDC that help.md can consume without ambiguity.
---

# cdc_help — Generate a CDC for a [SYSTEM]_help.md

## Objective

Guide an agent through the production of a complete, standardised CDC for a given system of skills.
The CDC produced by this skill is the input that `help.md` expects to author a `[SYSTEM]_help.md` file.

---

## When to use

- Before authoring any `[SYSTEM]_help.md`
- When no CDC exists for the target system
- When an existing CDC needs to be audited or updated after skills have changed

---

## Inputs required

| Input | Source |
|-------|--------|
| System name | Provided by the user or inferred from the project folder |
| List of source files | Provided by the user, or discovered by listing the system folder |
| Target path for `[SYSTEM]_help.md` | Provided by the user |

If any input is missing, ask the user before proceeding.

---

## Procedure

### Step 1 — Identify source files

List the files provided or discovered. Confirm with the user before reading:

```
I am about to read the following files to extract triggers:
- [file 1]
- [file 2]
- ...
Shall I proceed?
```

Wait for explicit confirmation.

### Step 2 — Extract triggers

Read each source file. For each trigger found (typically defined in a `## Triggers` table or equivalent):

- Record the **trigger name** (exact string the user types, e.g. `MAIL!`, `SEND! HANDLE`)
- Record the **source file**
- Record the **raw description** from the skill (one sentence if available)

Build a consolidated trigger inventory:

| Trigger | Source file | Raw description |
|---------|-------------|-----------------|
| `TRIGGER_1` | file_a.md | … |
| `TRIGGER_2` | file_b.md | … |

### Step 3 — Detect anomalies

Flag the following before producing the CDC:

| Anomaly | Definition | Action |
|---------|------------|--------|
| **Missing description** | Trigger exists but has no description in the source skill | Flag — ask user to provide one |
| **Duplicate trigger name** | Same trigger name found in two or more source files | Flag — ask user which takes precedence, or whether both should appear |
| **Shared prefix** | Multiple triggers share a common prefix (e.g. `INVITE READ`, `INVITE WRITE`) | Note — a gateway entry for the prefix should be added in `[SYSTEM]_help.md` |
| **Undocumented behaviour** | Trigger description is ambiguous or contradictory across files | Flag — ask user to clarify |

Report all anomalies in one block and wait for resolution before continuing.

### Step 4 — Qualify the CDC metadata

Ask the user the following questions in one block:

```
A few details before I draft the CDC:

1. System name (display name for the help file): [suggested: SYSTEM]
2. Target audience: who will use HELP! in this system?
   (e.g. non-developer end user / developer / mixed)
3. Language for [SYSTEM]_help.md runtime output: [suggested: français / english]
4. Target path for the deliverable: [suggested: PATH\[SYSTEM]_help.md]
5. Anything to exclude from the help file?
   (e.g. internal utility triggers not intended for end users)
```

Wait for answers before proceeding.

### Step 5 — Draft and submit the CDC

Produce the CDC in the standard format below and display it in full for user validation.
Do not write any file until the user explicitly approves.

---

## CDC standard format

```markdown
# CDC — [SYSTEM]_help.md

## System
[Display name and short description — 2 sentences max]

## Source files
[Exhaustive list of files read to extract triggers]

## Target audience
[One sentence — who reads the HELP! output]

## Language
[Language of runtime output — e.g. français, english]

## Triggers to document

| Trigger | Source file | One-line description (end-user language) |
|---------|-------------|------------------------------------------|
| `TRIGGER_1` | file_a.md | … |
| `TRIGGER_2` | file_b.md | … |

## Shared prefixes
[List any prefix groups identified — or "none"]

## Exclusions
[Triggers explicitly excluded from the help file — or "none"]

## Anomalies resolved
[Summary of any anomalies found in step 3 and how they were resolved — or "none"]

## Deliverable
[SYSTEM]_help.md — target path: [PATH]
```

---

## Validation and handoff

Once the user approves the CDC:

1. Write the CDC file to `[PROJECT_PATH]\CDC_[SYSTEM]_help.md`
2. Confirm in chat: `CDC written to [PATH]. Ready to proceed with help.md.`
3. Do not start authoring `[SYSTEM]_help.md` — that is the responsibility of the agent running `help.md`

---

## Rules

- Never read source files without user confirmation (step 1)
- Never write the CDC file without explicit user approval (step 5)
- Never author `[SYSTEM]_help.md` directly — hand off to `help.md`
- All anomalies must be resolved before the CDC is produced
- The trigger descriptions in the CDC must be rewritten in end-user language,
  even if the source skill uses technical wording
