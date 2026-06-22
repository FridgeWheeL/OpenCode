# OpenCode

A configuration and skill set for the [OpenCode](https://opencode.ai) AI assistant, extending it with custom agents, reusable skills, and a structured ticket-driven workflow.

## Overview

This repository is **not** the OpenCode CLI itself. It is a **configuration layer** — a collection of custom sub-agents, skills, and workflow definitions that plug into the OpenCode CLI to provide a structured, opinionated approach to AI-assisted software development.

The OpenCode CLI is an independent project. This repo configures it for a specific methodology: agent orchestration, ticket-driven development, user-approval gates, and self-improving skills.

## Features

- **Custom sub-agents** — Specialized AI roles (planner, architect, developer, reviewer, tester, cleanup, etc.) that collaborate within a defined workflow, each with restricted permissions and specific responsibilities.
- **Skills system** — Reusable instruction sets stored under `.opencode/skills/` that are auto-loaded when trigger keywords appear in conversation.
- **Setup skill** — The flagship component. Scans a codebase, detects project type (.NET, Terraform, Node.js, Python, Rust, Go, Java, mono-repos), asks targeted questions, and generates a tailored `opencode.json` and `AGENTS.md` with stack-specific conventions.
- **Ticket-driven workflow** — Each change follows a structured path: requirement → plan → implementation → tests → review → cleanup, with mandatory user approval at every stage.
- **Self-improvement** — A `/self-improve` command introspects the setup skill's templates, placeholders, and structure, auto-fixing mechanical issues and filing improvement tickets for structural gaps.

## Architecture Overview

At the top level, the **primary agent** (defined in `opencode.json`) reads the project instructions in `AGENTS.md` and orchestrates sub-agents. Sub-agents are invoked one at a time — never chained automatically — with user approval required between each stage.

Sub-agents use **skills** — reusable markdown files registered under `.opencode/skills/`. Each skill has a description with trigger keywords. When those keywords match a conversation, the skill is auto-loaded and its instructions become available to the active agent.

The **ticket workflow** (`/ticket TICKETN`) drives the full lifecycle of a change:

1. `@planner` produces a design (`plan.md`)
2. `@architect` designs the technical approach
3. `@developer` implements the code
4. `@reviewer` inspects for standards compliance
5. `@tester` adds tests
6. `@cleanup` polishes code

Each step pauses for user approval. See `docs/Architecture/stack.md` for the technology stack documentation.

## Getting Started

### Prerequisites

- [OpenCode CLI](https://opencode.ai) installed and configured on your machine.

### Setup

```bash
# Clone this repository
git clone https://github.com/FridgeWheeL/OpenCode.git my-project
cd my-project

# OpenCode reads opencode.json and AGENTS.md automatically on launch
# Run the setup skill to bootstrap a new project configuration:
#   /setup
# Or start work on a ticket:
#   /ticket TICKETN
```

Once the repository is cloned, OpenCode will load the configuration from `opencode.json` and the instructions from `AGENTS.md` on every session.

### Starting a new project

Use the `/setup` command to run the setup skill. It will:

1. Scan your project and detect the solution type and structure.
2. Ask targeted questions about your stack, testing framework, CI platform, and conventions.
3. Generate a tailored `opencode.json`, `AGENTS.md`, sub-agent definitions, and architecture docs.

### Working on a ticket

1. Create a branch: `git checkout -b feature/TICKETN`
2. Create a ticket directory: `docs/Tasks/TICKETN-Short-Description/`
3. Write `requirement.md` with the requirements and acceptance criteria.
4. Use `/ticket TICKETN` to start the workflow.

See the [ticket-workflow skill](.opencode/skills/ticket-workflow/SKILL.md) for full details.

## Project Structure

```
AGENTS.md                     Project-level instructions loaded by OpenCode
opencode.json                 Agent, command, and skill configuration
.opencode/
  agents/                     Sub-agent definitions (frontmatter + instructions)
    planner.md
    developer.md
    cleanup.md
    reviewer.md
  skills/
    setup/                    Setup skill — bootstraps OpenCode for any project
      SKILL.md                Usage instructions and phase definitions
      templates/              Template files for generated output
    git-conventions/          Git branch naming, commit messages, PR process
      SKILL.md
    ticket-workflow/          Ticket lifecycle, status tracking, user checkpoints
      SKILL.md
    skill-developer/         Self-improvement — audits the setup skill
      SKILL.md
      templates/
docs/
  Architecture/
    stack.md                  Technology stack reference
  Tasks/                      Ticket directories (one per feature/bugfix)
    TICKETN-Short-Description/
      requirement.md          What needs to be done
      plan.md                 Implementation plan
      status.md               Progress tracking
```

## Skills

| Skill | Description | File |
|-------|-------------|------|
| **setup** | Scans a codebase, detects project type, asks questions, and generates a tailored OpenCode configuration. Works with .NET, Terraform, Node.js, Python, Rust, Go, Java, and mono-repos. | [SKILL.md](.opencode/skills/setup/SKILL.md) |
| **git-conventions** | Branch naming patterns, conventional commit format, code review guidelines. | [SKILL.md](.opencode/skills/git-conventions/SKILL.md) |
| **ticket-workflow** | Full ticket lifecycle: creating requirement.md, plan.md, status.md, resuming across sessions, user approval gates. | [SKILL.md](.opencode/skills/ticket-workflow/SKILL.md) |
| **skill-developer** | Introspection and improvement for the setup skill. Audits templates, placeholders, paths, and structure. | [SKILL.md](.opencode/skills/skill-developer/SKILL.md) |

## Agents

| Agent | Role | Permissions |
|-------|------|-------------|
| **@planner** | Breaks down requirements into tasks, scope, milestones, and success criteria. Read-only. Never makes architecture decisions. | edit: deny, bash: ask |
| **@architect** | Designs solution structure, chooses technologies, defines architecture, maintains architecture docs. | edit: allow, bash: ask |
| **@developer** | Writes production code based on the architecture and plan. | (full) |
| **@reviewer** | Reviews code against requirements and architecture spec. Read-only. | edit: deny, bash: ask |
| **@tester** | Writes and runs unit/integration/edge-case tests. | (full) |
| **@cleanup** | Final polish: refactors readability, removes dead code, normalizes formatting, updates docs. | bash: ask |
| **@self-improver** | Introspects the setup skill, detects inconsistencies, generates improvement tickets. Invoked via `/self-improve`. | edit: deny, bash: ask |

## Contributing

Contributions follow the ticket workflow defined in this repository.

1. Open an issue or pick an existing one.
2. Create a branch: `feature/TICKETN` or `bugfix/TICKETN`.
3. Create a ticket directory at `docs/Tasks/TICKETN-Short-Description/` with `requirement.md`.
4. Work through the agent workflow: planner → architect → developer → reviewer → tester → cleanup.
5. Commit using conventional commit format (`TICKETN: description`).

See [git-conventions](.opencode/skills/git-conventions/SKILL.md) and [ticket-workflow](.opencode/skills/ticket-workflow/SKILL.md) for detailed guidance.
