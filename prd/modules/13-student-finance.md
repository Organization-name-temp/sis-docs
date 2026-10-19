# 13. Student finance and holds — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Calculate rich tuition, post immutable charges/credits, manage balances/holds, installments/waivers/refunds and provider/accounting reconciliation.

**Actors:** Finance officers, authorized registrars and students.

**Business flow:** Versioned quote → authorized charge → verified payment/aid instruction → idempotent posting → reconcile → apply/release policy-scoped holds → approved adjustment/refund.

**Core invariant:** Balance is ledger-derived; callbacks/redirects/instructions are not duplicate or unverified payment authority; historic postings are not rewritten.

**Policy questions / fixtures to resolve:** Tuition precedence/credits/proration/rounding, holds, provider matrix, installments/refunds and scholarship instruction acknowledgment.

**Screen references:** SC-O05, SC-S04. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns student accounts, rich tuition calculation, ledger, installments, holds, payments/refunds and reconciliation. FIN-06 remains a scholarship-policy anchor in Scholarships and financial aid, while finance executes installment/waiver/adjustment effects. Only finance posts monetary ledger changes.

## Detailed business scope

- Student accounts
- Charge policies
- Ledger entries
- Balance views
- Holds
- Scholarship credits
- Reconciliation
- Payment adapters

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| FIN-01 | R1 | Maintain student charge/credit ledger for tuition-related items | Balance is ledger-derived, not untracked mutable field; T-FIN-01 |
| FIN-02 | R1 | Rich versioned tuition engine by program, career, credits, cohort, term and effective date | Signed fixtures cover fee precedence, rounding, add/drop proration and approved exceptions; recalculation preserves historical postings; T-FIN-02 |
| FIN-03 | R1 | Academic/financial holds as explicit objects | A configured blocking hold prevents registration, with explanation; T-FIN-03 |
| FIN-04 | R1 | Finance view of balances and authorization-limited student view | Student cannot view another person's invoices; T-FIN-04 |
| FIN-05 | R1 | Payment gateway integrations, verified callbacks and reconciliation | Signed callbacks, provider status checks, duplicate/out-of-order events, failed payments, refunds and reconciliation discrepancies handled without duplicate postings; T-FIN-05 |
| FIN-07 | R1 | Agreed payment/accounting integration breadth and source-of-truth mapping | Provider/operation matrix approved before build completion; each agreed adapter proves mappings, failure/retry, reconciliation and balanced export with no dual posting; T-FIN-07 |

## Journeys and behavior

### 5.7 Finance + hold integration (J-FIN)

- A student may have one or more ledger accounts / account assignments linked to institution/program according to defined finance policy.
- Charges, credits and adjustments are append-only financial events with source, effective date, actor and approval reason.
- Balance is derived; manual adjustments require finance permission, not direct mutable balance changes.
- Financial holds are standalone objects with scope, effective dates, reason and release rule; registration engine consults only applicable blocking holds.
- R1 tuition quotes apply versioned program/career/cohort/credit/term rules, rounding, proration and published effective dates; posting preserves the exact policy and calculation trace.
- Finance configures scholarship eligibility, caps/stacking, approval, installment schedules, waivers and refund policies. A student sees award/application/payment status; only authorized humans approve consequential awards or exceptions.
- R1 payment providers verify callbacks and authoritative provider status before posting; duplicates, delayed/out-of-order callbacks, partial payments, refunds and provider outages cannot corrupt balances. Reconcile discrepancies through an owned review queue.
- R1 accounting/payment adapters implement an approved provider/operation matrix, mappings, source-of-truth ownership, retry and reconciliation. Full general ledger/payroll remains external.

## Acceptance scenarios

**AC-018 | Finance hold scoped by policy**  
**Given** student with account balance and a configured non-blocking notification hold  
**When** they register  
**Then** registration is not blocked solely by that notification; a blocking effective hold prevents booking.  
**Covers:** FIN-03, REG-05; **Proof:** two-policy fixture run.

**AC-026 | Rich tuition, scholarship and provider reconciliation**  
**Given** approved tuition/proration fixtures, scholarship caps/stacking, installment schedule and agreed provider adapters  
**When** charges are calculated and duplicate/out-of-order payments, partial payments and refunds arrive  
**Then** exact ledger/hold outcomes match fixtures, no duplicate posting occurs, failed signatures are denied and discrepancies enter reconciliation review with balanced accounting exports.  
**Covers:** FIN-01–07; **Proof:** finance golden cases, adapter contract tests and reconciliation report.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
