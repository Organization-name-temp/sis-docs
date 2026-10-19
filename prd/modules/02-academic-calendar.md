# 02. Academic structure and calendar — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Configure careers, academic years, terms/sessions, registration windows and deadlines by institution/program context.

**Actors:** Academic administrators, registrars, applicants and students.

**Business flow:** Draft calendar → validate dates/timezone/career applicability → publish effective version → present relevant deadlines → retain history.

**Core invariant:** Locale/browser timezone must not alter authoritative deadline instants; published changes cannot silently rewrite prior evaluations.

**Policy questions / fixtures to resolve:** Career-specific calendars, timezone, overlapping sessions, deadline changes and publication authority.

**Screen references:** SC-O12, SC-S01. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns academic careers, years, terms/sessions and published deadline calendars. Curriculum and offering modules reference these records rather than maintaining independent calendars.

Calendar presentation also follows the [student web capabilities](../shared/system-capabilities.md); calendar publication is CAL-01.

## Detailed business scope

- Academic careers
- Academic years
- Terms and sessions
- Academic deadlines
- Calendar publication
- Calendar presentation

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| ACA-01 | R1 | Configurable AcademicCareer with program linkage | UG, coursework MSc and research PhD configured in one tenant; T-ACA-01 |
| ACA-03 | R1 | Terms/sessions, deadlines and registration windows per career | PG and UG can use distinct term policies; T-ACA-03 |
| CAL-01 | R1 | Publish academic-year, term/session and deadline calendars | Conflicting dates fail validation; published versions retain timezone and career applicability; student/staff views agree on effective deadlines; T-CAL-01 |

New map-derived requirements are draft extensions; their release assignments are planning proposals pending institutional policy/acceptance review. Existing requirement release commitments are unchanged.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
