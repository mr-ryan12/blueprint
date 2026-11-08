# Architecture

## Current state and intended stack

Current bootstrap status: governance and the initial planning foundation are installed: one shared planning skill, shared strategist/reviewer roles, and thin Codex adapters. Bounded native adapter checks have demonstrated explicit shared-file loading, assigned plan/review writes, a fresh review-only parent invocation, and stopping on a missing shared reference. These checks establish the tested path, not arbitrary context isolation or complete role behavior. Broader behavioral validation and independent completion review of the foundation remain pending.

Application code, dependencies, executable application quality checks, and the remaining agent roles are not installed. The following technology choices are intended for subsequent approved implementation; package versions, commands and helper APIs are not established yet. Reconcile this capability status with actual verification results as the bootstrap advances.

| Area                 | Intended technologies                                                                                            |
| -------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Runtime and build    | Node.js, Yarn 4, TypeScript, Vite                                                                                |
| Application          | React 19, React Router v7 framework mode with SSR                                                                |
| Data and validation  | PostgreSQL, Prisma ORM, Zod                                                                                      |
| HTTP and logging     | Axios, Pino                                                                                                      |
| UI                   | Tailwind CSS, shadcn/ui                                                                                          |
| Quality and delivery | Vitest, React Testing Library, Playwright, ESLint, Prettier, EditorConfig, GitHub Actions, production Dockerfile |

Deferred design decision: Blueprint should eventually include a minimal, clearly removable example vertical slice demonstrating intended patterns. This may include a simple homepage, a small example domain with one or two Prisma models, representative validation, data access and tests, and a future `yarn blueprint:remove-examples` command. This records design intent, not an implementation task or an available command.

## Planning foundation

Canonical platform-agnostic content lives in `.agents/`; platform syntax and invocation details belong in thin adapters that explicitly read that content. Future adapters must reuse the same shared structure without relocating or copying the methodology or role bodies.

| Location | Responsibility |
| --- | --- |
| [Planning skill](../.agents/skills/implementation-planning/SKILL.md) | Investigation methodology and flexible plan/task contract |
| [Implementation strategist](../.agents/roles/implementation-strategist.md) | Plan creation, revision, and planning-only output ownership |
| [Plan reviewer](../.agents/roles/plan-reviewer.md) | Independent assessment, review output contract, and context-evidence procedure |
| [Codex adapter usage](../.codex/README.md) | Native role registration/invocation, explicit loading, tested-client limits, and diagnostics |

Codex is the initial supported adapter path; Claude Code and Kiro adapters are deferred. No skill-catalog wiring or discovery symlink is required. Automatic skill selection must preserve the assigned task and role, and is subject to the [planning-foundation checks](testing.md#planning-foundation-verification). Platform-neutral manual use follows the [workflow](development-workflow.md#using-the-planning-foundation); it does not establish native adapter support. Client-specific evidence and limitations belong in the adapter usage documentation, rather than being treated as shared guarantees.

## Application boundaries

- Prefer server loaders for initial route data and React Router actions for application mutations, using framework forms/fetchers and revalidation where appropriate.
- Routes orchestrate domain behavior. Database calls belong in server-only repositories; services hold business logic when needed and must not depend on routes or UI.
- Mark server-only modules with the framework-supported `.server` file or directory convention. A folder named `server` alone is not an enforced boundary.
- Use configured Axios instances for outbound HTTP, centralizing request/response metadata logging, error normalization, request IDs and appropriate authentication headers. Log bodies or sensitive values only when explicitly safe and permitted by the security guidance. Keep request-specific credentials isolated between requests. Retain native `Request` and `Response` where the framework expects them.
- Pino remains the core choice for structured application logging. Broader observability and error-monitoring integrations, such as OpenTelemetry, Sentry, Datadog or similar tools/vendors, are optional and deferred. Error handling and redaction requirements belong to [security](security.md).

## Infrastructure and deployment

- Native PostgreSQL is the local-development default. Homebrew is the current developer setup; other native installation methods are acceptable. Docker Compose is not required by default.
- Redis is optional and requires a concrete caching, rate-limiting, jobs or distributed-state need.
- Use Docker primarily for production containerization. Keep deployment configuration portable; Railway is a likely initial target, not an application dependency.
- Review schema changes and generated migration SQL deliberately. Plan backfills, rollout compatibility and recovery before applying migrations to shared data; do not use schema push as a shared/production migration strategy.
- Production migrations belong in a separate manually triggered GitHub Actions workflow (`workflow_dispatch`): validate the target environment, run `prisma migrate deploy`, and report the result. Do not run migrations from every application instance. Environment approval gates can be added later.

## Open decisions

Resolve these in the later reviewed application-bootstrap implementation plan, not the Phase 2 planning-foundation plan:

- Feature-oriented versus server-oriented directory organization. Either must preserve the boundaries above; no layout is selected yet.
- Authentication scope and strategy, including whether it is an optional starter capability.
- Exact compatible package/runtime versions and tooling configuration.
- Shared versus UI component organization, and whether optional capabilities warrant an installation CLI.

Record decisions and their rationale here when resolved, without treating reference-project implementations as defaults.
