# REMOVE: Remove env var API key scanning from setup skill

## Background

The setup skill's model discovery phase (Phase 1, step 2) scans environment
variables for `*_API_KEY` vars to detect which LLM providers are available.
This is invasive — users shouldn't need to trust a setup script with scanning
their env vars for API keys.

Since this setup runs inside OpenCode, we can instead:
- Read the existing `opencode.json` to see which providers and models are
  already configured
- Ask the user to specify the path to their `opencode.json` if it's elsewhere
- Present the models already configured in their config file

## Requirements

1. Remove step 2 from Model discovery in Phase 1 of `.opencode/skills/setup/SKILL.md`:
   - Remove checking `ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, `GOOGLE_API_KEY`,
     `GEMINI_API_KEY`, `GROQ_API_KEY`
   - Remove checking `*_API_KEY` env vars
   
2. Enhance the existing config scanning (Phase 1, step 1) to detect models
   from the user's opencode.json:
   - Read the root `opencode.json` for `model`, `provider.*.models`, and
     per-agent `model` overrides
   - If no opencode.json found at root, ask the user for the path to theirs
   - Don't ask for a path if env vars are the only source — just skip

3. Present the discovered models as the `AVAILABLE_MODELS` list for model
   assignment during setup (Phase 2, question 12).

## Acceptance Criteria

- [ ] No env var scanning (`ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, etc.) in
      setup skill
- [ ] opencode.json config is the primary model discovery source
- [ ] User is asked for opencode.json path only if none found at root
- [ ] `AVAILABLE_MODELS` still populates from well-known model list as fallback

## Files to modify

1. `.opencode/skills/setup/SKILL.md` — Phase 1 Model discovery (step 2 removal)
