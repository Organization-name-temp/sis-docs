# 12. Postgraduate research — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Manage candidature, topic/proposal, supervisors, milestones/progress and thesis metadata with scheduled examination depth.

**Actors:** Research candidates, supervisors, graduate coordinators and registrars.

**Business flow:** Eligible enrollment → candidature/supervision → proposal/plan → milestones/progress → versioned thesis submission → approved completion/examination handoff.

**Core invariant:** Taught-PG project is distinct; only authorized candidates/supervisors see evidence; R1 submission tracking is not full R2 examination automation.

**Policy questions / fixtures to resolve:** Principal/secondary supervision, milestone/ethics templates, leave/extensions, thesis evidence and R1/R2 examination boundaries.

**Screen references:** SC-S05, SC-O06. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns research candidature, topics, supervisors, milestones, progress, submissions and research requests. Full examination/defense/corrections and advanced research changes remain R2. Taught-PG projects are retained in Student Lifecycle.

## Detailed business scope

- Research candidature
- Research topics
- Supervisor assignments
- Milestone templates
- Progress reviews
- Thesis submissions
- Research requests
- Examination processes

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| RES-01 | R1 | Create research candidature only for eligible research program | Taught PG does not accidentally require PhD milestones; T-RES-01 |
| RES-02 | R1 | Assign principal/secondary supervisors | Approval scope and assignment history retained; T-RES-02 |
| RES-03 | R1 | Maintain research topic/proposal and statuses | Versions/approval milestones traceable; T-RES-03 |
| RES-04 | R1 | Define and evaluate program-specific milestones/deadlines | Overdue/upcoming/completed states consistent across web; T-RES-04 |
| RES-05 | R1 | Research candidate/student self-service progress view | Student only sees own candidature and feedback; T-RES-05 |
| RES-06 | R1 | Store thesis submission metadata and document version | Wrong candidate/supervisor cannot submit or view; T-RES-06 |
| RES-07 | R2 | Committees, external examiners, defense, correction cycles | Approval chain, conflicts and retention validated; T-RES-07 |
| RES-08 | R2 | Research leave/extension/change supervisor requests | Effective dates and approvals impact milestones correctly; T-RES-08 |

## Journeys and behavior

### 5.6 Research postgraduate journey (J-RES)

- A program flagged as *research-eligible* permits one or more scoped `ResearchCandidature` records attached to the student `ProgramEnrollment`.
- Supervisor roles and assignments have effective dates; one designated principal supervisor; assignment changes recorded.
- Coordinator publishes milestone template: proposal acceptance, annual progress, ethics, thesis draft, final submission as configured.
- Student sees own milestones, status, deadlines, supervisor and scoped feedback on web.
- Candidate uploads versioned proposal/thesis artifacts; ownership and role authorizations validated; submission date recorded.
- R1 explicitly **does not claim** full external examiner appointment, viva/defense, appeals, intellectual-property review or national-research regulations. These are R2+ with university-specific policy discovery.
- Award still uses registrar-approved completion evaluation; coursework components (if any) use common academic record/degree-audit primitives.

### 5.9 Expanded research lifecycle and release boundary (J-RES)

Discovery/application → proposal and prospective-supervisor review → conditional admission/funding review → matriculation/candidature → research plan and any coursework → applicable ethics/progress milestones → submission → examination/viva/corrections → final approval/deposit → award and lifelong record. R1 covers candidature, supervision, topic, milestones, submission tracking and registrar-controlled completion; external examiner panels, full defense/correction processes and advanced ethics/publication management remain R2, with explicit manual handoff rather than implied automation. Leave/extensions and supervisor changes preserve effective-dated history when enabled at their scheduled release depth.

## Acceptance scenarios

**AC-013 | Research-only screen and supervision**  
**Given** a non-research MSc student and a research candidate with assigned supervisor  
**When** each opens student web Research  
**Then** only candidate sees research data; unrelated supervisor cannot view candidate notes or documents.  
**Covers:** RES-01, RES-02, RES-05, IAM-02; **Proof:** role matrix and mobile screenshots.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
