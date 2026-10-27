# Loop Engine

Status: proposed design; not implemented.

## Purpose

Advance product delivery through a controlled loop: observe → evaluate → propose → authorize → execute → verify. Each run must have a goal, an evidence snapshot, a policy version, a bounded action budget and an auditable result.

The engine consumes domain events and scheduled checkpoints. It must not repeatedly poll unchanged state or mistake lack of progress for permission to change product commitments.

## Sprint and Release

A Sprint has a team, goal, time window and planned work. A Release has a product version, agreed scope, target date, acceptance evidence and artifact manifest. A release can span multiple sprints; a sprint can contribute to several releases. Work completion, sprint closure and release publication are distinct transitions.

Store Sprint, SprintItem and ReleaseItem independently. Record scope changes, carry-over decisions and dependencies explicitly. Adding work must not silently alter a sprint goal or external delivery commitment.

## Authorization

| Action class | Default policy |
| --- | --- |
| Summarize evidence, detect blockers, draft proposals | Automatic within the actor's authorized data scope |
| Create internal draft tasks, request missing information | Automatic only when the team has enabled that specific action |
| Change assignees, sprint scope or planned release allocation | Require approval unless a specific bounded team policy delegates the action |
| Change customer commitments, publish or withdraw a release, deploy to production | Explicit authorized human approval for the exact proposed action |

An AI suggestion does not grant authority. Approval binds to a particular scope revision, proposed diff, policy and expiry; changing the proposal invalidates approval. Publication must still pass business release gates.

## Run and action records

LoopRun records trigger, organization, product, goal, evidence revision, policy revision, limits and final result. LoopAction records target, preconditions, proposed change, approval, idempotency key, external result and verification evidence.

Run states: pending → evaluating → awaiting_approval / executing → verifying → completed. A no-change decision completes with a recorded reason. Failures may retry only within limits; stale evidence requires re-evaluation. Paused, failed and cancelled runs remain visible for audit.

## Safeguards

- Restrict each run to its organization, products and actor permissions.
- Apply version checks immediately before writes; do not execute an approved plan against changed facts.
- Deduplicate incoming events and outgoing commands. Use an outbox and stable idempotency keys across retries.
- Bound retries, time, cost and number of actions. Escalate when progress is blocked or confidence is insufficient.
- Tag engine-originated events and ignore no-op updates to prevent feedback storms and oscillation.
- Use deterministic rules for transition eligibility; use model output for interpretation and proposals.
- Verify external state after writes. An accepted command is not proof that the intended outcome occurred.
- Keep dry-run, pause and manual override available. Do not claim automatic rollback for irreversible external actions.

## First acceptance cases

1. A blocked dependency produces one proposal with its evidence and affected sprint/release scope.
2. A duplicate webhook causes no duplicate task, notification or scope change.
3. A release-scope change waits for approval; changed source data invalidates that approval.
4. A failed verification stops downstream actions and records the failure.
5. A run with no actionable change completes without changing scope or repeatedly notifying users.
6. Work can carry over to a subsequent sprint without marking the related release as published.
