# Deferred mobile scope — not in the current release

**Status: DEFERRED by the user; no active release, implementation role or acceptance gate.** Native Flutter, offline operations, widgets, device push and AC-022/028/029 are removed from current delivery. A future explicit scope decision is required to reactivate them; the old R0.5/R1 timing is superseded. Responsive React web remains active.

[PRD index](../README.md) · [System parts](../README.md)

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| COM-03 | DEFERRED | Student mobile push registration/device mapping | Expired/revoked devices don't receive protected payload; T-COM-03 |
| MOB-01 | DEFERRED | Enrolled-student-only session and account routing | Applicant-only account cannot access student mobile functions; T-MOB-01 |
| MOB-02 | DEFERRED | Mobile home/current timetable and upcoming events | Correct current program, term, timezone; T-MOB-02 |
| MOB-03 | DEFERRED | Mobile grades, GPA, balance, holds, degree progress | Backend-derived, scoped, stale-cache rules; T-MOB-03 |
| MOB-04 | DEFERRED | Mobile course search and eligibility/registration | Same business reasons and atomic API transaction as web; T-MOB-04 |
| MOB-05 | DEFERRED | Mobile student requests and notifications | Submission/read state reconciles with web; T-MOB-05 |
| MOB-06 | DEFERRED | Mobile research milestones if candidate eligible | Non-research students never see research module; T-MOB-06 |
| MOB-07 | DEFERRED | Student may switch concurrently active careers/programs | No mixing of totals, term selection or cache under switch; T-MOB-07 |
| MOB-08 | DEFERRED | Full offline operations for supported Flutter student workflows | Read cached data and queue supported registration/request/research drafts; reconnect reauthorizes/revalidates, rejects stale actions safely and commits idempotently with explicit status; T-MOB-08 |
| MOB-09 | DEFERRED | Encrypted offline storage, expiry and conflict resolution | Tenant/person/program cache separation; local logout/expiry purge and purge on observed server revocation; conflict recovery preserves work and never silently overwrites official state; T-MOB-09 |
| MOB-10 | DEFERRED | Student widgets for schedule, next tasks and permitted alerts | Permission, freshness and privacy controls tested; logout/expiry or observed revocation removes protected content; T-MOB-10 |

## Previously planned journeys — deferred

- **J-OFF:** enrolled Flutter student downloads permitted context → views freshness → drafts supported registration/request/research actions offline → sees queued status → reconnects, reauthenticates and synchronizes → resolves conflicts/rejections or receives confirmed commit. Payments, seat allocation, official grades and approvals are never final offline. Student widgets show permitted cached summaries and expiry. A disconnected device cannot receive immediate remote revocation; short institution-approved cache/widget expiry bounds exposure, and observed revocation purges protected local data.

## Acceptance scenarios

**AC-022 | Student mobile and web parity**  
**Given** same student, term and program context  
**When** student registers or drops from either device  
**Then** official registration truth and holds/schedule are consistent after commit; cached views refresh and unauthorized offline actions never commit.  
**Covers:** MOB-03–05/08/09, REG-04, REG-06; **Proof:** cross-device E2E and API truth assertion.

**AC-028 | Offline replay, conflicts and lost eligibility**  
**Given** a Flutter student with encrypted cached context and queued registration/request/research drafts  
**When** another device changes state, a seat fills or the session is revoked before reconnect  
**Then** offline UI stays pending, reconnect reauthorizes and revalidates, rejected/conflicting work is recoverable, retries commit once and no stale operation overwrites official state.  
**Covers:** MOB-08/09, REG-04/05, IAM-04; **Proof:** offline device runs, revocation and replay/API assertions.

**AC-029 | Widget privacy and freshness**  
**Given** a student widget with configured summary permissions  
**When** data expires, program changes or the user logs out/is revoked  
**Then** widget shows freshness and permitted context only, protected data is purged or hidden on local logout, expiry or observed revocation and no stale summary claims current official state.  
**Covers:** MOB-09/10; **Proof:** device/widget lifecycle captures and cache inspection.

## Related acceptance coverage

[AC-005](../modules/07-student-lifecycle.md), [AC-013](../modules/12-postgraduate-research.md). Scenario ownership and complete coverage are listed in the [acceptance index](../shared/acceptance-index.md).

## Shared contracts and release gates

Apply the shared [domain invariants](../shared/architecture.md), [API contracts](../shared/api-contracts.md), [screen inventory](../shared/screens.md), [quality requirements](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). Requirement IDs, release assignments and T-* families remain unchanged by this organization.
