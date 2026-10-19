# 16. Reporting and analytics — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Define/version approved metrics and reports; render dashboards/reconciliation/aid reports and permission-safe exports.

**Actors:** Authorized operational/finance/aid staff, analysts and tenant administrators.

**Business flow:** Choose approved definition/context/parameters → apply row/column scope → compute from authoritative source → show freshness/definition → authorized export.

**Core invariant:** UI/export/AI aggregation share authorization; reports never replace source records; financial/aid totals reconcile.

**Policy questions / fixtures to resolve:** Metric/source/rounding definitions, row/column access, aggregate disclosure rules, refresh expectations and R2 cohort/warehouse depth.

**Screen references:** SC-O15, SC-X02. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns permission-safe report definitions, dashboards, operational/reconciliation/aid analytics and exports. Domain modules retain record authority; cohort/warehouse depth remains R2.

## Detailed business scope

- Operational dashboards
- Report definitions
- Safe exports
- Reconciliation reports
- Cohort analytics
- Data feeds
- Scholarship analytics

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| RPT-01 | R1 | Tenant-scoped operational dashboards | Attendance of registrations/admissions by program matches source query; T-RPT-01 |
| RPT-02 | R1 | Permission-safe export of operational records | Export columns and rows filtered identically to UI; T-RPT-02 |
| RPT-03 | R2 | Cohort/retention analytics, warehouse feed | Derived metrics tie back to definition/version; T-RPT-03 |
| RPT-04 | R1 | Version approved report definitions and reconciliation reports | Definitions identify owner, metric formula, source and parameters; academic/finance/aid totals reconcile and UI/export use identical authorization; T-RPT-04 |
| RPT-05 | R1 | Scholarship operational analytics | Award, budget and disbursement indicators match SCH-06 definitions and ledger acknowledgment; no hidden student details exposed by filters or export; T-RPT-05 |

New map-derived requirements are draft extensions; their release assignments are planning proposals pending institutional policy/acceptance review. Existing requirement release commitments are unchanged.

## Map-derived acceptance scenario

**AC-040 | Approved report definitions reconcile**  
**Given** versioned metric definitions and academic/finance/aid fixtures with restricted rows and columns  
**When** authorized staff run dashboards, reconciliation reports or exports  
**Then** values match named sources and formulas, scholarship totals match funding/ledger acknowledgments and forbidden detail cannot be recovered through filters, aggregation or export.  
**Covers:** RPT-04/05, SCH-06, RPT-02; **Release:** R1; **Proof:** metric golden cases, reconciliation and authorization suite.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
