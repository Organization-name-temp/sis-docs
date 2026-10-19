# 03. Programs and curricula — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Define/version programs, requirement groups and cohort applicability; compare versions and preserve applied curriculum.

**Actors:** Academic administrators, registrars, advisors and students.

**Business flow:** Draft requirements → simulate student fixtures → compare versions/impacted cohorts → publish → bind cohort/enrollment → approve explicit migration if permitted.

**Core invariant:** An existing student retains the applied version; required/elective credits cannot be silently remapped or double-counted.

**Policy questions / fixtures to resolve:** Award/program variants, credit groups, elective pools, cohort mapping and migration approval.

**Screen references:** SC-O03, SC-O12, SC-S03. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns programs, curriculum versions, requirement groups and cohort applicability. Degree Audit evaluates them; Student Lifecycle binds the applied version to an enrollment.

## Detailed business scope

- Program definitions
- Program versions
- Curriculum versions
- Requirement groups
- Cohort applicability
- Version comparison

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| ACA-05 | R1 | Program/curriculum versions with publish/effective dates | Existing cohort remains on applied version after new release; T-ACA-05 |
| ACA-06 | R1 | Requirement groups (required, elective pool, credit min, course choice) | Deterministic degree audit matches reference fixtures; T-ACA-06 |
| ACA-12 | R2 | Multi-major/minor, combined award, advanced repeat forgiveness | Golden academic fixtures pass; T-ACA-12 |
| CUR-01 | R1 | Compare program/curriculum versions and cohort applicability | Diff identifies requirement/credit changes; impacted cohorts previewed without migrating existing enrollments or changing applied versions; T-CUR-01 |

New map-derived requirements are draft extensions; their release assignments are planning proposals pending institutional policy/acceptance review. Existing requirement release commitments are unchanged.

## Journeys and behavior

### 5.3 Catalog/curriculum lifecycle (J-ACA)

1. Academic administrator drafts academic career/program/catalog/curriculum for an effective period.
2. Configure requirement groups, course choices, credit thresholds, prerequisites, grades, academic standing and elective pools.
3. Run example student fixtures in *preview/simulation mode* and inspect expected vs actual.
4. Authorized publisher approves immutable version with effective dates and reasons.
5. Admissions/matriculation assigns students the applicable version explicitly, optionally following policy-based admission cohort mapping.
6. New curriculum version applies to future eligible cohorts; migrating an existing student requires explicit action, rationale, equivalence assessment and audit.
7. Audit output shows rule-version IDs, satisfied requirement IDs and blocking evidence; never derive historical degree eligibility from only the newest version.

**Rules DSL principle:** Prefer versioned declarative rule types (AND/OR, course set, minimum credits, GPA bound, prerequisite, co-requisite, repeat, eligibility, holds), not arbitrary executable JSON/SQL from tenant admins. Complex unsupported cases are escalated to approved exception or feature request.

**Sophisticated substitutions** permit approved course/credit combinations or prior learning to satisfy a different requirement, with applicability, evidence and no double-counting. **Articulation** maps partner/external courses or qualifications to local credits under effective-dated agreements. **Full policy DSL** is a safe domain-specific rule language for more complex versioned academic policies, with validation, simulation, explanation and approval; R1 uses bounded declarative primitives. These are R2 depth (ACA-11/13), not arbitrary tenant code.

## Acceptance scenarios

**AC-006 | Curriculum effective date**  
**Given** student assigned Curriculum v2025 and new v2027 published  
**When** degree audit is recalculated  
**Then** student's requirements remain v2025 unless explicit authorized migration occurs, with reason and audit.  
**Covers:** ACA-05, ACA-08, REC-05; **Proof:** before/after JSON and stored reference IDs.

## Map-derived acceptance scenario

**AC-035 | Curriculum comparison and catalog publication**  
**Given** two curriculum versions, existing cohort bindings and draft course/prerequisite references  
**When** an academic administrator compares versions and publishes a future catalog  
**Then** the diff identifies credits/requirements and cohort impacts, existing applied versions remain unchanged, drafts remain hidden and equivalence references cannot silently award transferred credit.  
**Covers:** CUR-01, CAT-01, ACA-05; **Release:** R1; **Proof:** comparison/catalog fixtures and curriculum binding assertions.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
