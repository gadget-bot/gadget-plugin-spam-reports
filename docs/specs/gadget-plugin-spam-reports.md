# gadget-plugin-spam-reports Specification

## Purpose
Provide a standalone spam reporting workflow and emit optional reward events for engagement systems.

## Standalone and Optional Integrations
- Operates standalone for report intake, status tracking, and removal outcomes.
- Optionally emits reward events consumed by `gadget-plugin-engagement-ledger`.
- No hard dependency on scheduler or chat adapter.

## v1 Functional Requirements
1. Support spam report creation and lifecycle tracking.
2. Mark reports with final outcomes, including successful removal.
3. On successful removal, identify the first reporter for that content.
4. Emit `spam.report.resolved` event for optional reward processing.
5. Never emit duplicate reward-eligible resolution events for the same report/content pair.
6. No moderator-intervention gate is required for reward eligibility.
7. No historical backfill of prior spam reports.
8. This plugin owns spam-report reward trigger logic; Penny no longer owns that logic.

## Events and API Contracts
### Emitted
- `spam.report.resolved`
  - Required fields: `event_id`, `workspace_id`, `report_id`, `content_id`, `removed`, `first_reporter_user_id`, `resolved_at`

### Idempotency
- Event emission must be idempotent and replay-safe.
- At most one reward-eligible emitted event per unique resolved spam incident.

## Non-Goals in v1
- Direct point ledger writes.
- Leaderboard generation.
- Cross-workspace reward aggregation.

## v2 Targets
- Rich abuse taxonomy and finer-grained resolution metadata.
- Enhanced audit and operator dashboards.

## Extractable Issues
1. **Implement standalone spam report lifecycle and resolution model**  
   Milestone: `v1-core-engagement`  
   Labels: `type:feature`, `area:spam`, `priority:p0`, `standalone`
2. **Implement first-reporter selection on successful removal**  
   Milestone: `v1-core-engagement`  
   Labels: `type:feature`, `area:spam`, `priority:p0`
3. **Emit idempotent `spam.report.resolved` events for optional consumers**  
   Milestone: `v1-optional-integrations`  
   Labels: `type:api`, `area:spam`, `integration:optional`, `priority:p0`
4. **Document and enforce no historical backfill policy**  
   Milestone: `v1-core-engagement`  
   Labels: `type:docs`, `area:spam`, `priority:p1`
5. **Migrate spam reward trigger ownership out of Penny**  
   Milestone: `v1-optional-integrations`  
   Labels: `type:feature`, `area:spam`, `priority:p0`
