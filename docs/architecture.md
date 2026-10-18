# Architecture

## Current state and intended stack

Current bootstrap status: governance only; application code, dependencies, agents, skills and executable quality checks are not installed. Update this status as capabilities are introduced. The following choices are intended for subsequent approved implementation; package versions, commands and helper APIs are not established yet.

| Area                 | Intended technologies                                                                                            |
| -------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Runtime and build    | Node.js, Yarn 4, TypeScript, Vite                                                                                |
| Application          | React 19, React Router v7 framework mode with SSR                                                                |
| Data and validation  | PostgreSQL, Prisma ORM, Zod                                                                                      |
| HTTP and logging     | Axios, Pino                                                                                                      |
| UI                   | Tailwind CSS, shadcn/ui                                                                                          |
| Quality and delivery | Vitest, React Testing Library, Playwright, ESLint, Prettier, EditorConfig, GitHub Actions, production Dockerfile |

Deferred design decision: Blueprint should eventually include a minimal, clearly removable example vertical slice demonstrating intended patterns. This may include a simple homepage, a small example domain with one or two Prisma models, representative validation, data access and tests, and a future `yarn blueprint:remove-examples` command. This records design intent, not an implementation task or an available command.

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

Resolve these in the first reviewed implementation plan:

- Feature-oriented versus server-oriented directory organization. Either must preserve the boundaries above; no layout is selected yet.
- Authentication scope and strategy, including whether it is an optional starter capability.
- Exact compatible package/runtime versions and tooling configuration.
- Shared versus UI component organization, and whether optional capabilities warrant an installation CLI.

Record decisions and their rationale here when resolved, without treating reference-project implementations as defaults.
