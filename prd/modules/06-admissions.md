# 06. Admissions — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Manage intake/forms, evidence, resumable applications, review, decisions and offers; hand accepted cases to Student Lifecycle.

**Actors:** Applicants, admissions officers and registrars.

**Business flow:** DRAFT → SUBMITTED → UNDER_REVIEW → missing information/waitlist/conditional offer/offer/rejection → offer acceptance/decline → matriculation handoff.

**Core invariant:** Published form versions and submission snapshots are stable; retries never duplicate submissions; rejected/draft applications do not create students.

**Policy questions / fixtures to resolve:** Intake eligibility, program-specific form conditions/evidence, ranked choices, correction windows, offer expiry and fee adapter.

**Screen references:** SC-A01–05, SC-O01. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns intake/application schemas, evidence, submissions, review, decisions and offers. Accepted eligible application is handed to Student Lifecycle; admissions must not create a second independent student record.

## Detailed business scope

- Intakes and requirements
- Application schemas
- Draft applications
- Supporting evidence
- Submission
- Review queues
- Decisions and offers
- Matriculation handoff

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| ADM-01 | R1 | Program/intake application eligibility and open window | Closed intake cannot accept new submissions; T-ADM-01 |
| ADM-02 | R1 | Program/intake/career-specific multi-step schemas | UG vs PhD show different requirements without code edits; T-ADM-02 |
| ADM-03 | R1 | Forms contain groups, repeaters, field types and declarative conditions | Passport conditional and prior-degrees repeater operate safely; T-ADM-03 |
| ADM-04 | R1 | Save/resume draft with server validation and step tracking | Browser refresh or alternate device resumes valid latest draft; T-ADM-04 |
| ADM-05 | R1 | Form drafts tied to a published immutable form version | Editing future template does not change old application; T-ADM-05 |
| ADM-06 | R1 | Upload supporting documents securely | Size/type/access rules and tenant scope enforced; T-ADM-06 |
| ADM-07 | R1 | Review page verifies required answers, attestation and uploads | Cannot submit incomplete required data; T-ADM-07 |
| ADM-08 | R1 | Submission idempotency and immutable snapshot | Double click creates one submission; edits locked except explicit correction; T-ADM-08 |
| ADM-09 | R1 | Admissions review queues and decision states | Only authorized officer advances state; reason/audit captured; T-ADM-09 |
| ADM-10 | R1 | Offers, conditional requirements and acceptance | Only live/eligible offer may be accepted, once; T-ADM-10 |
| ADM-12 | R1 | Applicants view state and missing-items requests | Status/messages visible only to application owner; T-ADM-12 |
| ADM-13 | R2 | WYSIWYG visual form designer and staff review scoring rubric | Publish preview, condition tests, permissions and history; T-ADM-13 |

## Journeys and behavior

### 5.1 Applicant workflow — multi-step, conditional, versioned (J-ADM)

**Entry:** Public university/program discovery → `Apply` for chosen intake. Application steps are generated from **published form version bound to `tenant + institution + intake + program/career`**.

**R1 template examples (illustrative; actual policy configured):**

| Step | Undergraduate applicant | Taught MSc applicant | Research PhD applicant |
|---|---|---|---|
| 1 | Account / identity | Account / identity | Account / identity |
| 2 | Personal and contacts | Personal and contacts | Personal and contacts |
| 3 | Secondary school and prior results | Degrees and academic history | Degrees and research history |
| 4 | Program/intake preferences | Program/intake | Program/research field |
| 5 | Supporting documents | Transcript/degree documents | Transcript/CV/proposal/references |
| 6 | Additional requirements | Funding/experience (if needed) | Supervisor preference/funding (if configured) |
| 7 | Review, declaration, submit | Review, declaration, submit | Review, declaration, submit |

The sequence, labels, steps and requirements are **configuration**, not universal requirements. When application fees are configured, use an agreed R1 payment adapter with verified callback/reconciliation, idempotent posting and an explicit pending/paid/failed state; a browser redirect alone cannot confirm payment.

**Behavior and rules:**
- On create, bind the application to immutable `form_version_id` and chosen intake; draft fields retain their data type and validation metadata.
- Step transitions check **server-side** validation for that step; final submit checks the entire published schema and business rules.
- Autosave is resilient to brief disconnections and reports last saved time; server revision/ETag prevents lost updates between two tabs/devices. On conflict, preserve versions and request resolution rather than silently overwrite.
- Conditional fields only become mandatory if the condition is satisfied. Hidden fields cannot be used to bypass security or to inject extra attributes.
- Repeatable degrees/recommendations have bounds; upload limits, accepted formats and scan/storage policy configured and communicated.
- Applicants can review, correct and submit once. Post-submission edits require a controlled `MissingInformationRequested` / correction window and preserve an old submission snapshot.
- Decision engine supports `DRAFT → SUBMITTED → UNDER_REVIEW → {MISSING_INFORMATION, WAITLISTED, CONDITIONAL_OFFER, OFFERED, REJECTED}`, and from offered states `OFFER_ACCEPTED/OFFER_DECLINED → MATRICULATED` where policy permits. Valid transitions defined explicitly; deny arbitrary status jumps.
- Not every program must support interview, waitlist or fee. These transitions are optional configurable actions.
- Applicants receive localized communication; message failure cannot roll back the accepted application and can be retried safely.

**Failure/edge cases:** intake closes while a draft exists; program withdraws availability; document deleted; 2 tabs edit different data; double submit; program changes requirements while draft exists; applicant becomes a student in another institution; invitation expires; applicant requests data correction after decision.

**Expected outcome:** A complete, reviewable snapshot with actor, form version, documents, history, review decision and candidate matriculation. No ghost students created for rejected/draft applicants.

## Acceptance scenarios

**AC-001 | Application wizard version stability**  
**Given** a published MSc intake form v1 and applicant draft attached to v1  
**When** admin publishes v2 with another required question  
**Then** v1 draft remains governed by v1, v2 new applicants see v2, and both snapshots/audit are reproducible.  
**Covers:** ADM-02, ADM-04, ADM-05; **Proof:** form snapshots, database assertions, screenshots in all locales.

**AC-002 | Conditional research fields**  
**Given** a UG application and a PhD application  
**When** applicant reaches documents and program-specific steps  
**Then** configured research proposal/supervisor fields only appear and become required on the relevant PhD program; hidden-field injections are rejected.  
**Covers:** ADM-02, ADM-03, IAM-02; **Proof:** validation/e2e API log.

**AC-003 | Conflicting autosave**  
**Given** the same draft open on two devices  
**When** each saves conflicting updates  
**Then** server identifies revision conflict and retains recoverable data rather than silently losing values.  
**Covers:** ADM-04; **Proof:** revision data + E2E replay.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
