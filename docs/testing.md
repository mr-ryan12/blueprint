# Testing and Verification

## Current bootstrap checks

For governance changes, inspect the diff, check Markdown links and rule consistency, verify ignore behavior, and confirm the change stays within scope. Inspect repository configuration to determine which application checks are available; record unavailable checks as unavailable, not passed.

## Application verification once tooling exists

- Each implementation task runs typecheck, lint and relevant unit/integration tests. Verify formatting and build behavior when affected; run end-to-end checks for affected critical flows.
- Add tests alongside meaningful business logic and bug fixes. Test observable behavior and failure modes rather than mirroring implementation details.
- Use Vitest for unit/integration tests, React Testing Library for component behavior, and Playwright for critical browser flows. Scripts and exact configuration will be established during implementation.
- Cover changed success and failure paths, input boundaries, access control and relevant UI states. Use the accessibility completion criteria in [conventions](conventions.md).
- Keep default tests deterministic and independent of live third-party services. Use controlled substitutes at external boundaries; use isolated PostgreSQL state for database integration tests and clean up reliably.
- Make live external-service suites explicitly opt-in. Required CI suites must fail when their prerequisites are missing; optional suites must clearly report skips.
- Record commands, results and limitations with the task's verification evidence. Review and commit gates are defined in the [development workflow](development-workflow.md).

## CI direction

The intended sequence is security checks → typecheck → lint/format → tests → build. Add security scanning incrementally as the base stabilizes; distinguish planned checks from enforced checks. Keep CI database state separate from production and provide its prerequisites explicitly.
