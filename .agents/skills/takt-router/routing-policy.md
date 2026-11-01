# Routing policy

TAKT-inspired heuristic for routing work inside Codex. It is an explicit, reviewable policy, not TAKT's proprietary engine and not deterministic scoring. Prefer risk and uncertainty over task size. Pick the highest tier triggered by any hard rule.

## Hard rules: route to High

- Authentication, authorization, secrets, cryptography, security boundaries, or vulnerability remediation.
- Persistent data, database schema/migrations, backups, destructive operations, data integrity, or recovery.
- Production deployment, release, infrastructure, CI/CD permissions, or changes with broad operational impact.
- Payment, privacy-sensitive data, compliance, or irreversible external side effects.
- Cross-cutting architecture or concurrency/state changes whose failure could cause broad regressions.
- The task is ambiguous in a way that could materially affect safety, data, security, or production behavior.

High means delegate to `takt_high`; decompose carefully, inspect relevant call paths and invariants, plan before editing, and require focused verification plus a risk review. The subagent must not perform irreversible external actions unless the user explicitly requested them and the environment permits them.

## Medium

Use Medium when the work requires tracing multiple components, making a nontrivial API or behavior change, refactoring with compatibility concerns, adding tests across layers, or diagnosing a bug with an uncertain cause, and no High hard rule applies.

Delegate to `takt_medium`. Ask for a short implementation plan, scoped changes, and relevant tests or other verification.

## Low

Use Low only for narrow, reversible work with clear expected behavior and low blast radius: typo or documentation edits, simple formatting, isolated mechanical changes, or a well-localized bug with an obvious fix.

Delegate to `takt_low`. Ask for the smallest change and a targeted check when one is practical.

## Tie-break and fallback

- Any High hard rule wins over lower-tier signals.
- If Low versus Medium is unclear, choose Medium.
- If tier cannot be determined, choose High for safety-relevant work; otherwise choose Medium.
- The sample custom agent files intentionally omit `model`. This makes the selected role inherit the parent model unless a model is chosen explicitly at spawn time. This is the safe default when model availability differs across accounts.
- Reasoning effort is set per tier (`low`, `medium`, `high`). If the installed Codex version or parent model rejects the requested effort, omit the override and let that model use its supported default; retain the tier's instructions and review rigor.
- If custom agents or multi-agent tools are unavailable, do the work in the current session following the selected tier's procedure. Do not invoke an external CLI, SDK, MCP service, or another Codex session as a workaround.

## Decision note

Before delegation, state `Tier: Low|Medium|High — <brief reason>`. Treat the classification as an operational choice that can be revised when repository inspection reveals new facts.
