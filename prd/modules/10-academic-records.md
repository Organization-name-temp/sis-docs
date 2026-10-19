# 10. Academic records and grading — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Manage official grades/attempts, publication/correction, GPA, transcript views and controlled credential requests.

**Actors:** Instructors/examiners, registrars and students.

**Business flow:** Assigned grade entry → validate/submit → authorized publication/lock → append-only approved correction → recalculate record/GPA → student transcript.

**Core invariant:** No own-grade edits, silent overwrites or cross-career GPA mixing; unofficial view is distinct from R2 official credential exchange.

**Policy questions / fixtures to resolve:** Grade schemes/precision, repeat/rounding conventions, deadlines/locks, correction authority and transcript disclosure.

**Screen references:** SC-O04, SC-O02, SC-S03. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns official grade entry/publication/corrections, academic attempts, GPA and transcript views. Academic standing uses versioned rules; degree eligibility and award approval belong to Degree audit and graduation. Official credential exchange remains R2.

## Detailed business scope

- Grade entry
- Grade publication
- Grade corrections
- Academic attempts
- GPA calculation
- Academic standing
- Transcript views
- Record requests

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| REC-01 | R1 | Faculty submit scoped course grades with cutoff/lock | Wrong instructor and locked term edits rejected; T-REC-01 |
| REC-02 | R1 | GPA/cumulative GPA based on correct grade/attempt rules | Fixture case agrees exactly using explicit rounding conventions; T-REC-02 |
| REC-03 | R1 | Append-only official grade change history with approval | After publication change creates new event/version; T-REC-03 |
| REC-04 | R1 | Unofficial academic transcript with term/career identity | Historical courses, grades, credit and GPA remain correct; T-REC-04 |
| REC-08 | R2 | Official transcript orders and secure credential exchange | Authorized issuance/receipt verifies issuer, integrity, recipient, delivery and supported status/revocation; consent/authority and partner/legal validation recorded; T-REC-08 |

## Journeys and behavior

### 5.5 Grade → academic record → degree audit → graduation (J-REC)

1. Instructor with assignment to a section enters final grades; range/type policy validated.
2. Registrar publishes official grades according to term/grade lock rules (R1 may use instructor submit→registrar publish workflow).
3. Grade publication generates append-only change history. Corrections after lock are formal workflow, not silent overwrite.
4. GPA engine applies versioned career/program grading/repeat rules with **specified rounding/precision** from policy fixture (never guess).
5. Student sees grade/term GPA/cumulative GPA, career-specific transcript and related requirement progress.
6. Degree audit evaluates exact applied curriculum version and recognized credits, detects unfulfilled requirements and rules/holds.
7. Graduation eligibility is **evaluation**, not final award. Registrar reviews and approves award; completion event/audit generated.

**Important data semantics:** `registered`, `withdrawn`, `completed`, `failed`, `transferred`, `audit-only` and other enrollment/attempt states must not be confused; audit rules determine which attempts count. Transcript and GPA must attribute records to the proper career and student program.

**Official credential exchange** is authorized secure delivery/receipt of institution-issued transcripts or awards with issuer/integrity/recipient/status verification, consent or documented authority, delivery evidence and supported correction/revocation. It is beyond R1 under REC-08; an unofficial transcript download does not prove exchange or standards certification.

**Official credential exchange** is authorized secure delivery/receipt of institution-issued transcripts or awards with issuer/integrity/recipient/status verification, consent or documented authority, delivery evidence and supported correction/revocation. It is beyond R1 under REC-08; an unofficial transcript download does not prove exchange or standards certification.

## Acceptance scenarios

**AC-010 | Illegal grade amendment**  
**Given** officially published grades and no approved correction  
**When** faculty requests an update  
**Then** direct edit is denied; approved correction creates recorded history and appropriate recalculations.  
**Covers:** REC-01, REC-03, REC-05; **Proof:** history, permissions, recalculated values.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
