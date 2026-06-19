# Plan: UPDATE-README — Update default README

## Overview

Replace the minimal `# OpenCode` README with a comprehensive project README that accurately describes this repository as an OpenCode configuration and skill set project, covering its purpose, features, agent architecture, skills system, and getting-started instructions.

## Areas Affected

| Area | Impact |
|------|--------|
| `README.md` (root) | Complete rewrite — only file touched |
| `docs/Architecture/stack.md` | Possible minor update if the README refers to stack info that should be kept in sync |

All changes are limited to documentation only. No production code, configuration, or architecture changes.

## New Items

None. No new files to create.

## Modified Items

| File | Change |
|------|--------|
| `README.md` | Full rewrite with comprehensive project documentation |

## Implementation Order

1. **Analyze the project structure and extract key facts** (already done by the planner):
   - Project type: OpenCode configuration repository
   - Key components: custom agents (planner, coding-agent, test-specialist, reviewer, cleanup-agent, solutions-architect, self-improver), custom skills (setup, git-conventions, ticket-workflow, skill-developer), OpenCode configuration (`opencode.json`), project instructions (`AGENTS.md`), architecture docs (`docs/Architecture/stack.md`)
   - The setup skill is the flagship deliverable — it bootstraps OpenCode config for any project type (.NET, Terraform, Node.js, Python, mono-repos, etc.)
   - GitHub: `https://github.com/FridgeWheeL/OpenCode`

2. **Write the README** — content sections in order:
   - **Title & Tagline**: `# OpenCode` with a one-line subtitle describing the repo as a configuration and skill set for OpenCode AI assistant
   - **Badges** (optional, but nice): repo stats, license
   - **Overview / What is this?**: Explain this is not the OpenCode CLI itself, but rather a configuration layer — a collection of custom agents, skills, and workflows that extend OpenCode for AI-assisted software development
   - **Features**: Bullet list of key offerings:
     - Custom sub-agents (planner, coding-agent, reviewer, etc.)
     - Skills system (setup, git-conventions, ticket-workflow, skill-developer)
     - Setup skill — auto-generates OpenCode config for any project type
     - Ticket-driven workflow with user approval gates
     - Self-improvement introspection via `/self-improve`
   - **Architecture Overview**: Brief description of the agent hierarchy, the skills system, and how they interact (reference `docs/Architecture/stack.md`)
   - **Getting Started / Usage**:
     - Prerequisites: OpenCode CLI installed
     - Clone the repo
     - OpenCode reads `opencode.json` and `AGENTS.md` automatically
     - Run `/setup` to bootstrap a new project, `/ticket TICKETN` to start work
   - **Project Structure**: Annotated directory tree showing key files and their purpose
   - **Skills**: Description of each skill with links to their SKILL.md files
   - **Agents**: Table of sub-agents with their roles
   - **Contributing**: How to contribute (ticket workflow, branch naming)
   - **License**: MIT (as indicated by the `@opencode-ai/plugin` package)

3. **Verify markdown correctness** — check for broken links, consistent formatting, proper heading hierarchy.

## Edge Cases & Risks

- **Misrepresentation risk**: The README must NOT claim this is the official OpenCode CLI repository. It must clearly state that this is a *configuration and skill set* for use *with* OpenCode. The upstream official OpenCode is at `github.com/opencode-ai/opencode` (archived) and now continues at Charm (`github.com/charmbracelet`).
- **Stale content**: If the project structure changes later (new skills, removed agents), the README will need updating. Mitigation: keep the directory tree at a high level rather than listing every file.
- **Out-of-sync with stack.md**: The README should reference `docs/Architecture/stack.md` for the technology stack rather than duplicating it, to avoid drift.

## Test Strategy

No code production — no automated tests applicable. Verification steps:

1. **Manual review**: Read the rendered markdown for clarity, accuracy, and professional formatting.
2. **Link check**: Verify all internal links (to `docs/`, `.opencode/skills/*/SKILL.md`, etc.) resolve correctly.
3. **Preview**: Optionally render in a markdown viewer to check formatting.

## Documentation Flags

```
Documentation updates needed:
  - None required. The README is a standalone document that references
    existing architecture docs (docs/Architecture/stack.md) without
    changing them. No architecture decisions are made.
```

| Doc file | Needs update? | Reason |
|----------|--------------|--------|
| `docs/Architecture/stack.md` | No | README references it but does not change it |
| `docs/Architecture/architecture.md` | N/A — does not exist | No architecture document to update |
