# Contributor and Agent Guidance

Blueprint is a reusable starter kit. See [current bootstrap status](docs/architecture.md#current-state-and-intended-stack) for implemented capabilities.

## Sources of truth

- Do not stage changes, create commits, push branches, merge changes, publish releases, or alter Git history unless the current task explicitly authorizes that specific action.
- Inspect repository code and configuration before making assumptions. Verify that referenced paths, functions, types, commands, and dependencies exist.
- Repository evidence and committed project conventions take precedence over generic skills, examples, historical notes, and reference projects. Code describes current behavior; conventions describe required behavior. Surface conflicts rather than silently choosing one.
- Treat examples as patterns, never as proof of an existing repository API. Reuse reference material only after reviewing its applicability.

## Canonical guidance

Each topic has one owner; link to it instead of repeating its rules in agents, skills, or other documents.

| Document                                             | Owns                                                                             |
| ---------------------------------------------------- | -------------------------------------------------------------------------------- |
| [Conventions](docs/conventions.md)                   | TypeScript, maintainability, dependencies, UI completion, portable guidance      |
| [Development workflow](docs/development-workflow.md) | Branches, local work artifacts, roles, planning, implementation and review gates |
| [Architecture](docs/architecture.md)                 | Intended stack, boundaries, infrastructure, deployment and open decisions        |
| [Testing](docs/testing.md)                           | Verification expectations, test isolation and CI direction                       |
| [Security](docs/security.md)                         | Validation, authorization, secrets and safe error handling                       |

## Working discipline

- Never include private data or secrets in external searches or tool requests.
- Read the relevant canonical guidance and affected files before changing them.
- Keep changes bounded to the requested task. Record consequential decisions and unresolved questions using the development workflow.
- Report what changed, checks actually performed, and remaining limitations. Never claim a missing or unrun check passed.
- Update the owning document when a convention changes; avoid parallel sources of truth.
