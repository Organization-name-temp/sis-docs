# 14. Scholarships and financial aid — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Operate aid programs, applications/evidence, eligibility/review, awards/budgets, disbursement instructions, renewal/suspension and reporting.

**Actors:** Students, aid reviewers/approvers and finance officers.

**Business flow:** Publish program → student application → scoped evidence review → human award/budget reservation → finance acknowledgment → renewal/suspension/termination → reconciled reports.

**Core invariant:** Concurrent awards respect budget; no self-approval/AI award; instruction is not ledger credit; renewal/recovery never erases historic credits.

**Policy questions / fixtures to resolve:** Funding allocations/currency units, eligibility/caps/stacking, renewal/recovery rules, reviewer conflicts and finance acknowledgment.

**Screen references:** SC-O14, SC-S07, SC-O15. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns scholarship programs, aid applications, eligibility/review, human awards, funding allocations, disbursement instructions, renewal/suspension/termination and aid reporting. Finance owns credits, refunds and reconciliation; approval/instruction alone is not proof of disbursement. This does not introduce a full government funding regime.

FIN-06 spans scholarships, installment schedules, waivers and adjustments. Award administration is here; [Student Finance](13-student-finance.md) executes all ledger/installment/refund effects. SCH-01–06 expand the scholarship lifecycle without duplicating finance authority.

## Detailed business scope

- Scholarship programs
- Eligibility policies
- Aid applications
- Evaluation and review
- Award decisions
- Budget allocation
- Disbursement instructions
- Renewals
- Suspension and termination
- Aid reporting

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| FIN-06 | R1 | Scholarships, installments, waivers and refundable adjustments | Versioned eligibility, award limits/stacking, installment schedules, approvals and reversals match finance fixtures; no autonomous scholarship award; T-FIN-06 |
| SCH-01 | R1 | Configure scholarship programs and versioned eligibility policies | Program, period, funding source, caps, stacking, required evidence and renewal conditions versioned; approved rule fixtures explain eligibility; T-SCH-01 |
| SCH-02 | R1 | Aid applications, evidence, scoped review and human award decisions | Student submits own application; authorized reviewers record decision/reason; no self-approval or autonomous AI award; retries cannot create duplicate awards; T-SCH-02 |
| SCH-03 | R1 | Scholarship budget allocation and reservation | Concurrent awards cannot exceed approved budget unless explicit finance-authorized exception; cancellations release reserved amounts with audit; T-SCH-03 |
| SCH-04 | R1 | Disbursement instructions and finance acknowledgment | Approved award creates idempotent scoped instruction; finance posts scholarship credit once and returns reconciled status; failed delivery remains retryable without duplicate credit; T-SCH-04 |
| SCH-05 | R1 | Renew, suspend and terminate awards under published policy | Effective-dated review checks continued eligibility; authorized decisions update future instructions; historic credits preserved and any recovery uses approved finance adjustment; T-SCH-05 |
| SCH-06 | R1 | Scholarship/aid operational and budget reporting | Applications, decisions, commitments, disbursements and remaining budget reconcile with finance; row/column scope enforced; no unvalidated government aid compliance claim; T-SCH-06 |

New map-derived requirements are draft extensions; their release assignments are planning proposals pending institutional policy/acceptance review. Existing requirement release commitments are unchanged.

## Map-derived acceptance scenario

**AC-037 | Aid award budget and finance handoff**  
**Given** an approved scholarship program, applications, a limited budget and authorized reviewers  
**When** two awards compete for remaining budget and an approved disbursement instruction is retried  
**Then** only permitted budget commitments succeed, decisions are human/audited, finance posts the credit once and reports acknowledgment; the instruction alone is never shown as reconciled payment.  
**Covers:** SCH-01–04, FIN-01/06; **Release:** R1; **Proof:** eligibility fixtures, concurrent budget races and instruction/ledger replay reconciliation.

## Map-derived acceptance scenario

**AC-038 | Aid renewal, suspension and reporting**  
**Given** an existing award with renewal conditions, historic credited amounts and future instructions  
**When** a reviewer renews, suspends or terminates the award under effective-dated policy  
**Then** future commitments/instructions change according to approval, historic credits remain intact, recovery uses finance adjustments and aid/budget reports reconcile with ledger acknowledgments.  
**Covers:** SCH-05/06, FIN-06, RPT-05; **Release:** R1; **Proof:** award lifecycle fixtures, reconciliation and row/column export negatives.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
