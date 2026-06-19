---
name: skill-developer
description: Self-introspection and improvement for the setup skill. Audits templates, placeholders, paths, and structure. Auto-fixes mechanical issues and generates improvement tickets for structural ones. Trigger keywords: self-improve, introspect, audit skill, review skill, skill review.
---

# Skill Developer

This skill runs **only** on the OpenCode repository itself. It audits the
`setup` skill — its SKILL.md, all 14+ template files, and the generated
config — for inconsistencies, dead references, stale model names, orphan
placeholders, and structural gaps.

## Guard

Before anything else, check that this is the OpenCode repo:

1. Run `git remote -v`.
2. If `origin` URL contains `anomalyco/opencode`, proceed.
3. If not, check for the backup sentinel at `.opencode/.is-opencode-repo`.
4. If neither matches, print:
   > "This skill only runs on the OpenCode repository (anomalyco/opencode)."
   and exit immediately.

## Phase 1 — Bootstrap

If `AGENTS.md` or `opencode.json` do not exist at the project root:

1. Run the setup skill (`@setup`) to generate them.
2. After setup completes, add the `self-improver` sub-agent and the
   `/self-improve` command (see Registration steps at the end of this file).

If both files already exist, skip this phase.

## Phase 2 — Self-review

Run each check below. Group results into:
- **PASS** — no issues
- **FIX** — mechanical issue, auto-fix with user confirmation per group
- **TICKET** — structural issue, create an improvement ticket

### Check 1: Placeholder audit

1. Glob all files under `.opencode/skills/setup/templates/`.
2. For each file, find every `{{PLACEHOLDER}}` — record the file, the
   placeholder, and its line number.
3. Read the setup SKILL.md, find the **Phase 3 — Build substitution map**
   section. Extract every placeholder name from the code blocks.
4. Cross-reference:
   - **Dead entries** — placeholders in the substitution map that appear
     in zero template files. Flag for removal.
   - **Orphan placeholders** — `{{PLACEHOLDER}}` in template files that
     have no entry in the substitution map. These will render as-is in
     the generated output, which is a bug.
5. Auto-fix dead entries (remove them from the substitution map block).
   File a ticket for orphan placeholders (needs a new substitution entry).

### Check 2: Path consistency

1. Grep all template files and the setup SKILL.md for `docs/` paths.
2. Verify they match the current doc layout:
   - Architecture docs → `docs/Architecture/*.md`
   - Ticket docs → `docs/Tasks/TICKETN-Short-Description/`
   - No old-style paths like `docs/tickets/`, `docs/architecture/`
     (wrong case), or `docs/TICKETN/` (no prefix).
3. Auto-fix case mismatches. File a ticket for structural path changes.

### Check 3: Dead reference scan

1. Grep all files under `.opencode/skills/setup/` for the word `scaffold`
   (case-insensitive).
2. Report any remaining references — these should have been renamed to
   `setup` during the scaffold→setup migration.
3. Auto-fix with confirmation.

### Check 4: File inventory

1. Read the setup SKILL.md Phase 5 file list (the table mapping template
   paths to output paths).
2. Compare against the actual files in `.opencode/skills/setup/templates/`.
3. Report:
   - Template files on disk not mentioned in SKILL.md (unreferenced)
   - Template files in SKILL.md not found on disk (missing)
4. File a ticket for any mismatch.

### Check 5: Agent name consistency

1. Read `opencode.json` (either the template at
   `.opencode/skills/setup/templates/opencode.json` or the root one if it
   exists) — extract all agent keys under `"agent"`.
2. Read `AGENTS.md` (template or root) — extract all sub-agent names from
   the Sub-agent Usage table.
3. Read the filenames under `.opencode/skills/setup/templates/agent/`.
4. Verify all three lists agree. Report any name that appears in one list
   but not the others.
5. File a ticket for any inconsistency.

### Check 6: TODO/FIXME/HACK scan

1. Grep all files under `.opencode/skills/setup/` for `TODO`, `FIXME`,
   `HACK`, `XXX` (case-insensitive, excluding `.git/`).
2. Report each with file, line, and context.
3. File a ticket per unique TODO cluster.

### Check 7: Model freshness

1. Read the well-known model tables in the setup SKILL.md (Phase 1 model
   discovery section and Phase 4 model selection guidance).
2. Flag models that are known to be deprecated or superseded.
3. Suggest current replacements based on provider naming conventions.
4. Auto-fix with confirmation per change.

### Check 8: Questions-to-usage audit

1. Read each Phase 2 question from the setup SKILL.md.
2. For each question, trace its answer variable through the substitution
   map to the templates. Does the answer flow to at least one rendered
   `{{PLACEHOLDER}}` in a template file?
3. Flag questions whose answers are collected but never used as candidates
   for removal.
4. File a ticket for each candidate.

### Check 9: Structural coherence

1. For each `SECTION_*` placeholder (SECTION_CODING_STANDARDS,
   SECTION_TESTING, SECTION_ARCHITECTURE), verify there is corresponding
   Phase 4 logic that computes its value.
2. For each `MODEL_*_LINE` placeholder, verify model line computation
   exists in Phase 4.
3. Report any placeholder with missing computation logic.
4. File a ticket for each gap.

## Phase 3 — Report

Build a summary table:

```
Check                          Result
─────────────────────────────────────────────
1. Placeholder audit           PASS / FIX / TICKET
2. Path consistency            PASS / FIX / TICKET
3. Dead reference scan         PASS / FIX / TICKET
4. File inventory              PASS / TICKET
5. Agent name consistency      PASS / TICKET
6. TODO/FIXME scan             PASS / TICKET
7. Model freshness             PASS / FIX / TICKET
8. Questions-to-usage          PASS / TICKET
9. Structural coherence        PASS / TICKET
─────────────────────────────────────────────
Fixes applied:    N (listed above)
Tickets created:  N (listed below)
```

List each auto-fix applied and each ticket created with its path.

## Registration in this repo

This section is for the bootstrap phase or manual setup. It describes what
needs to be added to AGENTS.md and opencode.json.

### AGENTS.md additions

Add to the Sub-agent Usage table:

```
| `@self-improver` | Read-only introspection of the setup skill. Audits templates, detects inconsistencies, generates improvement tickets. Invoked via /self-improve. |
```

Add a dedicated section:

```
## 11. Self-Improvement

This repository includes a `self-improver` sub-agent and a
`/self-improve` command. Use them to audit the `setup` skill's
templates, placeholders, and structure.

- **Trigger**: Run `/self-improve` at any time.
- **Scope**: Audits files under `.opencode/skills/setup/`.
- **Output**: Report of passes, auto-fixes, and improvement tickets.
- **Safety**: Mechanical fixes (typos, formatting, stale model names)
  are auto-fixed with confirmation. Structural changes (new phases,
  template files, questions) create tickets for human review.
```

### opencode.json additions

Add to `"agent"`:

```json
"self-improver": {
  "description": "Read-only introspection of the setup skill. Audits templates, detects inconsistencies, generates improvement tickets. /self-improve to invoke.",
  "mode": "subagent",
  "permission": {
    "edit": "deny",
    "bash": "ask"
  }
}
```

Add to `"command"`:

```json
"self-improve": {
  "description": "Run full introspection on the setup skill. Audits templates, placeholders, paths, and structure. Auto-fixes mechanical issues, creates improvement tickets for structural ones.",
  "agent": "self-improver"
}
```
