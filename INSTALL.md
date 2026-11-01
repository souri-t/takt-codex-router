# TAKT-inspired Codex Router

This bundle provides a project-local Skill and three custom Codex subagents. It adapts TAKT-style difficulty and risk routing to Codex. It is not TAKT's routing engine. The Skill asks the current Codex session to classify work and delegate it to one named subagent.

## Requirements and behavior

- A current Codex client with project Skills and custom subagents enabled.
- The project must be trusted before Codex loads `.codex/config.toml`.
- Subagents inherit the parent session's provider, permissions, sandbox, approval settings, and MCP tools. These files do not relax those controls.
- The agents omit a `model` value by design, so they inherit the parent model safely. To route to distinct models, add a model ID available to your signed-in account in each `.codex/agents/takt_*.toml`; check current availability in your Codex model picker first. This affects delegated subagents only, not the parent session.
- Reasoning effort values may vary by model. If the client rejects one, remove that agent's `model_reasoning_effort` line and let the model use its default.

## Install

1. Extract the ZIP. Copy its `.agents/` directory and `.codex/` directory into the root of the target repository, preserving the directory structure.
2. If those directories already contain files, merge them. Do not overwrite existing `.codex/config.toml`; merge the `[agents]` entries and custom role registrations. Keep one definition per key. Preserve your existing approval, sandbox, model, provider, and MCP settings.
3. Open the target repository in Codex and trust it when prompted. Restart or reload the project if the Skill or agents do not appear.
4. In a task prompt, explicitly request: `Use $takt-router to classify this task and delegate it to the matching custom subagent.` The Skill name is `takt-router`.
5. Verify first with a harmless documentation-only task. The parent should state a tier, use one matching subagent, inspect the result, and report checks accurately.

## Verify configuration

Inspect Codex's agent list or ask it to identify the available custom roles. Confirm `takt_low`, `takt_medium`, and `takt_high` are listed. Confirm the project Skill `takt-router` is discoverable. If custom agents are unavailable in your client, use the Skill's same-tier procedure in the current session; the package does not require another Codex session, CLI, or SDK.

## Routing summary

- **Low:** narrow, clear, reversible changes.
- **Medium:** multi-component or nontrivial behavior changes.
- **High:** security, persistent data, production impact, irreversible effects, or material safety uncertainty.

Risk takes precedence over task size. If uncertain, route upward. See `.agents/skills/takt-router/routing-policy.md` for the complete policy.

## Official references checked

- [Codex Subagents and custom agents](https://learn.chatgpt.com/docs/agent-configuration/subagents): project agent files, required fields, model and reasoning configuration, inheritance, and permissions.
- [Codex Configuration Reference](https://learn.chatgpt.com/docs/config-file/config-reference): project-local `.codex/config.toml`, trust behavior, `[agents]` settings, and role `config_file` entries.
- [Codex Skills](https://developers.openai.com/codex/skills): Skill folder structure and `SKILL.md` metadata/instructions.

Codex changes over time. Recheck the official references before pinning model IDs or relying on version-specific behavior.
