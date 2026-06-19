# REMOVE-EnvVarScan — Status

## Progress

- [x] **Planner**: Analyzed requirement — SKILL.md already satisfies all acceptance criteria. Only change needed: remove redundant "env vars are not scanned" note on line 112.
- [x] **Coding-agent**: Removed redundant note from `.opencode/skills/setup/SKILL.md:110-111`.
- [x] **Test-specialist**: All 4 acceptance criteria pass. No residual env var references.
- [x] **Reviewer**: Approved. No documentation flags.
- [x] **Solutions-architect**: No architectural concerns. No doc updates needed.
- [x] **Cleanup-agent**: No artifacts, clean whitespace, no stale references.

## Summary

Removed redundant "env vars are not scanned" note from the Model discovery section of `.opencode/skills/setup/SKILL.md`. The env var scanning was already removed in a previous change; this was just cleanup of a leftover note.

## Acceptance Criteria

- [x] No env var scanning (`ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, etc.) in setup skill
- [x] `opencode.json` config is the primary model discovery source
- [x] User is asked for `opencode.json` path only if none found at root
- [x] `AVAILABLE_MODELS` still populates from well-known model list as fallback
