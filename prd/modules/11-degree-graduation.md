# 11. Degree audit and graduation — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Evaluate versioned requirements, degree progress/snapshots and completion eligibility; authorize final awards.

**Actors:** Registrars, graduation reviewers, advisors and students.

**Business flow:** Run audit against applied curriculum and official attempts → inspect satisfied/remaining/blocked evidence → resolve approved issues/holds → authorized award review → explicit approval/denial.

**Core invariant:** Eligibility is not an awarded degree; golden-case agreement and applied versions are preserved; only authorized approval issues award.

**Policy questions / fixtures to resolve:** Completion/standing/hold policy, recognized credits, audit evidence and graduation/award approval authority.

**Screen references:** SC-O02, SC-S03. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns degree progress/evaluations, reproducible audit snapshots, graduation review and authorized awards; consumes curriculum requirements, official attempts and standing/hold context.

Degree audit consumes official grade/attempt/standing data from [Records & Grading](10-academic-records.md) and versioned requirements from [Program & Curriculum](03-program-curriculum.md).

## Detailed business scope

- Requirement evaluation
- Degree progress
- Eligibility review
- Audit snapshots
- Graduation review
- Academic awards

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| REC-05 | R1 | Degree audit computes completed/remaining/blocked requirements | Result references exact curriculum and rule versions; T-REC-05 |
| REC-06 | R1 | Academic standing and graduation eligibility evaluation | Failing/eligible candidates distinguished with reasons; T-REC-06 |
| REC-07 | R1 | Registrar final approval of award/completion | Evaluation alone never silently awards degree; T-REC-07 |

## Acceptance scenarios

**AC-011 | Degree audit fixed golden case**  
**Given** signed-off UG and MSc academic histories with electives, failures/repeats and required credits  
**When** degree audit runs  
**Then** each requirement, earned credits and GPA reconcile with institution-approved manual expectation exactly.  
**Covers:** ACA-06, ACA-09, REC-02, REC-05; **Proof:** signed fixture results.

**AC-012 | Graduation requires human approval**  
**Given** a program enrollment whose degree audit reports eligible  
**When** eligibility evaluation completes  
**Then** award remains pending until authorized registrar approves; denial requires reason.  
**Covers:** REC-06, REC-07; **Proof:** status/event history.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
