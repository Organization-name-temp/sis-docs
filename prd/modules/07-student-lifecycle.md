# 07. Student lifecycle, advising and requests — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Execute matriculation, maintain profile/career/program states, advisor assignments and requests; manage taught-PG projects without forced research candidature.

**Actors:** Registrars, advisors, students, taught-PG project advisors and approvers.

**Business flow:** Eligible accepted offer → idempotent matriculation → applied program/curriculum → status/advisor history → requests/project milestones → approved completion handoff.

**Core invariant:** One Person/StudentProfile with multiple program contexts; academic status and login/tenant status are independent; no self-approval.

**Policy questions / fixtures to resolve:** Permitted status/leave/transfer transitions, advisor scope, project/proposal/submission/assessment rules, request approvers and lifelong access.

**Screen references:** SC-O02, SC-O13, SC-S01, SC-S06, SC-S04. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns matriculation execution, student profiles, career memberships, program enrollment/status, advisor context and student requests. ADM-11 keeps its stable ID here. Includes taught-PG dissertation/project management under PGT-01; research candidature remains in Postgraduate research.

Matriculation accepts an eligible admission/offer reference and atomically reuses Person/StudentProfile, creates the program enrollment and emits an outbox event. Admissions owns the offer; this module owns the resulting student state.

## Detailed business scope

- Matriculation
- Student profiles
- Career memberships
- Program enrollments
- Status transitions
- Student context
- Student requests
- Advisor context

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| IAM-05 | R1 | Institutional account state and student program state are independent | Alumni retain allowed record access without registration access; T-IAM-05 |
| ACA-02 | R1 | Multiple sequential/concurrent student program enrollments | Credits/grades and statuses remain program/career attributable; T-ACA-02 |
| WFL-01 | R1 | Configurable request definition, fields and approval steps | Transcript/request prototype follows assigned approver chain; T-WFL-01 |
| WFL-02 | R1 | Approver resolver checks institution and record scope | No self-approval where policy forbids; T-WFL-02 |
| WFL-03 | R1 | Request status/history, rejection reason and resubmission | History survives reassignment/cancellation; T-WFL-03 |
| PGT-01 | R1 | Taught-PG dissertation/project lifecycle distinct from research candidature | Eligible program supports advisor allocation, proposal, submission, assessment and completion conditions without requiring PhD milestones; T-PGT-01 |
| ADM-11 | R1 | Matriculation creates appropriate student and program enrollment | Reused Person, unique StudentProfile, correct career and curriculum; T-ADM-11 |
| STU-01 | R1 | Maintain student profile, career memberships and program status transitions | Transition matrix validates actor, evidence and effective dates; duplicates and arbitrary status jumps denied; account and academic status stay independent; T-STU-01 |
| STU-02 | R1 | Student context and effective-dated advisor assignments | Authorized advisor sees assigned program context only; assignment history survives reassignment and concurrent careers remain distinct; T-STU-02 |

New map-derived requirements are draft extensions; their release assignments are planning proposals pending institutional policy/acceptance review. Existing requirement release commitments are unchanged.

## Journeys and behavior

### 5.2 Admissions officer and registrar workflow (J-MAT)

1. Admissions officer sees assigned intake/program reviews and file completeness.
2. Officer requests missing data; applicant responds in controlled correction cycle.
3. Authorized decision is recorded with optional conditions and a timestamp.
4. Applicant accepts a valid offer and satisfies required conditions.
5. Registrar/matriculation service resolves existing `Person` by scoped identity criteria; suspected duplicates enter manual resolution queue.
6. Under a database transaction, service creates or links one `StudentProfile`, adds a new `ProgramEnrollment`, applies career and curriculum version, emits `StudentMatriculated` outbox event.
7. If retries, same `application_id + matriculation_intent_id` returns same result, never a second student or enrollment.

**Important:** A person can be student and applicant simultaneously for another career. No automatic merge based solely on email or name; use verified identifiers and manual review for collisions.

### 5.8 Undergraduate and taught-postgraduate lifecycle (J-UG / J-PGT)

| Stage | Undergraduate (J-UG) | Taught postgraduate (J-PGT) |
|---|---|---|
| Entry | Prior-school evidence, offer conditions, matriculation and orientation | Prior degree/prerequisite review, bridging requirements where configured and matriculation |
| Planning/study | Versioned core/elective plan, advising, registration, assessments and credits | Advanced module/specialization plan, coursework, advisor and dissertation/project route |
| Progress/changes | Standing, repeats, appeals and permitted leave/transfer requests | Coursework/project progress, extensions, appeals and permitted route changes |
| Completion | Degree audit, hold resolution, registrar approval and award | Coursework plus dissertation/project submission/assessment, completion review and award |
| After award | Permitted lifelong record and transcript access | Permitted lifelong record and transcript access |

Taught-PG project records use PGT-01, not a forced research candidature. A taught-PG TA uses the same Person with separate academic and student contexts. Detailed transfer automation remains R2; requests and approved exceptions retain history in R1.

## Acceptance scenarios

**AC-005 | Concurrent academic careers**  
**Given** a student with valid BSc and MSc program enrollments (distinct current terms/curricula)  
**When** switching programs on web  
**Then** course records, GPA/degree audit and current schedule are from selected context; no unintended mixing.  
**Covers:** ACA-02, REG-08, REC-05; **Proof:** fixture comparison across both clients.

**AC-033 | Three academic journeys and taught-PG completion**  
**Given** UG, taught-PG and research enrollments with distinct approved policies  
**When** students progress through configured admission, study and completion tasks  
**Then** UG follows credit/degree rules; taught-PG completes coursework plus approved project/dissertation without forced PhD milestones; research shows R1 candidature/submission scope with explicit R2 examination handoff; each retains allowed lifelong record access.  
**Covers:** ACA-01/02, PGT-01, RES-01–06, REC-05–07, IAM-05; **Proof:** three-journey UAT, project assessment and release-boundary assertions.

**AC-004 | Double matriculation**  
**Given** an accepted valid offer bound to an applicant Person  
**When** acceptance/matriculation endpoint is retried or double-clicked  
**Then** one StudentProfile and one matching ProgramEnrollment exist, with one logical matriculation event.  
**Covers:** ADM-08, ADM-11, IAM-01, INT-02; **Proof:** row counts, event IDs, API responses.

## Map-derived acceptance scenario

**AC-036 | Student status, advisor and section context**  
**Given** concurrent enrollments, effective-dated advisor/teaching assignments and an approved status transition matrix  
**When** staff change program status, reassign an advisor or query a roster  
**Then** invalid transitions and foreign scopes fail, history is retained, career contexts stay separate and the roster reflects committed registrations and valid teaching assignments.  
**Covers:** STU-01/02, SEC-01, ACA-02, IAM-09; **Release:** R1; **Proof:** transition and assignment fixtures, roster/API authorization tests.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
