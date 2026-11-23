---
name: delivery-operations
description: Evaluate reproducible setup, native development prerequisites, CI evidence, container behavior and manual migration readiness within an assigned delivery task.
---

# Delivery operations

Identify the delivery claim and its target. Installing from a fresh checkout,
booting a production image and observing a required check on a reviewed
revision each need different evidence. Use this method in the assigned plan,
implementation or review. The assignment supplies authority for any command,
external access or change.

## Reconstruct the entry point

Read the manifest, version declarations, lockfile and contributor instructions
that actually exist. Trace how the runtime and package manager are selected
before dependencies are resolved. A developer's existing global tool can
hide a missing prerequisite on a fresh machine.

Compare what a clean install consumes with what the build and runtime need.
Look for generated files left in a developer checkout, unrecorded environment
defaults and a lockfile that changes during installation. When an isolated
rehearsal is authorized, capture the exact revision, versions and the first
step at which another contributor could observe success or failure.

If the bootstrap task has yet to supply an entry point, identify the missing
file or prerequisite. Refer to the [current stack and bootstrap status](../../../docs/architecture.md#current-state-and-intended-stack)
when separating planned commands from those available in the candidate.

## Trace local prerequisites

For native PostgreSQL development, distinguish installing the software,
starting the service, selecting a local target and preparing its schema.
A reachable server may still have the wrong database, permissions or schema
revision. Identify the smallest observation that locates the failing step.

Follow how the process receives configuration in each mode. Compare the
documented placeholder names with the configuration reader and note when
validation runs. Capture the failing variable's name and stage while keeping
its value out of evidence. Use [security guidance](../../../docs/security.md#errors-and-observability)
for diagnostic handling.

## Follow an artifact into the container

Trace source, dependencies, generated code, server output and static assets
from the build context into the final image. Check what the runtime entry
point resolves relative to its actual working directory. A successful build
stage can leave an image missing a production dependency or a client asset.

Consider the boundary between build-time and runtime configuration. Identify
any import that opens a listener, connects to a database or reads a deployment
secret during compilation. Check what configuration and local files are
copied into the build context and what survives in image layers.

Use an isolated container check when permitted and available. Observe startup
with the documented runtime inputs, an actual request to the application and
termination. If the task distinguishes liveness from readiness, vary a
dependency and inspect both responses. An image build alone cannot exercise
those runtime behaviors. Keep host-specific assumptions visible when assessing
[deployment portability](../../../docs/architecture.md#infrastructure-and-deployment).

## Read CI evidence at the candidate revision

Start with the event, filters and job conditions that select the workflow.
Trace the command, installed prerequisites and relevant services through to
the reported result. A successful wrapper can conceal a skipped test suite.
A workflow file can also exist without a run for the event under review.

Distinguish three observations when reporting a check:

- Repository configuration identifies which job is defined and when it is
  intended to run.
- An observed remote run identifies the event, candidate revision, jobs
  executed and their outcomes. Record that evidence when available.
- Enforcement depends on the active remote controls. A successful run alone
  leaves open whether merging depends on that check.

Report pending, failed, skipped or inaccessible evidence precisely. Inspect
[CI direction](../../../docs/testing.md#ci-direction) and the assigned acceptance
criteria before deciding which gap matters. Keep remote inspection and any
settings change within the authority granted for the task.

## Assess manual migration readiness

Trace how the separately triggered migration operation selects its environment,
obtains its connection and checks the intended target before applying a
reviewed migration. Identify the operator-visible evidence of the target,
migration revision and result without including credentials.

Compare the operation with application startup and replica scaling. Starting
a second instance should not launch a second migration. If a migration stops
partway, identify which database state is observable, which application
versions can still run and what recovery information the operator needs
before deciding on a retry. Consult the [migration policy](../../../docs/architecture.md#infrastructure-and-deployment).

Evaluate the procedure using configuration inspection or an explicitly scoped
rehearsal against disposable data. A live migration or deployment requires
specific authorization naming the operation and target. A review records
gaps and leaves execution to the authorized assignment.

## Handoff

Tie each conclusion to its source. For example, a locally built image and
a passing local request support the tested runtime path. Establishing that
the same artifact was deployed requires evidence of its identity at the
destination. Record the missing observation if that link cannot be made.
