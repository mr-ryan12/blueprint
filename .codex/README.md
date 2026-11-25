# Codex project adapters

These project-scoped adapters explicitly load shared roles and skills. They
contain only identity, selection description, and bootstrap instructions.
Shared methodology, review procedure, and output contracts remain in
`.agents/`.

| Native agent name | Shared contract |
| --- | --- |
| `implementation-strategist` | [Strategist](../.agents/roles/implementation-strategist.md) |
| `plan-reviewer` | [Reviewer](../.agents/roles/plan-reviewer.md) |
| `fullstack-engineer` | [Fullstack engineer](../.agents/roles/fullstack-engineer.md) |
| `task-verifier` | [Task verifier](../.agents/roles/task-verifier.md) |
| `code-reviewer` | [Code reviewer](../.agents/roles/code-reviewer.md) |

Every adapter requires complete reads of [AGENTS.md](../AGENTS.md), its assigned
shared role, and relevant canonical guidance before acting. The two planning
adapters also read [the planning skill](../.agents/skills/implementation-planning/SKILL.md)
in full. Use explicit file reads. If the supplied root or a required file is
missing or unreadable, report the exact input and stop.

## Start and invoke

Launch a fresh Codex session from the trusted repository root that owns the
project registry after adding or changing adapters. For the CLI,
`workspace-write` is an option when it permits the assigned writes.

```sh
codex --sandbox workspace-write
```

Ask the parent to invoke the exact registered agent by name. Supply the assigned
repository root, source paths, scope, current authorization, and exact writable
paths. For a disposable fixture, start from the trusted registry root and give
the child the fixture root explicitly. The adapter resolves shared reads from
that assigned root. Record both roots when they differ.

For planning assignments, give the exact output path under `workspace/` and
state whether replacement is authorized. Supply repository-state evidence from
an authorized coordinator or verifier. The strategist and plan reviewer cannot
run Git, including read-only queries.

For example, a planning parent assignment can say:

> Invoke the registered `implementation-strategist` child for this repository.
> Use the supplied issue path and shared guidance. Write only the assigned local
> plan artifact; do not implement changes. Wait for the child and report its
> result and observable loading evidence. Do not substitute a generic agent.

Replace “supplied” and “assigned” with actual paths in the assignment. A missing
input should result in a request for it, not a guessed path.

## Implementation and review assignments

Supply the source issue, approved plan and revision, bounded assignment, and
permitted check side effects. Pass the exact role name and the following
role-specific inputs to the child.

| Role | Assignment and permitted writes |
| --- | --- |
| `fullstack-engineer` | Give one approved task, its acceptance criteria, and the exact implementation paths. Give the exact path under `workspace/` if a written handoff is required. |
| `task-verifier` | Give one task, its baseline and exact candidate, complete task delta including untracked files, implementation evidence, and the sole verification output path under `workspace/`. Include the findings and cited evidence for a finding-validation assignment. |
| `code-reviewer` | Give the baseline, exact entire PR or integrated candidate, full delta including untracked files, relevant durable decisions and evidence, and the sole comprehensive review path under `workspace/`. |

Specify replacement authority and any authorized build, cache or fixture
writes or isolated test resources. For the two review roles, explicitly
authorize read-only Git or supply timestamped coordinator evidence sufficient
to identify the complete candidate delta. Role invocation grants no Git
mutation authority.

All three adapters read [testing and verification](../.agents/skills/testing-and-verification/SKILL.md)
in full for check selection or evidence review. Name any additional skills
that apply to the assigned change, using their paths from the assigned root.

| Change area | Shared skill |
| --- | --- |
| Application and server boundaries | [Application engineering](../.agents/skills/application-engineering/SKILL.md) |
| Data and trust boundaries | [Data and application security](../.agents/skills/data-and-application-security/SKILL.md) |
| User flows and interface states | [Frontend engineering](../.agents/skills/frontend-engineering/SKILL.md) |
| Setup, CI and deployment | [Delivery operations](../.agents/skills/delivery-operations/SKILL.md) |

Ask the child to report the exact guidance paths it read. Retain the skill
selection from the assignment and observed file reads with the task evidence.

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

For `task-verifier` and `code-reviewer`, follow the same independence procedure
as applied by their shared contracts. Use a fresh review-only parent with the
appropriate task or whole-change input packet. Exclude implementation history
from both and task-verifier private history from comprehensive review. The
tested implementation-review path used fresh parents with `fork_turns="none"`.
Verify context provenance for each real handoff and use the fresh manual or
human fallback when the native path cannot establish it.

## Permissions, settings, and trust

