# SITE3: DOTNET_VERSION collected but unused in templates

## Requirements

Phase 2 question 10 collects the .NET version from the user:

> ".NET version (if applicable, from TFM e.g., 
et8.0):"

This value is stored as {{DOTNET_VERSION}} in the substitution map, but no template file contains a {{DOTNET_VERSION}} placeholder. The version is never displayed in the generated output.

## Options

1. **Add {{DOTNET_VERSION}} to 	emplates/docs/stack.md** — display the .NET runtime version in the Project section of the tech stack table.
2. **Remove the question and map entry** if the information is not needed in generated output.
3. **Keep as-is** if the value is used internally by the setup skill for framework detection logic (verify this first).

## Acceptance Criteria

- [ ] {{DOTNET_VERSION}} appears in at least one template (recommended: stack.md) OR question 10 and the map entry are removed.
- [ ] If kept, the stack table correctly displays the version.
