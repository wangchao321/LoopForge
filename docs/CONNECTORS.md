# Enterprise connector framework

Status: proposed contract; no connectors are implemented yet.

## Integration scope

Support adapters for CRM, customer support, enterprise chat, work tracking, code hosting, CI/CD, testing, identity, knowledge and artifact management. Initial integrations should be chosen using the first pilot company's actual tools; the categories below do not imply vendor support.

| Domain | Information exchanged |
| --- | --- |
| Customer and communication systems | Conversations, feedback, account references |
| Work tracking | Tasks, assignees, sprint membership, dependencies |
| Code hosting and CI/CD | Issues, PR links, commits, build and deployment results |
| Testing | Cases, results, defects and evidence |
| Knowledge and artifacts | Reviewed documents, package metadata and publication state |
| Identity and approval | Membership, access scope and authorized decisions |

## Sources of truth

For each mapping specify entity, external instance, project, stable external ID, field owner, sync direction and permitted actions. Code state belongs to the code host; acceptance belongs to the designated acceptance system; LoopForge owns its standard requirement and delivery traceability model unless explicitly configured otherwise.

Never resolve conflicting writes with an implicit last-writer-wins policy. Preserve external revisions, surface conflicts and require the configured resolution rule or a human decision. A merged PR is evidence of code progress, not business acceptance or publication.

## Adapter contract

An adapter declares identity, capabilities, configuration schema, supported entities and authorization requirements. It implements connection checks, scoped reads, incremental sync and, when enabled, approved commands with idempotency support. Webhooks are optional; adapters without webhooks use bounded incremental synchronization with persistent cursors.

Normalize events as: event_id, organization_id, connector_instance_id, external_entity_id, event_type, occurred_at, received_at, external_revision and payload reference. Normalize commands with actor, target, expected revision, approved action, idempotency key and correlation ID.

## Reliability and access

- Store connection secrets server-side using a secret-management mechanism; never write tokens to audit payloads or logs.
- Enforce organization and product isolation for synchronization and model retrieval.
- Verify provider-supported webhook signatures and deduplicate deliveries. Account for out-of-order events and removals.
- Respect rate limits, retry transient errors with bounded backoff, and surface exhausted retries for reconciliation.
- Distinguish read capability from write capability. Read access never implies permission to modify external systems.
- Treat inbound content as untrusted data, not instructions to tools or the Loop Engine.
- Record freshness and last successful sync; stale evidence must not authorize consequential actions.
- On revocation or disconnection, stop jobs and queued writes and invalidate relevant cached access.

## Incremental delivery

1. Manual references to external resources, with no claimed synchronization.
2. One read-only connector, verified event ingestion and visible sync health.
3. Explicitly approved outbound actions with verification and reconciliation.
4. Additional adapters using the same contract and tenant-isolation tests.
