---
name: implementation-planning
description: Develop or revise phased implementation plans from an issue or request and repository evidence. Use for planning before implementation; reviewers may consult the plan contract. Reading this skill does not convert an implementation or review assignment into plan authoring.
---

# Implementation planning

Turn requirements and repository evidence into a plan an implementer can execute
and an independent reviewer can challenge. Preserve the current assignment:
when reviewing, consult the plan contract without rewriting the plan; when
implementing, use relevant guidance without abandoning the assigned task.
Only a planning assignment calls for the complete workflow below.

## Establish authority and scope

Read [AGENTS.md](../../../AGENTS.md) before designing the change. Read relevant
canonical guidance through its index; do not replace it with skill examples.
The governing owners are:

- [Development workflow](../../../docs/development-workflow.md): roles, artifacts,
  review findings, and approval gates.
- [Conventions](../../../docs/conventions.md): maintainability, dependencies,
  TypeScript, UI completion, and portable guidance.
- [Architecture](../../../docs/architecture.md): current capabilities, intended
  boundaries, infrastructure, and unresolved decisions.
- [Testing](../../../docs/testing.md): applicable verification and isolation.
- [Security](../../../docs/security.md): trust boundaries, authorization, secrets,
  and safe error handling.

Identify the source issue/request, objective, scope exclusions, repository root,
authorized outputs, and current authorization. Report missing source material;
historical notes and examples do not silently substitute for requirements.
Separate required outcomes and explicit constraints from implementation suggestions.
Record relevant owner clarifications and decisions with their sources. Personal
scratch notes are not durable requirements unless the owner explicitly promotes
particular content into the task.

Planning/review roles must not perform Git operations, including read-only Git
commands. When needed, obtain branch, HEAD, and working-tree status as pre-run
evidence from an authorized coordinator/verifier. Record its source, collection
time, repository root, and known intervening changes. Label stale or unavailable
state; request refreshed evidence when material rather than guessing. File reads
and artifact hashes do not establish branch or dirty state.

## Investigate before choosing an approach

Inspect affected source, configuration, interfaces, dependencies, tests, and
operational documentation. Follow actual paths that own relevant behavior.
Distinguish existing behavior from intended capabilities and proposed additions.
Verify referenced paths, symbols, component interfaces, and commands before
relying on them. Reference projects teach patterns, not target-repository APIs.

Before sequencing work, expose missing requirements, contradictions, unsupported
assumptions, edge cases, and dependencies. Distinguish facts, inferences, and
unanswered questions; attach evidence to consequential conclusions.
Ask the owner before finalizing choices that materially change architecture,
scope, sequencing, or acceptance criteria. Continue investigation that does not
depend on the answer; an unanswered question is not approval.

## Assess implications proportionately

Consider the following against governing documents and inspected behavior.
Explain material non-applicability instead of inventing work for every category.

- Architecture/dependencies: affected systems, ownership boundaries, compatibility,
  integration contracts, prerequisites, and operational costs.
- Data: model changes, existing records, constraints, migrations/backfills,
  rollout compatibility, recovery, and data-loss risks.
- Server/API: public behavior, validation boundaries, authorization, failures,
  consumers, and request-specific state.
- UI: affected flows, accessibility, responsiveness, and relevant interaction,
  loading, empty, failure, and success states.
- Security: changed trust boundaries, resource access, secret exposure, sensitive
  logging, and dependency risks. Keep private data out of external requests.
- Testing: observable success/failure behavior, input boundaries, appropriate test
  layers, isolation, prerequisites, and currently unavailable checks.
- Deployment/operations: configuration, rollout order, environment differences,
  monitoring, failure recovery, and migration ownership where applicable.

Choose the simplest approach supported by evidence and requirements. Explain
consequential tradeoffs and rejected alternatives, not every conceivable option.
Label proposed interfaces and paths as proposals. Avoid large or executable-looking
examples that could be mistaken for existing APIs. Link canonical rules instead
of copying their policy into the plan.

## Define phases and bounded tasks

Sequence work by dependencies and feedback needs. Use phases to group coherent
outcomes, with tasks small enough to implement and verify independently.
Avoid keystroke-sized tasks and fixed backend/UI/testing phase templates.
Place meaningful tests alongside changed behavior rather than deferring all
verification to a final phase. Keep unresolved prerequisites as explicit gates.

Each task should identify:

- Stable ID, intended outcome, and dependency IDs or prerequisites.
- Bounded changes and affected systems, including existing versus proposed paths.
- Acceptance criteria stated as observable outcomes.
- Verification tied to inspected commands or manual checks; clearly label checks
  to be introduced and checks currently unavailable.
- Relevant risks, unresolved decisions, and handoff or stop conditions.

## Plan contract

Use a format suited to the change. A handoff-ready plan should express:

- Objective, requirements, and scope exclusions.
- Current architecture/context and inspected evidence, with supplied state limits.
- Assumptions, open questions, risks, and decisions requiring owner input.
- Proposed approach, affected systems, dependencies, and consequential tradeoffs.
- Relevant data, server/API, UI, security, testing, and deployment implications.
- Phases and bounded tasks with acceptance criteria and verification requirements.
- Readiness, remaining limitations, and the next review/approval step.

Combine or omit irrelevant headings for small tasks while retaining material
reasoning. Include requirement-to-task traceability when complexity warrants it.
Distinguish proposed, executed, failed, and unavailable checks; absent tooling
cannot be reported as passing. Do not add infrastructure solely to make a
documentation plan look tested.

## Write and hand off within the assignment

While acting as planner, write the plan directly to its assigned artifact under
repository-root `workspace/`. Confirm the exact destination before writing;
reject paths or symlink redirections outside that area. Preserve existing outputs
unless replacement is authorized. Additional artifacts require explicit assignment.
Do not modify production code, dependencies, governance, platform configuration,
or unrelated files while planning. If the assigned write is denied, report the
unmet requirement without bypassing controls or substituting another writer.
These instructions do not establish an enforced filesystem sandbox.

Hand the durable plan and relevant evidence to independent review according to
the development workflow. Keep private authoring history out of the reviewer
handoff; do not describe self-review as independent. Record reviewer limitations
and use the workflow's separate-session or human fallback when needed.
Revise from confirmed findings, recording corrections and explicit deferrals.
A reviewer recommendation is not task-owner approval, and an approved plan does
not expand implementation or Git authorization. Stop before implementation when
the assignment is planning; report what remains for review and owner approval.