The adapters inherit model/reasoning choices and permissions; they do not set or
expand them. Choose settings in the parent session. For reproducible evidence,
record actual model/reasoning settings or their unavailability, client version,
invocation, sandbox/approval mode, and output paths. No repeated runs or model pins
are required. [Official custom-agent documentation](https://learn.chatgpt.com/docs/agent-configuration/subagents).

`workspace-write` can permit writes beyond the assignment. The sandbox governs
host permissions and the assignment defines the allowed paths. Check actual
read/write activity and before/after contents. Preserve `.git` and unassigned
files, including protected fixture inputs and sentinels. Report denied writes
and other failed prerequisites explicitly. Use no bypass flags.

Codex loads project configuration subject to project trust. Verify actual loading
for the repository and any disposable fixture; do not assume inherited trust or
change personal configuration to make a test pass. If a role is absent or project
configuration is ignored, report that limitation before claiming native support.
[Official configuration basics](https://learn.chatgpt.com/docs/config-file/config-basic).

## Validation and diagnostics

Record the registry root, assigned root, project-trust and load results, exact
role requested and identity observed, and any error or unavailable evidence.
For a missing role, check the exact registered name, fresh startup from the
registry root, and actual project trust. Record client or trust failures against
the affected cases. A version string, TOML parse or displayed role name
provides static or registration evidence only.

### Implementation and review validation

Issue #7 fixture checks used Codex CLI `0.171.0` from the trusted repository
registry root with an explicitly supplied fixture root. The initial four
default gRPC attempts blocked on guidance reads. Subsequent runs used the local
tool transport via a session-local flag. The handoff runs used
`gpt-6.2-astra`, high effort, `workspace-write` and approval policy `never`.
The tested noninteractive launch was:

```sh
codex --disable code_mode_host_grpc -a never exec --strict-config --sandbox workspace-write --json --color never -
```

On the test host, the first reviewer handoff failed before session creation.
The outer sandbox blocked in-process app-server client initialization with
`Operation not permitted`. The separately authorized outer launch started
successfully with `--sandbox workspace-write` and `-a never` still in effect.
The gRPC guidance-read stalls occurred after startup.

Supply the bounded assignment on standard input. For native invocation, ask the
fresh parent to launch exactly the registered role and record the selected
settings and linked session identities. Establish standalone fixture trust
separately if its registry is needed.

Independent checks observed the engineer’s scoped edit, both reviewers’ direct
report writes on sound and flawed fixtures with concealed expected outcomes,
and task-verifier finding validation. Boundary checks covered missing inputs,
out-of-scope and redirected destinations, existing outputs and actual denied
writes. Protected fixtures remained unchanged. Ordinary planning, editing and
review sessions preserved their assignments. Planning and review skill reads
were observed, while skill selection for the edit remains unobservable.

For reviewer handoffs, the accepted native retries used distinct fresh parents
and isolated children with recorded linked launch provenance and bounded inputs.
An earlier running native task verifier returned incomplete independence evidence.
Exact plaintext equivalence of the encrypted child prompts remains unverified.
Separate fresh top-level sessions also completed both handoffs with explicit
manual loading. These results establish the tested fixture paths. Default gRPC
success and general sandbox enforcement of assigned paths remain unverified.

### Planning validation history

The planning-role observations below used Codex CLI `0.167.0` with GPT-6.1-Sol
and medium effort as invocation settings. Those results apply to the tested
configuration. Record new runtime outcomes and limitations with each task's
local verification evidence.

Bounded smoke runs observed both named native children explicitly reading their
shared role, skill, and governance, then directly writing separate assigned plan
and review artifacts. A missing-skill fixture run stopped without writing output.
The default reviewer used a fresh `exec` parent whose recorded assignment and
history contained no strategist authoring conversation; the child inherited that
review-only parent. This supports that invocation path, not arbitrary parent forks
or proof of every possible upstream context source. Keep evidence limits visible
and obtain independent task verification before proceeding under the workflow.

Further bounded validation observed ordinary planning select/read the skill and
write a plan, ordinary implementation perform its assigned text edit without
switching to planning, and ordinary review consult the plan contract without
rewriting the plan. No full skill read was observed in the implementation run;
unobservable internal selection is not inferred from the output alone.

The native reviewer detected each seeded unsupported-API, dependency-order, and
security/migration-verification defect. Manual shared-role sessions for both
roles rejected external/traversal or symlink-redirected destinations, preserved
existing outputs without replacement authorization, and stopped after actual
host-denied writes. All negative targets/sentinels stayed inside a disposable
fixture, whose source files remained unchanged. Permissions were restored after
the denied-write evidence was captured; no bypass or alternate writer was used.

These fixture runs loaded shared content under an explicitly supplied fixture
root while the CLI used the existing project registry. They do not establish
automatic trust/config loading for an unrelated standalone fixture project.

The tested CLI offered `codex debug prompt-input` for inspecting model-visible
registration. Its full output can include unrelated context, so retain only
the relevant evidence. Confirm the requested role name and actual runtime reads
separately. A trusted root with loaded adapters does not prove that an unrelated
fixture project is trusted.

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
