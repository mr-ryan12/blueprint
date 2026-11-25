# Architecture

## Current state and intended stack

Governance and the initial planning foundation are installed, including a shared planning skill, shared strategist and reviewer roles, and thin Codex adapters. Bounded checks demonstrated native invocation and explicit loading, assigned plan and review writes, fresh review context, missing-input stops, ordinary-session task preservation, seeded-defect detection, a plan/review/revision handoff, and rejection of invalid, redirected, unauthorized-replacement, and host-denied writes. The planning foundation completed independent task verification and completion review. The observations apply to the [tested planning configuration](../.codex/README.md#planning-validation-history).

The shared fullstack engineer, task verifier and code reviewer, their Codex adapters and five implementation and review skills are installed. Independent fixture verification observed native execution for all three roles and demonstrated separate native and manual handoffs for both review roles on the tested local transport. See [adapter usage](../.codex/README.md#implementation-and-review-validation) for the tested configuration and outstanding limitations.

Application code, dependencies and executable application quality checks are still pending. The approved support baseline is Node 24 (initial pin 24.22.2), Yarn 4 (initial pin 4.18.1, `node-modules` linker), React Router 7, Prisma 7 and PostgreSQL 18 (initial pin 18.6). Use one ESM application package with strict TypeScript. Revalidate the approved version matrix at implementation and keep the runtime baseline aligned across development, CI and Docker. Installation, lockfile resolution and runtime compatibility still require verification.

| Area                 | Intended technologies                                                                                            |
| -------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Runtime and build    | Node.js, Yarn 4, TypeScript, Vite                                                                                |
| Application          | React 19, React Router v7 framework mode with SSR, thin Express server using the official React Router adapter    |
| Data and validation  | PostgreSQL, Prisma ORM, Zod                                                                                      |
| HTTP and logging     | Axios, Pino                                                                                                      |
| UI                   | Tailwind CSS, shadcn/ui                                                                                          |
| Quality and delivery | Vitest, React Testing Library, Playwright, ESLint, Prettier, EditorConfig, GitHub Actions, production Dockerfile |

The approved example is a removable public notes feature with one model and list, create and delete operations. It is disabled by default in production and has no account or ownership semantics. Document manual removal. An automated removal command and optional capability installer are deferred.

## Planning foundation

Canonical platform-agnostic content lives in `.agents/`; platform syntax and invocation details belong in thin adapters that explicitly read that content. Future adapters must reuse the same shared structure without relocating or copying the methodology or role bodies.

| Location | Responsibility |
| --- | --- |
| [Planning skill](../.agents/skills/implementation-planning/SKILL.md) | Investigation methodology and flexible plan/task contract |
| [Implementation strategist](../.agents/roles/implementation-strategist.md) | Plan creation, revision, and planning-only output ownership |
| [Plan reviewer](../.agents/roles/plan-reviewer.md) | Independent assessment, review output contract, and context-evidence procedure |
| [Codex adapter usage](../.codex/README.md) | Native role registration/invocation, explicit loading, tested-client limits, and diagnostics |

Codex is the initial supported adapter path. Claude Code and Kiro adapters, skill-catalog wiring and discovery symlinks are deferred. Automatic skill selection must preserve the assigned task and role. See the [planning checks](testing.md#planning-foundation-verification) and [implementation and review checks](testing.md#implementation-and-review-foundation-verification). The [workflow](development-workflow.md#using-the-planning-foundation) describes platform-neutral manual loading for planning. Client-specific evidence belongs in [adapter usage](../.codex/README.md#validation-and-diagnostics).

## Application boundaries

Use a feature-oriented layout. These locations are approved for subsequent application work.

| Planned location | Responsibility |
| --- | --- |
| `app/root.tsx`, `app/routes.ts`, `app/routes/` | Framework shell, route registration, thin loaders/actions and route error boundaries |
| `app/features/example-notes/` | Example validation, feature UI and server-only persistence. Add a service where business logic warrants it. |
| `app/lib/*.server.ts` | Reusable environment, database, request context, logging and outbound HTTP infrastructure |
| `app/components/ui/`, `app/components/` | shadcn/ui primitives and non-domain components with demonstrated reuse, respectively |
| `server/index.ts` | Thin Express bootstrap for startup validation, request context and graceful shutdown |

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
- Codex automatic GitHub PR review is in bootstrap scope. Ryan owns configuration and verification of remote controls, including CI and branch protection. Their enabled state remains unverified. GitHub MCP is deferred. Follow the [workflow](development-workflow.md#solo-maintainer-signoff) for delivery signoff.

## Authentication

Authentication implementation is deferred from the application bootstrap. Design, provider and credential decisions belong to the separately planned authentication feature. See the [application shipping gate](development-workflow.md#application-shipping-gate).
