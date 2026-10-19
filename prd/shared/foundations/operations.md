# Operations and recovery

[PRD index](../../README.md) · [System parts](../../README.md)

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| OPS-01 | R1 | Structured logs, metrics, alerts and request traces | Demonstrated detection of auth failure and booking error; T-OPS-01 |
| OPS-02 | R1 | Encrypted/controlled backups and restore rehearsal | Sample restore and data-consistency checks pass; T-OPS-02 |
| OPS-03 | R1 | Secrets handling and isolated environments | No credentials in source/CI logs; T-OPS-03 |
| OPS-04 | R1 | Background job retry/dead-letter operations | Failed dispatches recover without double application; T-OPS-04 |
| OPS-05 | R1 | Documented deployment/rollback and incident playbook | Clean deploy and rollback rehearsal pass; T-OPS-05 |
| OPS-06 | R2 | Region-specific retention, residency and privacy configuration | Legal/privacy review per jurisdiction, deletion/archive cases; T-OPS-06 |

## Acceptance scenarios

**AC-020 | Backup and restore**  
**Given** approved backup in isolated storage  
**When** disaster-restore runbook executes on fresh environment  
**Then** essential academic record, version histories and event-state consistency reconcile, secrets are rotated and restoration meets documented trial objective.  
**Covers:** OPS-02, OPS-05; **Proof:** signed restore rehearsal and discrepancy report.

## Related acceptance coverage

[AC-017](integrations.md). Scenario ownership and complete coverage are listed in the [acceptance index](../acceptance-index.md).

## Shared contracts and release gates

Apply the shared [domain invariants](../architecture.md), [API contracts](../api-contracts.md), [screen inventory](../screens.md), [quality requirements](../quality.md), [traceability](../traceability.md) and [release gates](../release-plan.md). Requirement IDs, release assignments and T-* families remain unchanged by this organization.
