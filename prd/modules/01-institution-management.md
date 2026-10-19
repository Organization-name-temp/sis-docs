# 01. Institution management and academic staffing — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Maintain institution/campus/unit structure and effective-dated responsibilities, qualifications, appointments and TA/teaching links.

**Actors:** Tenant/academic administrators, academic members, TAs and faculty/department leaders.

**Business flow:** Draft a structure/appointment change → review affected dates and units → authorize → publish/apply → retain prior structure and appointment history.

**Core invariant:** Titles and PhD qualifications never grant permissions or teaching duties; a taught-PG TA keeps the same Person and cannot grade self or approve own work.

**Policy questions / fixtures to resolve:** Who owns a unit and its responsibilities; appointment/qualification terminology; concurrent appointments and effective dates.

**Screen references:** SC-O11, SC-O09. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns institution/campus/unit hierarchy, responsibilities and effective-dated academic membership, qualifications, deans/heads and TA appointments. A title or appointment never grants permissions or implies teaching. Course & Section Management links appointments to teaching duties.

## Detailed business scope

- Institution profiles
- Campus structure
- Academic units
- Unit responsibilities
- Structure history

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| TEN-02 | R1 | Tenant has institution(s), campuses and academic units | Institution and department hierarchy permits distinct campuses; T-TEN-02 |
| IAM-08 | R1 | Separate academic membership, qualifications, rank, leadership appointments and teaching assignments | Non-teaching Dr./PhD holder and faculty/department leader can operate authorized duties without section assignment or automatic grade rights; T-IAM-08 |
| IAM-09 | R1 | Effective-dated TA assignments and separation of duties | Taught-PG TA retains own student access, sees only assigned academic work and cannot grade self or approve own request; T-IAM-09 |
| INS-01 | R1 | Version institution profiles and campus/unit responsibilities | Authorized edits retain effective dates, responsible unit/office and structure history; future changes do not rewrite historical affiliations; T-INS-01 |

New map-derived requirements are draft extensions; their release assignments are planning proposals pending institutional policy/acceptance review. Existing requirement release commitments are unchanged.

## Acceptance scenarios

**AC-024 | Overlapping TA identity and non-teaching leadership**  
**Given** one taught-PG student who is also TA/academic member, and a non-teaching Dr./PhD holder appointed dean/head  
**When** each opens authorized web tasks and attempts grade changes  
**Then** the TA sees assigned academic work and own student record without self-grading/self-approval; the leader needs explicit permissions and gains no teaching rights from title or appointment.  
**Covers:** IAM-03/08/09; **Proof:** identity/assignment fixtures and positive/negative role UAT.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
