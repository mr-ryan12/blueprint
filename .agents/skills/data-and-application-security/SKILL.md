---
name: data-and-application-security
description: Assess how application input, resource access, Prisma queries, PostgreSQL changes and request data cross trust boundaries in an assigned design, implementation or review.
---

# Data and application security

Choose an operation affected by the task. Follow its input to a decision,
a database effect and the information returned or logged. Use the trace in
the assigned output. A review produces findings and a planning assignment
identifies decisions. Each keeps its original write scope.

## Identify who controls each value

Begin with a route parameter, form field, environment value or upstream
response. Record its source and the first consumer that relies on its shape.
Consider a missing field, a repeated value and a value that is well formed
but invalid for the operation. Which of them could reach a write?

Find the parser and follow its output into the query. For example, a form
can submit fields that its rendered controls never offered. Inspect which
validated fields become persistence arguments. A parse followed by a write
of the original input leaves the write exposed. Use the [trust boundary guidance](../../../docs/security.md#trust-boundaries).

Separate a valid identifier from permission to act on its resource. Determine
the public or protected status specified for this operation. For protected
data, locate the trusted source of actor or tenant identity and follow it
into the resource lookup or mutation predicate. For an explicitly public
feature, evaluate its intended exposure and server-side availability controls.
Leave changes to access policy or authentication design to their owner.

## Examine the repository operation

Find the Prisma call that determines what PostgreSQL reads or changes.
Identify its filter, selected columns, constraints and return value. Compare
them with the route's promise. A broad read narrowed only in a component can
expose fields in the serialized response. A broad mutation can affect more
rows than the caller's success message suggests.

Consider two requests against the same precondition. If both can pass an
initial lookup, identify what enforces the rule at write time. Depending on
the invariant, a database constraint, a conditional mutation or an atomic
transaction may be appropriate. Check what the caller receives when another
request wins. Keep database mechanics with their [architectural owner](../../../docs/architecture.md#application-boundaries).

For a retry after an ambiguous result, trace whether the original change may
already have committed. Identify the user-visible result of replay before
suggesting a retry. The consequence differs between reading an existing row
and creating a second one.

## Reason about migrations with existing rows

Read the schema delta alongside the generated SQL. Describe which existing
rows the new shape can represent. A required column added to a populated
table raises two questions. Where does each prior row's value come from,
and which version of the application can supply values during rollout?

Trace readers and writers on each side of the change. Check defaults, unique
constraints, nulls and lock duration where they affect compatibility. For a
destructive operation, identify what recovery would need after data is lost.
Reverting application code alone may leave the database incompatible.

Use a populated, isolated test target for a migration rehearsal when the
assignment permits it. Record what happened to representative prior rows.
Consult [infrastructure and deployment](../../../docs/architecture.md#infrastructure-and-deployment)
for migration ownership and the separate production execution path.

## Check isolation across two requests

Follow two simultaneous requests with different identities through shared
dependencies. Identify where each credential, destination and correlation
identifier is attached to the outbound operation. Inspect mutable defaults
on long-lived clients and callbacks that capture a previous request's data.

Use distinct synthetic markers if an isolated check is available. Observe
which destination receives each marker and whether either appears in a
response or log from the other request. This probes a leak that two requests
executed one after the other may miss.

## Trace an unsuccessful operation

Follow invalid input, a missing or inaccessible resource where applicable,
and an unexpected database failure. Identify the public response and where
diagnostic context is recorded. Follow any framework response or redirect
through exception handling. Check the [error and logging rules](../../../docs/security.md#errors-and-observability).

Inspect what the logger actually emits, including nested errors and context
added by middleware. An error may carry connection details even when its
top-level message looks safe. With synthetic values, compare the emitted
record and the public response. Retain only sanitized evidence.

## Choose evidence for the affected boundary

- For a parser change, pair a valid example with an input that would reach
  the wrong effect if validation were bypassed. Check persistence as well as
  the response.
- For a query change, inspect the actual predicate and exercise the affected
  invariant with isolated rows. A mocked success cannot demonstrate a
  database constraint.
- For a failure path, identify the request consistently in the permitted
  diagnostics and inspect the information available to its caller.

Use [testing guidance](../../../docs/testing.md#application-verification-once-tooling-exists)
for isolation and available checks. Identify any unexercised failure and its
effect on the assigned assessment.
