# Development Workflow

## Branches and local work

Use `<type>/<issue-number>-<short-kebab-case-description>`, where type is `feature`, `fix`, `refactor`, `chore`, `docs`, `test`, or `security`. Example: `feature/123-add-user-auth`.

Use `main` as the default integration branch. Additional deployment branches are not part of the default model. The existing `docs/governance` branch is a bootstrap exception; do not invent an issue number or rename it solely to satisfy this convention.

The repository-root `/workspace/` directory is local and gitignored. The owner's subsequent governance instruction selected this existing directory, superseding the earlier `.workspace/` proposal in the handoff and reuse audit without moving local notes. Only `/workspace/` is supported and ignored; `.workspace/` is not a compatibility alias and must not be used for local artifacts.

Organize task artifacts by branch, for example:

```text
workspace/feature/123-add-user-auth/
  issue-123.md
  context.md
  plan.md
  plan-review.md
  task-verification.md
  code-review-summary.md
  pull-request-summary.md
```

Create artifacts as needed. `context.md` records durable decisions, clarified requirements, assumptions, rejected approaches, discoveries, and open questions; it is not a chat transcript. Put decisions needed by future contributors in committed documentation or the issue/PR, since local artifacts are not shared.

## Responsibilities

These responsibilities apply whether performed by people or agents. Available automation is recorded in the [current bootstrap status](architecture.md#current-state-and-intended-stack).

| Role                        | Responsibility                                                                                                                           |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `issue-author`              | Define the problem, requirements, acceptance criteria, scope and dependencies; distinguish requirements from implementation suggestions. |
| `implementation-strategist` | Inspect issue, code and conventions; expose gaps and risks; produce a phased plan without production implementation.                     |
| `plan-reviewer`             | Independently challenge requirements, assumptions, architecture, sequencing, tests, security and deployment concerns.                    |
| `fullstack-engineer`        | Implement complete vertical slices through bounded tasks.                                                                                |
| `task-verifier`             | Independently verify each task's diff, requirements, checks and unintended changes. When requested, validate findings from other reviewers before implementation changes are made. |
| `code-reviewer`             | Review the complete change independently of both implementation and task verification.                                                   |
| `architect`                 | Advise on major architectural decisions, new subsystems and significant refactors or infrastructure.                                     |

## Using the planning foundation

Use the [implementation-planning skill](../.agents/skills/implementation-planning/SKILL.md) for methodology and the plan/task output contract. The shared [implementation strategist](../.agents/roles/implementation-strategist.md) owns plan creation and revisions; the [plan reviewer](../.agents/roles/plan-reviewer.md) owns the independent assessment and its review artifact. Native platform invocation and tested-client limitations are documented in [adapter usage](../.codex/README.md).

For manual role loading, start a session for the assigned role and explicitly read its shared definition, the skill, AGENTS.md, and their required canonical references. Supply the source issue, repository root/access, relevant durable decisions, current authorization, and exact assigned output path under `workspace/`. Supply the current plan for a review. When repository state matters, an authorized coordinator/verifier provides pre-run branch, HEAD, and working-tree evidence with provenance and freshness under the skill's contract; the roles do not run Git commands.

Use durable artifacts for the handoff: issue and relevant context → strategist plan → independent review → strategist revision and finding disposition → re-review where warranted → task-owner approval. Each role writes its assigned artifact directly; the reviewer must not modify the plan. Preserve existing outputs unless replacement is authorized, and follow the shared contracts for missing inputs or denied writes. Local artifact organization remains as described above; do not require private scratch notes or a saved authoring conversation as review input.

Apply the reviewer's shared [independent-review procedure](../.agents/roles/plan-reviewer.md#independent-review-procedure) and evidence standard for both native and manual review. That definition owns fresh-session inputs, permitted isolated-child paths, context evidence, and fallback details. Manual use or a role label alone does not prove native support. The approval gates below still apply, including separate-session or human review where automation is unavailable.

## Planning and implementation

1. Author the issue, then begin planning in a fresh session/context with durable issue context.
2. Inspect the repository before planning. Separate requirements from suggestions; identify contradictions, assumptions, edge cases, dependencies, and security, migration and testing implications.
3. Write meaningful engineering tasks with acceptance criteria and verification. Obtain independent plan review, revise, and obtain approval from the task owner before implementation.
4. Implement one approved task at a time. Run the applicable checks in [testing](testing.md), obtain independent task verification, and fix confirmed issues. If the current task explicitly authorizes committing, commit the verified task before proceeding; otherwise stop at the verified working-tree state.
5. Obtain comprehensive code review. Independently verify findings before implementing fixes; repeat relevant checks and review when warranted.
6. When authorized, push and open a PR. Resolve confirmed automated-review findings, obtain human/team review, and merge through the repository's configured controls.

If authorization ends at a working-tree review, stop there. If an independent reviewer is unavailable, report the missing review and seek human review; do not describe self-review as independent verification.

## Review findings

| Severity | Meaning                                                                         |
| -------- | ------------------------------------------------------------------------------- |
| Critical | Security issue, data loss risk, broken core behavior or dangerous migration     |
| High     | Clear correctness bug, significant regression or serious architecture violation |
| Medium   | Maintainability concern, missing edge case, test gap or accessibility issue     |
| Low      | Naming, style or optional cleanup                                               |

Findings must identify evidence, impact and an actionable correction. Verify whether they are real and in scope before acting; avoid churn from speculative or low-value comments. Record confirmed fixes and explicitly deferred findings.

## Bootstrap sequence

1. Establish this governance layer and obtain human review.
2. Create the implementation-planning skill, implementation strategist, and independent plan reviewer.
3. Use a manually authored bootstrap issue and those planning roles to plan and review the remaining starter kit.
4. Establish implementation and verification roles early in the approved plan, then build the stack using the task loop above.

When authoring the bootstrap issue, explicitly include or defer each remaining item below, with a rationale for deferrals. Tool/provider selection and nonessential skills remain open; this checklist does not authorize implementation.

- Environment example (`.env.example`) and health endpoint.
- PR template and dependency-update setup.
- Automated GitHub review and branch-protection configuration; assign their setup rather than assuming they already exist.
- Remaining skill areas: TypeScript, React Router framework mode, Prisma/PostgreSQL, application security, testing, frontend design/UI engineering, system design, DevOps, Mermaid diagrams, and GitHub work management.

Until the relevant agents exist, use separate reviewer sessions or human review. Application checks become required as the corresponding tooling is introduced; current documentation checks are defined in [testing](testing.md).
