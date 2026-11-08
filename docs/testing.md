# Testing and Verification

## Current bootstrap checks

For governance changes, inspect the diff, check Markdown links and rule consistency, verify ignore behavior, and confirm the change stays within scope. Inspect repository configuration to determine which application checks are available; record unavailable checks as unavailable, not passed.

## Planning-foundation verification

Use proportionate static inspection and bounded manual behavioral checks; no application test framework is required for instruction files. Follow the [workflow](development-workflow.md) for independent verification and review gates, and the shared [reviewer procedure](../.agents/roles/plan-reviewer.md#independent-review-procedure) for context evidence. The [architecture status](architecture.md#current-state-and-intended-stack) records installed versus verified capabilities; expectations below do not imply that every check has passed.

- Check relative links/anchors and required file-load paths, skill frontmatter and size against [conventions](conventions.md#shared-agent-and-skill-content), adapter structure against its supported schema, scope, and ignore behavior. Inspect untracked deliverables directly rather than staging them to obtain a diff.
- Verify canonical ownership and platform separation: shared content must remain client-neutral, adapters must explicitly load it, and role responsibilities/output ownership must remain distinct. A parser, help command, catalog listing, or role name alone is not proof of native execution. Skill-catalog discovery remains deferred.
- Observe native invocation, explicit shared-file reads, assigned direct artifact writes, unchanged inputs, and missing-input failure behavior. Verify fixture trust/loading rather than assuming it, and record actual enforcement separately from instruction-level write restrictions.
- Exercise representative planning, implementation, and review prompts in ordinary sessions. Record skill selection when observable and resulting behavior; the agent must preserve its assigned task and role. A reviewer may consult the skill's plan contract without becoming a planner or rewriting the plan.
- Challenge review behavior using bounded flawed and sound plans. Record actionable evidence and whether each seeded defect was detected, missed, or unverified separately; do not manufacture findings for sound plans. Validate plan/review/revision handoff and the reviewer's inability to grant owner approval.
- Check artifact-write failures in disposable fixtures: external/traversal destinations, symlink redirection, existing output without replacement authorization, and host-denied writes. Keep sentinels and targets inside the disposable test area. Compare before/after contents and tool activity; rejected writes must not produce alternate or substitute-writer output or permission-bypass attempts.
- Record prompts, client/version, actual model/reasoning settings or their unavailability, supplied repository-state provenance/freshness, allowed output paths, relevant reads/writes, context/launch evidence, outcomes, and limitations in local task artifacts. Limit behavioral conclusions to the tested configuration; no model pin or repeated-run requirement is implied. Do not count unavailable checks as passed or self-checks as independent verification.

Bootstrap validation uses recorded manual sessions, static document/schema inspection, file-content comparisons, and disposable-fixture write attempts; there is no installed repository test runner for this content. Ordinary-session checks assess implicit selection separately from native role loading. Negative-case evidence includes actual denied writes and unchanged sentinels, not just a role's final assurance. See [adapter usage](../.codex/README.md#validation-and-diagnostics) for the tested configuration and limits; keep raw prompts, per-defect scores, settings, and outcomes in the local task evidence.

Reconcile capability status and verification descriptions with the final recorded outcomes after behavioral validation and corrections. Preserve evidence of failed or unavailable scenarios and leave the relevant capability unverified until supported. Optional external validators may be unavailable; record that separately from checks actually performed with available tools. Available documentation/static checks and native smoke checks do not establish application typecheck, lint, test, build, CI, or database verification; those require the corresponding tooling below.

## Application verification once tooling exists

- Each implementation task runs typecheck, lint and relevant unit/integration tests. Verify formatting and build behavior when affected; run end-to-end checks for affected critical flows.
- Add tests alongside meaningful business logic and bug fixes. Test observable behavior and failure modes rather than mirroring implementation details.
- Use Vitest for unit/integration tests and React Testing Library for component behavior. Reserve Playwright/end-to-end tests for critical user flows and cross-system/browser behavior; they are not the default layer for all application logic. Scripts and exact configuration will be established during implementation.
- Cover changed success and failure paths, input boundaries, access control and relevant UI states. Use the accessibility completion criteria in [conventions](conventions.md).
- Keep default tests deterministic and independent of live third-party services. Use controlled substitutes at external boundaries. Database integration tests must use an isolated test database or schema, clean up reliably, and never run against shared development, UAT, staging or production data.
- Make live external-service suites explicitly opt-in. Required CI suites must fail when their prerequisites are missing; optional suites must clearly report skips.
- Record commands, results and limitations with the task's verification evidence. Review and commit gates are defined in the [development workflow](development-workflow.md).

## CI direction

The intended sequence is security checks → typecheck → lint/format → tests → build. Add security scanning incrementally as the base stabilizes; distinguish planned checks from enforced checks. Keep CI database state separate from production and provide its prerequisites explicitly.
