# Codex planning adapters

These project-scoped adapters explicitly load the canonical shared roles and
planning skill. They contain only identity, selection description, and bootstrap
instructions. Shared methodology, review procedure, and output contracts remain
in `.agents/`; no copied skill, symlink, model pin, or project config is needed.

| Native agent name | Shared contract |
| --- | --- |
| `implementation-strategist` | [Strategist](../.agents/roles/implementation-strategist.md) |
| `plan-reviewer` | [Reviewer](../.agents/roles/plan-reviewer.md) |

Both read [the planning skill](../.agents/skills/implementation-planning/SKILL.md)
and [AGENTS.md](../AGENTS.md), then their required canonical references before
acting. A missing root or required file stops loading with an explicit error;
catalog selection is not a substitute for these reads.

## Start and invoke

Launch a fresh Codex session from the repository root after adding or changing
adapters. For the CLI, choose a sandbox that permits assigned artifact writes:

```sh
codex --sandbox workspace-write
```

Ask the parent to invoke the exact registered agent by name, rather than merely
act like that role. Supply the repository root, source paths, scope, current
authorization, and exact permitted output path under `workspace/`. State whether
replacement of an existing output is authorized. Provide needed repository-state
evidence from an authorized coordinator/verifier; the roles cannot run Git.

For example, a planning parent assignment can say:

> Invoke the registered `implementation-strategist` child for this repository.
> Use the supplied issue path and shared guidance. Write only the assigned local
> plan artifact; do not implement changes. Wait for the child and report its
> result and observable loading evidence. Do not substitute a generic agent.

Replace “supplied” and “assigned” with actual paths in the assignment. A missing
input should result in a request for it, not a guessed path.

## Independent reviewer invocation

Follow the shared [independent-review procedure](../.agents/roles/plan-reviewer.md#independent-review-procedure)
and [workflow gates](../docs/development-workflow.md). The default native path is
a fresh review-only top-level Codex session that invokes `plan-reviewer` as its
child with the bounded input packet and assigned review destination. Never reuse
the strategist authoring conversation as that default parent.

Record the parent assignment and identity, child prompt and identity, available
launch/context configuration, explicit file reads, and any evidence limitations.
Apply the shared evidence standard before counting independence; an opaque or
unavailable child-context record must be disclosed. Another demonstrably isolated
child path remains permitted. A child's failure to repeat a parent fact alone
does not establish isolation.

If the native path cannot establish sufficient evidence, use the shared manual
fresh-session or human fallback. In Codex, manually request explicit reads of the
same shared role, skill, and governing references. Manual use does not establish
native adapter registration, invocation, or loading support. The role still writes
its assigned artifact directly; the parent must not save it on the role's behalf.

## Permissions, settings, and trust

The adapters inherit model/reasoning choices and permissions; they do not set or
expand them. Choose settings in the parent session. For reproducible evidence,
record actual model/reasoning settings or their unavailability, client version,
invocation, sandbox/approval mode, and output paths. No repeated runs or model pins
are required. [Official custom-agent documentation](https://learn.chatgpt.com/docs/agent-configuration/subagents).

`workspace-write` permits more than the assigned artifact. Shared role instructions
constrain intended writes but do not create a path-specific filesystem sandbox.
Check actual read/write activity and before/after contents. Keep `.git`, shared
guidance, application files, and unassigned artifacts unchanged. Use no bypass
flags to overcome a denied write, and report failed prerequisites explicitly.

Codex loads project configuration subject to project trust. Verify actual loading
for the repository and any disposable fixture; do not assume inherited trust or
change personal configuration to make a test pass. If a role is absent or project
configuration is ignored, report that limitation before claiming native support.
[Official configuration basics](https://learn.chatgpt.com/docs/config-file/config-basic).

## Validation and diagnostics

Implementation-time validation uses Codex CLI `0.167.0`. Runtime outcomes and
limitations must be recorded with each task's local verification evidence; a
version string, TOML parser, or displayed role name alone does not prove execution.
Broader role behavior, implicit selection, and artifact-write negative cases are
the later validation task's responsibility.

Bounded smoke runs observed both named native children explicitly reading their
shared role, skill, and governance, then directly writing separate assigned plan
and review artifacts. A missing-skill fixture run stopped without writing output.
The default reviewer used a fresh `exec` parent whose recorded assignment and
history contained no strategist authoring conversation; the child inherited that
review-only parent. This supports that invocation path, not arbitrary parent forks
or proof of every possible upstream context source. Keep evidence limits visible
and obtain independent task verification before proceeding under the workflow.

The installed CLI offers `codex debug prompt-input` for inspecting model-visible
registration; avoid publishing its full output because it can include unrelated
context. Confirm both names and actual runtime reads separately. A trusted root
with loaded adapters does not prove that an unrelated fixture project is trusted.

In the tested CLI, a fresh `codex exec --ephemeral` parent failed child startup
with “no rollout found for thread id”. Use a fresh normal session for child smoke
checks; it creates ordinary runtime session storage, not a personal config change.
Do not resume an authoring session to work around the error. Treat this as an
observed client-version limitation, not a portable requirement of the shared roles.

If an adapter cannot read a required shared file, retain the exact failure and
confirm it did not write an artifact or silently use generic instructions.
Keep missing-reference tests inside disposable fixtures rather than renaming
repository files. When checks are unavailable, record them as unverified and stop
at the current task boundary under the workflow.
