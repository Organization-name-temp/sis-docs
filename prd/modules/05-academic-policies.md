# 05. Academic policies and rule evaluation — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Configure deterministic prerequisite/corequisite, credit/load/repeat, hold, grading and progression rules with traceable exceptions.

**Actors:** Academic administrators, registrars, advisors and students.

**Business flow:** Draft rule primitives → simulate signed fixtures → review explanation/impact → publish version → evaluate → apply authorized scoped exception if permitted.

**Core invariant:** AI/client values cannot override official rules; evaluation must identify policy/version/evidence; exceptions are explicit and scoped.

**Policy questions / fixtures to resolve:** Institutional GPA/repeat/hold conventions, rule precedence, effective version, exception authority and R2 DSL boundary.

**Screen references:** SC-O03, SC-S02, SC-S03. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns deterministic eligibility/policy evaluation, grading/progression rule definitions, exceptions and simulation. Records owns official grade history; Degree Audit owns persisted degree evaluations. Advanced articulation/substitutions and full DSL retain R2 depth.

Grading policies are defined here; [Records & Grading](10-academic-records.md) owns official grades/GPA, and [Degree Audit](11-degree-graduation.md) owns completion evaluations.

## Detailed business scope

- Rule definitions
- Eligibility evaluation
- Grading policies
- Progression rules
- Evaluation traces
- Exception application
- Policy simulation

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| ACA-07 | R1 | Reusable prerequisite/corequisite/credit-load/repeat/hold policies | Engine returns structured eligibility + reason codes; T-ACA-07 |
| ACA-08 | R1 | Rule changes require versioned publish and actor/reason history | Previously evaluated historical context remains reproducible; T-ACA-08 |
| ACA-09 | R1 | Grade schemes, weight/quality points and GPA configured per career | PG and UG policies differ correctly; T-ACA-09 |
| ACA-10 | R1 | Institution-controlled exceptions with explicit policy/expiry | Unauthorized override rejected; approved exception audited; T-ACA-10 |
| ACA-11 | R2 | Credit transfer, sophisticated substitutions, articulation, waivers and equivalence rules | Versioned partner agreements/evidence and approvals determine satisfaction; no double-credit; T-ACA-11 |
| ACA-13 | R2 | Safe full academic policy DSL beyond R1 declarative primitives | Versioned typed rules validate/simulate against signed fixtures; arbitrary executable tenant code denied; traces explain decisions; T-ACA-13 |

## Journeys and behavior

**Sophisticated substitutions** permit approved course/credit combinations or prior learning to satisfy a different requirement, with applicability, evidence and no double-counting. **Articulation** maps partner/external courses or qualifications to local credits under effective-dated agreements. **Full policy DSL** is a safe domain-specific rule language for more complex versioned academic policies, with validation, simulation, explanation and approval; R1 uses bounded declarative primitives. These are R2 depth (ACA-11/13), not arbitrary tenant code.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
