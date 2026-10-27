# Development roadmap

All items below are planned, not completed. Milestones define exit criteria rather than promised delivery dates.

## M0 — Runnable foundation

- [ ] Select and pin application frameworks and runtime versions.
- [ ] Create web, API and worker directories with reproducible setup instructions.
- [ ] Add local database and object storage setup, migrations and synthetic demo data.
- [ ] Implement login, organizations, memberships and product access controls.
- [ ] Add automated formatting, meaningful core tests and CI.
- [ ] Decide repository license and target hosting environment.

Exit: a contributor can start a clean local environment, sign in and access only authorized products.

## M1 — Discovery and requirement intake

- [ ] Persist conversations with access controls.
- [ ] Add a model-provider adapter and structured requirement drafts.
- [ ] Require human confirmation before intake.
- [ ] Add requirement list, filters, review, comments and duplicate merging.
- [ ] Preserve source links and audit history.

Exit: one conversation becomes a reviewed requirement; merged sources remain traceable.

## M2 — Design and delivery planning

- [ ] Add solution revisions, review decisions and acceptance criteria.
- [ ] Add delivery items for partial delivery across releases.
- [ ] Add releases, tasks, owners and dependencies.
- [ ] Add independent sprints, iteration goals and explicit carry-over records.
- [ ] Add manual Issue / PR / commit links.
- [ ] Add a dry-run Loop Engine that detects blockers and proposes changes with evidence.

Exit: an approved requirement can be traced through its design and planned delivery items to tasks and releases.

## M3 — Acceptance and release center

- [ ] Record acceptance evidence against scope revision and build.
- [ ] Add blocking defects, rework and re-acceptance.
- [ ] Add package uploads, checksums and required release documents.
- [ ] Enforce server-side release gates and immutable published manifests.
- [ ] Add release withdrawal and authorization-aware downloads.

Exit: a failing acceptance blocks publication; an approved version publishes its exact package and materials.

## M4 — Knowledge and FAQ loop

- [ ] Add review and audience controls for release knowledge.
- [ ] Add version-aware retrieval with citations.
- [ ] Handle insufficient evidence and revoked knowledge.
- [ ] Convert FAQ feedback into human-reviewed requirement drafts.
- [ ] Verify the full workflow and cross-organization isolation.

Exit: the FAQ answers from authorized released knowledge and new feedback returns to intake.

## M5 — Enterprise hardening

- [ ] Add enterprise identity integration and configurable approval policies.
- [ ] Add scoped GitHub App integration and webhook processing.
- [ ] Implement the connector contract with one pilot-selected read-only enterprise adapter.
- [ ] Add approved Loop Engine actions, stale-plan rejection, bounded retries and outcome verification.
- [ ] Define and validate performance, backup/restore and recovery targets.
- [ ] Add operational monitoring, retention controls and deployment documentation.

## First vertical slice

Use synthetic data: a fictional customer asks for scheduled report export. Capture the conversation, confirm the requirement, approve a proposal, schedule a delivery item, attach development links, record a failed and then passed acceptance, publish a sample package and release notes, answer a version-specific FAQ question, and convert a follow-up request back into intake.
