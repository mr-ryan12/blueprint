---
name: frontend-engineering
description: Translate an assigned user flow into responsive, accessible interface behavior and assess its loading, submission, recovery and success states during design, implementation or review.
---

# Frontend engineering

Choose a concrete user goal and follow it from first arrival to completion.
Describe the action, the feedback and the user's next available action.
Use this reasoning for the assigned deliverable. A review cites observed
behavior and a plan specifies acceptance observations. Edits require an
implementation assignment covering the affected paths.

## Inspect the interface the user already has

Locate the route, the source of its data and any existing component used for
a similar interaction. Inspect its actual exports, props, rendered semantics
and styling tokens. A component name or an intended design library alone
does not tell you whether a label, pending state or keyboard action works.

Check which component owns state and which system confirms the outcome.
A button may know it was pressed while only the action response can establish
that the write succeeded. Use the [UI completion conventions](../../../docs/conventions.md#ui-completion)
when assessing the flow.

## Walk the states that affect this goal

| Moment | Question to answer in the interface |
| --- | --- |
| Initial loading | What can the user identify or do before the required data arrives? |
| Successful empty read | How does the user recognize an empty result and find an available first action? |
| Submission pending | Which action is in progress, and which other controls still work? |
| Failed read | Can the user distinguish unavailable data from a successful empty result? |
| Failed submission | Can the user find the problem, retain valid input and try again? |
| Success | What changed, when is it visible, and where can the user continue? |

Apply only the states the actual path can enter. A direct server render may
already have its initial data. Inspect navigation and revalidation separately
before proposing a loading indicator. Replacing useful existing content
while refreshed data is pending can interrupt the user's current work.

For a list with per-row actions, identify which submission owns the pending
state. Slow one synthetic response in an authorized fixture and observe both
the affected row and its neighbor. A global busy flag can disable unrelated
actions or label the wrong row as in progress.

## Connect feedback to the action

Follow an unsuccessful form submission from submit to correction. Locate
the error in relation to the control, verify the accessible association and
check what value remains. A user who entered several valid fields should be
able to recover from one invalid value without rebuilding the form.

Decide where feedback belongs based on what the user must do next. A field
error needs an identifiable control. A page-level failure may need a retry
path. If feedback appears without focus moving, check whether a user relying
on assistive technology can discover it without repeated announcements.

For a successful mutation, observe the subsequent rendered data. Identify
whether the user remains on the form, sees a refreshed row or navigates to
another page. Drive success feedback from that confirmed outcome. Reconcile
any optimistic state with a rejected response when optimism is in scope.

## Walk the keyboard and focus path

Start at a recognizable landmark, navigate to the action, activate it and
continue after the response using the keyboard. Inspect names, roles and
visible focus along the way. Where a native control fits the interaction,
its built-in behavior gives a baseline for testing a composed component.

Consider what happens to focus when its current target disappears. After a
row deletion, find a logical next control or heading from which the user can
continue. For a validation error, confirm that users can reach the affected
field and understand what correction is needed.

Check meaningful information without relying on color or motion alone.
Test the supported reduced-motion behavior if a transition communicates
progress or reveals feedback. Check contrast in the affected states,
including disabled-looking controls that are actually still interactive.

## Change the space available

Follow the same flow at a narrow viewport and with enlarged text. Use long
labels, realistic content lengths and an error message that wraps. Inspect
reading order, control reachability and whether horizontal overflow hides
the action or its feedback.

At wider sizes check where a long line, large gap or distant status message
separates a control from its context. Evaluate the actual failure and success
states at these sizes. A screenshot of the initial desktop view alone gives
little evidence about recovery on a narrow screen.

## Capture the observation that matters

Use component-level interactions for state transitions they can reproduce.
Use a browser when layout, focus after navigation or actual revalidation is
material. Consult [application verification](../../../docs/testing.md#application-verification-once-tooling-exists)
for the available layers and record any state the fixture could not reach.

For example, a review of an empty-state change can trace a failed loader
through the view. If the view displays the first-use invitation on failure,
record the failing input, the misleading message and the distinction needed
for recovery. A planning assignment can use the same pair of outcomes as
acceptance criteria without assuming a particular component API.
