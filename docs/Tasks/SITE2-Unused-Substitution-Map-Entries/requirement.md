# SITE2: Unused substitution map entries

## Requirements

The Phase 3 substitution map in .opencode/skills/setup/SKILL.md defines several variables that have no corresponding {{PLACEHOLDER}} in any template file under 	emplates/. These will never appear in generated output, making them dead entries.

### Dead entries

| Entry | Section | Category |
|-------|---------|----------|
| INDENT_STYLE | Required | Scanned from .editorconfig, never used |
| INDENT_SIZE | Required | Scanned from .editorconfig, never used |
| ROOT_NAMESPACE | .NET-specific | Detected, never used |
| TEST_CLASS_ATTR_XML | .NET-specific | Alternate XML repr, never used |
| NULLABLE_ENABLED | .NET-specific | Detected, never used |
| FILE_SCOPED_NS | .NET-specific | Detected, never used |
| WEB_FRAMEWORK | .NET-specific | Detected, never used |
| TF_VERSION | Terraform-specific | Never used |
| TF_BACKEND | Terraform-specific | Never used |
| TF_PROVIDER | Terraform-specific | Never used |
| NODE_VERSION | Node-specific | Never used |
| NODE_PKG_MGR | Node-specific | Never used |
| NODE_TEST_FRAMEWORK | Node-specific | Never used |

## Options

1. **Remove them** from the substitution map block and any corresponding scan/question logic.
2. **Add template support** — introduce {{PLACEHOLDER}} in relevant templates so the values appear in generated output.
3. **Keep some as intermediate values** if they feed into computed placeholders (e.g., some might be used to build STACK_ENTRIES).

## Acceptance Criteria

- [ ] Each dead entry is either removed from the substitution map or given a corresponding {{PLACEHOLDER}} in a template.
- [ ] If removed, any scan or question logic that populates it is also cleaned up.
- [ ] Intermediate values that feed computed placeholders are documented as such.
