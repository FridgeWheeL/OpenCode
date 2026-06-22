# AGENTS.md — Project Instructions

This file is loaded at the start of every session. It defines the standards,
workflow, and expectations for all work in this repository.

---

## 1. Purpose

This is a **OpenCode** project (other). See
`docs/Architecture/stack.md` for the technology stack and
`docs/Architecture/architecture.md` for the architecture overview.

## 4. Git Workflow

Apply the **git-conventions** skill for full details. Key rules:
- Branch: `feature/TICKETN`, `bugfix/TICKETN`
- Commits: conventional commits (`feature:`, `bugfix:`, `test:`,
  `refactor:`). Imperative mood, 50-char subject, body wraps at 72.
- Commit early, commit often — each commit should compile and pass tests.

## 6. Ticket Workflow

Apply the **ticket-workflow** skill for full details. Standard flow:

1. Create branch: `git checkout -b feature/TICKETN`
2. Ticket documents live at `docs/Tasks/TICKETN-Short-Description/`
   with:
   - `requirement.md` — what needs to be done
3. Start work via `/ticket TICKETN` or by saying "Work on TICKETN".
4. The agent will:
   - Read requirement.md and any existing status.md
   - Invoke `@planner` to break down requirements into tasks, scope, milestones, and success criteria → writes `plan.md`
   - ✅ **Show the plan to the user** → "Does this plan look good?"
     - If the user requests changes, iterate with the planner
     - Only proceed when the user explicitly says "go ahead"
   - Invoke `@architect` to design the technical approach — system design, tech stack, data models, file structure, APIs
   - ✅ **Show the architecture to the user** → "Architecture looks good?"
   - Invoke `@developer` to implement code based on the architecture
   - ✅ **Show summary of changes** → "Review and continue?"
   - Invoke `@reviewer` to review code against requirements and architecture spec — check logic, anti-patterns, security, spec drift
   - If issues found, loop back to `@developer` to apply feedback (repeat 1-2 times)
   - Invoke `@tester` to write and run unit/integration/edge-case tests
   - ✅ **Show test results** → "Tests pass. Continue?"
   - If tests fail, loop back to `@developer` for fixes
   - Invoke `@cleanup` to refactor, remove dead code, normalize formatting, update docs — shippable state
   - ✅ **Ask about commit** → follow the commit workflow (see §10)
   - Update `status.md` with progress summary

## 7. Sub-agent Usage

| Sub-agent | When to use |
|-----------|-------------|
| `@planner` | Break down requirements into tasks, scope, milestones, and success criteria. Read-only. Never makes architecture decisions — escalates to architect. |
| `@architect` | Design technical approach — system design, tech stack, data models, file/folder structure, APIs, interfaces. Reviews planner's plan for architectural consistency. |
| `@developer` | Writing or modifying production code based on the architecture. |
| `@reviewer` | After implementation. Reviews code against requirements and architecture spec — logic errors, anti-patterns, security, spec drift. Read-only. |
| `@tester` | Writing and running unit/integration/edge-case tests. |
| `@cleanup` | Final pass: refactor readability, remove dead code, normalize formatting, update comments/docs, ensure shippable state. |
| `@self-improver` | Read-only introspection of the setup skill. Audits templates, detects inconsistencies, generates improvement tickets. Invoked via /self-improve. |

The primary agent orchestrates the workflow. Sub-agents must be invoked one
at a time, never chained automatically. After each sub-agent completes,
inspect its output and present the result to the user. Never invoke the next
sub-agent automatically — always wait for the user's explicit approval to
proceed.

## 8. Skills System

Skills are reusable instruction sets registered under `.opencode/skills/`.
Each skill has a `description` containing trigger keywords — when those
keywords match the conversation, the skill is auto-loaded and its
instructions become available.

To create a new skill: add a folder under `.opencode/skills/<name>/` with a
`SKILL.md` containing `name` and `description` frontmatter.

## 9. Communication Rules

- No conversational AI phrases ("As an AI...", "Let me know if...",
  "I hope this helps...").
- Be direct and technical.
- Acknowledge with "Done" or the minimal needed response.
- When reporting errors, provide the exact error message and location.

## 10. Commit Workflow

This applies at the end of every ticket workflow AND whenever the user
directly asks to commit.

1. Agent asks: "Would you like to commit these changes?"
2. If yes, agent runs `git diff --stat` to summarize changes.
3. Agent proposes a commit message in this exact format:

   ```
   TICKETN: Brief description of the change

   - Bullet summary of each logical change
   - Another change
   ```

4. Agent shows the message: "Proposed commit message:\n\n```\nTICKETN: ...\n```\n\nConfirm? [Y/n]"
5. If user confirms → `git commit` (local only — never include `--amend`
   or `--force` unless explicitly asked)
6. Agent asks: "Push to remote? [Y/n]"
7. If yes → `git push`

Rules:
- Never commit without showing the message first and getting explicit
  confirmation.
- Never push without asking first.
- Always use `TICKETN:` prefix in the subject line (no dash after number).
- The commit must be local-only until push is confirmed.
- If `git status` shows no changes, inform the user rather than attempting
  a commit.

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
