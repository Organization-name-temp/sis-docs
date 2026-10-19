# Integration infrastructure

[PRD index](../../README.md) · [System parts](../../README.md)

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| INT-01 | R1 | Versioned REST API with OpenAPI, pagination and error schema | Contract/schema verification in CI; T-INT-01 |
| INT-02 | R1 | Transactional domain outbox + idempotent webhook dispatch | No published registration event for rolled-back booking; T-INT-02 |
| INT-03 | R1 | At least one demo integration: LMS-like roster projection or equivalent | Enrollment add/drop sync and retry demonstration; T-INT-03 |
| INT-04 | R1 | Audit requests, auth failures and cross-service correlation IDs | Trace test follows applicant→student→enrollment; T-INT-04 |
| INT-05 | R2 | Data import staging, mapping, dry run and reconciliation | Duplicate/unmapped rows reported before commit; T-INT-05 |
| INT-06 | R2 | Align SIS academic data exchange to applicable EduAPI profile | Mapping validated against chosen version; T-INT-06 |
| INT-07 | R2 | LTI/LMS launch only for supported product/tool role | Conformance tests against selected version; T-INT-07 |

## Acceptance scenarios

**AC-017 | Integration down during registration**  
**Given** roster subscriber unavailable  
**When** a valid enrollment commits  
**Then** student registration succeeds; event is retained and safely re-delivered when subscriber recovers with no duplicate roster enrollment.  
**Covers:** INT-02, INT-03, OPS-04; **Proof:** forced outage/replay.

## Related acceptance coverage

[AC-004](../../modules/07-student-lifecycle.md), [AC-007](../../modules/09-course-registration.md). Scenario ownership and complete coverage are listed in the [acceptance index](../acceptance-index.md).

## Shared contracts and release gates

Apply the shared [domain invariants](../architecture.md), [API contracts](../api-contracts.md), [screen inventory](../screens.md), [quality requirements](../quality.md), [traceability](../traceability.md) and [release gates](../release-plan.md). Requirement IDs, release assignments and T-* families remain unchanged by this organization.
