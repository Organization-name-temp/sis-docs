# 04. Course catalog — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Maintain course definitions/versions, applicability, prerequisites and approved equivalence references.

**Actors:** Academic administrators, advisors, applicants and students.

**Business flow:** Draft/version course → validate references/credits → approve catalog publication → discover approved offering → preserve historical identity.

**Core invariant:** A course definition differs from a section; an equivalence reference alone cannot award transfer credit; drafts must not leak.

**Policy questions / fixtures to resolve:** Course identifiers, credits, career applicability, prerequisite ownership and publication/equivalence approval.

**Screen references:** SC-O03, SC-S02, SC-A01. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns course definitions/versions and catalog publication. Declared prerequisites reference Academic Rules; equivalence references do not themselves approve transferred credit.

## Detailed business scope

- Course definitions
- Course versions
- Career applicability
- Prerequisite declarations
- Catalog publication
- Equivalence references

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| ACA-04 | R1 | Course and course-version catalog with program/career eligibility | Historical course label/credits retained when catalog changes; T-ACA-04 |
| CAT-01 | R1 | Publish versioned catalogs and prerequisite/equivalence references | Draft courses are hidden from public search; publication records actor/effective period; references identify applicable rule/approved mapping without granting unapproved transfer credit; T-CAT-01 |

New map-derived requirements are draft extensions; their release assignments are planning proposals pending institutional policy/acceptance review. Existing requirement release commitments are unchanged.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
