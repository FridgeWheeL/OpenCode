# UPDATE: Update branch naming conventions

## Requirements
- Change `feat/TICKETN` to `feature/TICKETN` in all branch naming references
- Change `fix/TICKETN` to `bugfix/TICKETN` in all branch naming references
- Remove `chore/TICKETN` from branch naming references
- Update conventional commit types from `feat:` to `feature:`, `fix:` to `bugfix:`, remove `chore:`
- Update all example commit messages accordingly

## Acceptance Criteria
- [ ] 6 files updated across the codebase
- [ ] No remaining references to `feat/`, `fix/`, or `chore/` in branch naming context
- [ ] Commit and push complete

## Files to modify
1. `.opencode/skills/setup/templates/skills/git-conventions/SKILL.md`
2. `.opencode/skills/git-conventions/SKILL.md`
3. `AGENTS.md`
4. `.opencode/skills/setup/templates/AGENTS.md`
5. `.opencode/skills/ticket-workflow/SKILL.md`
6. `.opencode/skills/setup/templates/skills/ticket-workflow/SKILL.md`
