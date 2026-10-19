# 08. Course offerings and sections — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Publish offerings, section meetings, instructors/TAs, room/time configuration, capacity and scoped rosters.

**Actors:** Academic schedulers, academic administrators, instructors, TAs and students.

**Business flow:** Choose published course/version → draft section and assignments → validate room/time/dates → publish → project committed roster → handle changes/cancellation by policy.

**Core invariant:** Configured capacity is not a booked seat; teaching assignment is explicit/effective-dated; roster authority comes from registrations.

**Policy questions / fixtures to resolve:** Room/time clashes, instructor/TA responsibilities, capacity changes, cancellation effect and roster access/export rules.

**Screen references:** SC-O04, SC-O09, SC-S02. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns offerings, sections, meetings, teaching assignments, capacities and authorized roster views. Registration owns seat allocations. External roster projection is delivered through integration infrastructure.

## Detailed business scope

- Course offerings
- Sections
- Instructor assignments
- Meeting schedules
- Capacity configuration
- Rosters
- Scheduling checks

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| REG-01 | R1 | Create section offerings with campus, time, room, capacity, instructor | Search returns approved published sections; T-REG-01 |
| REG-10 | R2 | Class scheduling optimization / room availability | Solver satisfies hard constraints; T-REG-10 |
| SEC-01 | R1 | Section meeting/room checks, instructor/TA assignments and scoped rosters | Published offering references catalog version; clashes and invalid assignment dates are reported; roster reads/exports follow registration truth and permissions; T-SEC-01 |

New map-derived requirements are draft extensions; their release assignments are planning proposals pending institutional policy/acceptance review. Existing requirement release commitments are unchanged.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
