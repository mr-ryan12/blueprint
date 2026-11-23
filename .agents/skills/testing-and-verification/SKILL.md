---
name: testing-and-verification
description: Select checks for an assigned behavior or failure mode, assess fixture isolation and report reproducible verification evidence in planning, implementation or review work.
---

# Testing and verification

Start with the claim the task needs to establish. State what a caller,
contributor or operator should observe and what could falsely appear to
satisfy it. The assignment determines whether to propose a check, run it or
assess someone else's evidence, and which outputs the role may write. Consult
the [workflow](../../../docs/development-workflow.md#planning-and-implementation)
for independent verification and approval gates.

## Choose a check that can distinguish the outcomes

Trace the behavior to the boundary where success and failure differ. Choose
the smallest check that crosses that boundary. For example, a successful
submission message can coexist with a failed write. If persistence is the
claim, read the stored result through an independent observation.

| Claim | Discriminating observation |
| --- | --- |
| A domain rule rejects an invalid transition | Exercise the rule with the nearest valid and invalid inputs and compare outcomes. |
| A form retains entered text after failure | Submit via its controls and inspect the value and accessible feedback after the response. |
| PostgreSQL enforces a constraint | Trigger the conflict with real rows in an isolated target and inspect the resulting state. |
| A route renders on a direct visit and revalidates after mutation | Observe the initial response and the subsequent read through the browser and server boundary. |
| Contributor instructions resolve to real inputs | Follow paths and anchors from the document and inspect the target content. |

Look up actual repository entry points, dependencies and prerequisites before
naming a command. For instruction-only changes, inspect new files directly
and use the [bootstrap checks](../../../docs/testing.md#current-bootstrap-checks).
For application behavior, consult the [application checks](../../../docs/testing.md#application-verification-once-tooling-exists).

## Derive cases from the failure mechanism

Choose data that would expose a plausible mistake in the changed behavior.
For a resource filter, use two distinguishable resources. For a boundary
condition, compare the last accepted and first rejected values. For a failed
write, observe both returned feedback and the state after the attempt.

Check whether the fixture makes success inevitable. A stubbed repository
that always returns the expected record can exercise how a caller renders
that record. It cannot expose a missing filter in the real query. Move the
observation to the layer that owns the filter or qualify the claim.

Select a browser case when routing, focus, SSR or multiple systems must work
together. Use lower layers for permutations of a pure rule. This keeps a
failure near its cause while still exercising the critical integrated path.

## Isolate the resources a check can change

Identify the candidate, working directory, fixture paths, build outputs,
caches and external resources a check uses. Compare the resolved targets
with the assignment's permitted side effects. A command advertised as a
check may rewrite snapshots, generated types or the candidate itself.

For a database case, inspect how the test process resolves its connection
configuration, including defaults and overrides. Establish that it selects
the dedicated test database or schema before setup or cleanup can write.
If the target cannot be identified safely, leave the check unrun and record
the missing prerequisite without exposing connection credentials.

Consider parallel workers, retries and interrupted teardown. Give each run
a distinguishable fixture and verify cleanup affects only its own resources.
If an application opens its own connections, a transaction held by the test
process alone may fail to isolate the application's writes.

## Make checks repeatable

Control time, ordering, generated identities and external responses when
they affect the observation. Substitute at the external boundary and keep
the behavior under test intact. A test expecting the newest row can pass by
accident if the fixture is the only row or all timestamps compare equal.

Wait for the observable state transition or request completion. A fixed
delay can hide stale UI or intermittent failures. When a retry succeeds,
record the initial failure and what changed between attempts. A repeat with
the same inputs gives different evidence from a run with a repaired fixture.

## Record a result another person can reproduce

For each material claim, retain the following in the assigned handoff:

- The base and candidate identity available to the role, including relevant
  untracked inputs, and any change between the first run and a rerun.
- The command and working directory or exact manual interaction, the
  prerequisites and synthetic fixture, and what was observed.
- The expected outcome, actual result and any side effects that matter for
  repeating the check. Identify the failing step if setup ends early.
- For a skipped or unavailable check, the reason and the part of the claim
  that remains open. A missing runner leaves different evidence from a
  runner that executed the case and returned a failure.

When evaluating supplied evidence, compare its candidate and inputs with
the assignment. If they differ, identify which results still apply and which
need fresh evidence. For an independent review, use permitted checks and
place any proposed correction in the assigned findings.

## Example assignment

A reviewer receives a green integration run that reports zero executed
database cases. Inspect selection, prerequisites and skipped-case output.
Identify whether the claimed database behavior was exercised and document
the gap in the review. A planner can use the same reasoning to specify the
precondition and expected observation for a future integration check.
