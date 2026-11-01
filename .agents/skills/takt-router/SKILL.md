---
name: takt-router
description: Classify a software task by difficulty and risk, then delegate it to the matching Codex custom subagent (takt_low, takt_medium, or takt_high). Use when the user asks to implement, investigate, review, or change software and wants effort routed by task complexity.
---

# TAKT-inspired Codex task router

Use this workflow when the user requests software work and invokes this skill or asks for difficulty-based routing.

1. Read `routing-policy.md` before classifying.
2. Classify by the highest risk or complexity signal. Do not infer difficulty from file count or task length alone. State the tier and one-sentence reason.
3. Delegate the substantive work to exactly one matching custom agent: `takt_low`, `takt_medium`, or `takt_high`. Do not fan out parallel agents just to implement one task. If the selected agent is unavailable, use the fallback in `routing-policy.md` and proceed in the current session when necessary.
4. Give the subagent the user's full task, relevant constraints, chosen tier, and expected verification. Ask it to inspect local instructions and repository state before editing, make only task-scoped changes, and report files changed, decisions, and verification evidence.
5. Wait for the subagent. Review its diff and claims yourself. Run verification appropriate to the request and repository instructions. Do not claim checks that were not run.
6. If evidence changes the classification materially, stop delegation and reassess before continuing. Escalate from low to medium/high when new risk appears; never silently downgrade a security, data-loss, or production-impacting task.
7. Report the tier, chosen agent, concise reason, changed files, and verification outcome. Be clear that routing selects a Codex subagent configuration; it does not change the parent session's model.

For planning-only, explanation-only, or other read-only tasks, classify and recommend a tier but do not spawn a subagent unless delegation is useful and the user has authorized it. Never use this skill to bypass the user's approval, sandbox, or permission settings.
