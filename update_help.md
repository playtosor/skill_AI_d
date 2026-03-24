---
name: update_help
description: Detects changes between a published [SYSTEM]_help.md and the current state of the source skills, then applies a validated delta to bring the help file up to date. Run after any evolution of a documented system. Companion to help.md and cdc_help.md.
---

# update_help — Update a [SYSTEM]_help.md after system changes

## Objective

Propagate changes from evolved source skills into an existing `[SYSTEM]_help.md`,
without rewriting from scratch. Surgical, validated, traceable.

---

## When to use

- After publishing a new version of a system (new triggers, modified descriptions, removed triggers)
- When `[SYSTEM]_help.md` is known or suspected to be out of sync with the source skills
- When new skill files have been added to the system folder since last documentation

---

## Triggers

| Trigger | Behaviour |
|---------|-----------|
| `UPDATE HELP?` | Quick scan using timestamps + folder diff — signals what has changed, no file written |
| `UPDATE HELP!` | Full procedure: delta analysis, validation, update of `[SYSTEM]_help.md` and CDC |

---

## Reference model

The skill determines its reference for the previous state as follows:

1. **If a CDC exists and is more recent than all source skill files** → use the CDC as reference (it is reliable)
2. **Otherwise** → use `[SYSTEM]_help.md` directly as reference (the CDC may have drifted)

In both cases, the source skill **folder** is scanned in full — not just the files previously known.
This ensures newly added skill files are always detected.

---

## Trigger: UPDATE HELP?

A fast, non-destructive scan. No files are written.

### Step 1 — Timestamp pre-scan
Read file metadata (`lastModified`) for:
- `[SYSTEM]_help.md`
- All files in the source skills folder

Flag any source file more recent than `[SYSTEM]_help.md` as **potentially changed**.
Flag any source file absent from the CDC (or from `[SYSTEM]_help.md` trigger inventory) as **potentially new**.

### Step 2 — Report
Display a summary:

```
== QUICK SCAN — [SYSTEM] ==

[SYSTEM]_help.md last updated : YYYY-MM-DD HH:MM

Potentially changed skills (modified after help file) :
  ~ file_x.md     (YYYY-MM-DD HH:MM)
  ~ file_y.md     (YYYY-MM-DD HH:MM)

Potentially new skills (not previously documented) :
  + file_z.md

Skills apparently unchanged :
  [list]

Note: timestamps indicate changes — actual trigger diff requires UPDATE HELP!
```

If nothing is newer than `[SYSTEM]_help.md`:
```
No source file is more recent than [SYSTEM]_help.md.
Run UPDATE HELP! to confirm with a full trigger comparison.
```

---

## Trigger: UPDATE HELP!

Full procedure — executes all steps below.

### Step 1 — Confirm inputs

Confirm with the user:
1. Path to the existing `[SYSTEM]_help.md`
2. Source skills folder to scan (full scan — not a pre-selected list)
3. Reference to use: CDC (if recent and reliable) or `[SYSTEM]_help.md` directly

If ambiguous, apply the reference model above and inform the user of the choice made.

### Step 2 — Full folder scan

List all `.md` files in the source skills folder.
Compare against the files known from the reference (CDC or `[SYSTEM]_help.md`).

Flag:
- **New files** — present in folder, absent from reference → read and extract triggers
- **Removed files** — present in reference, absent from folder → their triggers are candidates for removal
- **Existing files** — present in both → read and compare triggers

### Step 3 — Build the trigger delta

Extract triggers from all source files (new and existing).
Compare against triggers documented in the reference.

Classify every difference:

| Change type | Definition |
|-------------|------------|
| **Added** | Trigger present in source skills, absent from reference |
| **Removed** | Trigger present in reference, absent from source skills |
| **Modified** | Trigger present in both, description or behaviour has changed |
| **Unchanged** | Trigger identical in both — no action needed |

Display the full delta report:

```
== DELTA REPORT — [SYSTEM] ==

NEW SOURCE FILES DETECTED (n)
  + file_z.md → triggers found : TRIGGER_E, TRIGGER_F

ADDED TRIGGERS (n)
  + TRIGGER_A     [source: file_x.md] — [raw description from skill]
  + TRIGGER_E     [source: file_z.md] — [raw description from skill]

REMOVED TRIGGERS (n)
  - TRIGGER_C     [was: one-line description from reference]

MODIFIED TRIGGERS (n)
  ~ TRIGGER_D     [source: file_x.md]
                  Before : [description in reference]
                  After  : [new description from skill]

UNCHANGED (n)
  [list of trigger names]
```

### Step 4 — Validate the delta

Display proposed changes in end-user language and request explicit approval:

```
I am about to apply the following changes to [SYSTEM]_help.md:

ADDED
  + TRIGGER_A : [proposed end-user description]
  + TRIGGER_E : [proposed end-user description]

REMOVED
  - TRIGGER_C

MODIFIED
  ~ TRIGGER_D : [proposed end-user description]

Shall I proceed?
```

Wait for explicit approval. The user may adjust descriptions before confirming.

### Step 5 — Apply the delta to [SYSTEM]_help.md

Apply only the validated changes:

- **Added triggers**: insert in summary block (respecting frequency order) + add detailed entry
- **Removed triggers**: remove from summary block + delete detailed entry
- **Modified triggers**: update description in summary block + detailed entry

Do not touch unchanged entries.

### Step 6 — Regenerate the CDC

Once `[SYSTEM]_help.md` is updated, regenerate the CDC using the standard format from `cdc_help.md`:
- Reflect the new trigger inventory
- Update source files list (including newly added files)
- Write to `[PROJECT_PATH]\CDC_[SYSTEM]_help.md`

Confirm in chat:
```
[SYSTEM]_help.md updated — n trigger(s) added, n removed, n modified.
CDC_[SYSTEM]_help.md regenerated to reflect current state.
```

---

## Rules

- Always scan the **full source folder** — never rely on a pre-selected file list
- Timestamps are signals, not proof — always confirm with a full trigger comparison (UPDATE HELP!)
- Never write any file without explicit user approval (Step 4)
- Never rewrite unchanged entries — surgical edits only
- End-user language is mandatory in all proposed descriptions (Step 4)
- CDC regeneration (Step 6) is mandatory after every UPDATE HELP!
- If the CDC was used as reference and found reliable, it is still regenerated to capture the new state
