# SITE1: CUSTOM_RULES placeholder inconsistency

## Requirements

Phase 5 step 4 of the setup skill (.opencode/skills/setup/SKILL.md) instructs:

> If {{CUSTOM_RULES}} is non-empty, insert at the {{CUSTOM_RULES}} marker. If empty, remove the marker line.

However, no template file under 	emplates/ contains the {{CUSTOM_RULES}} placeholder. This means the instruction can never be carried out — the marker doesn't exist.

## Options

1. **Add {{CUSTOM_RULES}}** to an appropriate template (e.g., 	emplates/AGENTS.md in the Communication Rules section, or as a standalone section) so the mechanism works as documented.

2. **Remove the handling logic** from Phase 5 step 4 and mark CUSTOM_RULES as a dead map entry if it's not needed in generated output.

3. **Remove the question and the variable entirely** if custom rules are better handled manually.

## Acceptance Criteria

- [ ] Either {{CUSTOM_RULES}} exists in a template and is properly substituted, OR Phase 5 step 4 and the CUSTOM_RULES substitution map entry are removed.
- [ ] The Q&A question about custom conventions still works or is adjusted accordingly.
- [ ] Tests verify the chosen path.
