# Application Security

These requirements apply as the corresponding capabilities are implemented. Authentication and other unresolved technology choices remain in [architecture](architecture.md).

## Trust boundaries

- Validate external input at its boundary, including route parameters, query/form bodies, environment configuration and third-party data used by the application. Use Zod where appropriate; TypeScript types do not validate runtime values.
- Define public and protected operations explicitly. Enforce authentication where required and authorization for the actual resource/action on the server; scope access to the owner or tenant where applicable.
- Keep secrets and privileged capabilities out of client bundles. Use placeholders in environment examples; never commit credentials.
- Keep HTTP credentials scoped to the intended destination. Assess server-side request forgery (SSRF), injection, cross-site request forgery (CSRF), upload risks and, where relevant, XSS/content injection when introducing endpoints or integrations.

## Errors and observability

- Distinguish expected validation failures, framework redirects/responses and unexpected errors. Catch errors when handling or translating them adds value; preserve framework control flow instead of turning every failure into a generic response.
- Return safe user-facing errors without stack traces or internal details. Log unexpected failures with useful context and request/correlation IDs, avoiding duplicate reporting at every layer.
- Redact credentials, cookies, authorization headers and sensitive personal data. Request/response logging should use selected safe metadata rather than raw payloads by default.
- Validate required environment values at startup; non-null assertions do not provide fail-fast validation.

## Change review

Assess access-control, secret exposure and data-loss implications for affected changes. When introducing or upgrading packages, assess dependency and supply-chain security risks; dependency selection and justification remain in [conventions](conventions.md). Include relevant failure-path tests under [testing](testing.md) and follow the migration safeguards in [architecture](architecture.md).
