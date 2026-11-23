---
name: application-engineering
description: Trace strict TypeScript and React Router SSR feature changes across route, domain and server boundaries when planning, implementing or reviewing an assigned application task.
---

# Application engineering

Start with one route behavior from the assignment. Identify who requests it,
what the first response should contain and what can change after submission.
Use that trace to scope a plan, make an assigned edit or assess a candidate.
The assignment determines the output and writable paths.

## Establish the available contracts

Look for the manifest, framework configuration, route registration and nearest
implemented feature. Follow one route from its entry point to its data source.
Locate the types the installed tooling generates and the way callers consume
them. A proposed module name in a plan is a design input until it exists.

If the application has yet to be bootstrapped, describe the contract to be
established and the evidence a later implementation can supply. Keep sample
imports, generated types and scripts out of runnable instructions until their
sources exist. Check the [bootstrap status](../../../docs/architecture.md#current-state-and-intended-stack).

## Trace a read through the first render

For a direct request, identify the URL inputs, the loader responsible for
them, the data required to render and the fields that can cross to the client.
Then follow navigation to the same route. Compare what the user sees and how
the framework obtains fresh data in each path.

- If meaningful content first appears after a browser effect fetches it,
  inspect the initial HTML. Could the server loader supply that same data
  once, including on a direct visit?
- If rendering reads the clock, browser storage or viewport, compare the
  server's initial output with the browser's first render. Choose when the
  browser can safely enhance that output.
- If the record contains internal fields, inspect the actual return value.
  A narrower TypeScript annotation alone does not remove fields at runtime.

## Follow a write through revalidation

Identify the form or fetcher, its target action and the loader whose data
should change. Choose navigation when the user should land elsewhere and a
local interaction when the surrounding screen should stay in place. Check
the behavior with the installed framework's submission and revalidation APIs.

Trace a successful write to the next visible read. If the action persists a
record but the screen stays stale, inspect which loader the view consumes,
whether it revalidated and whether copied component state masks its result.
Add manual cache or state synchronization only for a demonstrated gap.

Trace an invalid submission along the same path. Locate the mapping from
validation failure to field feedback and confirm that a rejected request
leaves persistence untouched. The failure's transport shape needs to survive
the return trip to the form.

## Place code by its callers and lifetime

In the approved feature-oriented layout, group behavior that changes with
the feature. Start at a thin route and follow imports toward domain logic
and persistence. Keep a rule in a service when it has meaning outside its
route. Check the ownership of queries at the server-only repository boundary.
Use the [application boundaries](../../../docs/architecture.md#application-boundaries)
to resolve placement questions.

Trace imports in the opposite direction from each browser entry. Inspect
re-exports and shared helpers for accidental dependencies on a database
client, credentials or server startup. Check the framework's enforced server
module boundary and the actual client build when available.

Distinguish request data from process resources. A shared connection pool
may outlive many renders. A user's headers and request context belong to the
individual operation. Inspect initialization during build, startup and request
handling when a module has side effects at import time.

## Use strict types to expose disagreements

Follow a value from external input through parsing, persistence and the
returned data. Find the first place where the caller knows less than the
callee assumes. Narrow there using evidence available at runtime. Then use
inference through the rest of the path. Consult the [TypeScript conventions](../../../docs/conventions.md#typescript).

For example, if a route key can be absent, trace absence to the intended
response before passing the key into a query. An assertion at the call site
can make the signature compile while leaving the missing-input path broken.
Check both branches using the project's actual route types when available.

## Preserve framework control flow

For each catch boundary, follow a redirect, an expected response and an
unexpected exception to their destinations. A generic catch around an entire
action can turn a successful redirect into a failure page. Locate the narrow
operation whose failure needs translation and let the framework handle its
own responses. Consult [errors and observability](../../../docs/security.md#errors-and-observability).

## Evidence for the assignment

A plan can name the route and observable outcome that will settle an open
design choice. An implementation can exercise direct SSR, the submission
and the subsequent read for the affected feature. A review can cite the first
broken boundary, its caller and the visible effect in the assigned findings.
Use [application verification](../../../docs/testing.md#application-verification-once-tooling-exists)
to select the checks available for that candidate.
