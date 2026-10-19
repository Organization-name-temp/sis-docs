# Modern SIS — Business-module PRD

**Version:** 0.7  
**Date:** 2026-10-19  
**Status:** Working draft; not yet approved by institutional stakeholders  
**Product vision:** [Modern SIS product vision](../modern_sis_product_vision_v0.1.md)  
**Product owner:** Project team (named owner TBD)  
**Tech decisions:** React responsive web + Laravel backend; mobile deferred; PostgreSQL suggested  
**Languages:** More than 99 supported languages; `ar`, `en`, `fr` are reference validation languages, with LTR/RTL support  
**Release baseline:** R0.5 = required pre-R1 AI delivery; R1 = expanded graduation release with waitlists, rich finance and visual workflows; R2 = pilot expansion; R3 = enterprise breadth

> **What “closed loop” means here:** Every objective links to measurable outcomes; each outcome to a user journey and numbered requirement; every requirement to a designed flow, domain/API contract, test, observed evidence and release decision; and production feedback or defects reopen and update the same record. Writing a PRD alone does not close a loop.

## Organization

Each business module has **one PRD file** containing its purpose, users, product journey, scope/boundaries, business rules, functional requirements, dependencies, open policy questions and acceptance criteria. The module PRD describes the whole module, not one implementation role.

Analyze and refine each module PRD first. Backend, AI, UI/UX, Frontend, QA and Security work will be derived from it later. Role-specific Markdown files are not part of this PRD folder.

## Business-module PRDs

| Business module | Product requirements document |
|---|---|
| 01. Institution management and academic staffing | [Module PRD](modules/01-institution-management.md) |
| 02. Academic structure and calendar | [Module PRD](modules/02-academic-calendar.md) |
| 03. Programs and curricula | [Module PRD](modules/03-program-curriculum.md) |
| 04. Course catalog | [Module PRD](modules/04-course-catalog.md) |
| 05. Academic policies and rule evaluation | [Module PRD](modules/05-academic-policies.md) |
| 06. Admissions | [Module PRD](modules/06-admissions.md) |
| 07. Student lifecycle, advising and requests | [Module PRD](modules/07-student-lifecycle.md) |
| 08. Course offerings and sections | [Module PRD](modules/08-course-sections.md) |
| 09. Course registration and waitlists | [Module PRD](modules/09-course-registration.md) |
| 10. Academic records and grading | [Module PRD](modules/10-academic-records.md) |
| 11. Degree audit and graduation | [Module PRD](modules/11-degree-graduation.md) |
| 12. Postgraduate research | [Module PRD](modules/12-postgraduate-research.md) |
| 13. Student finance and holds | [Module PRD](modules/13-student-finance.md) |
| 14. Scholarships and financial aid | [Module PRD](modules/14-scholarships-aid.md) |
| 15. Notifications and communications | [Module PRD](modules/15-communications.md) |
| 16. Reporting and analytics | [Module PRD](modules/16-reporting-analytics.md) |

## Shared product requirements

- [Scope and personas](shared/overview.md)
- [Shared system capabilities](shared/system-requirements.md) — responsive web and AI requirements
- [Localization/accessibility](shared/localization.md)
- [Shared foundations](shared/foundations/README.md) — identity, configuration/workflow, tenancy, integrations and operations
- [Screen inventory](shared/screens.md), [architecture/invariants](shared/architecture.md) and [API contracts](shared/api-contracts.md)
- [Quality requirements](shared/quality.md), [acceptance scenarios](shared/acceptance-index.md), [traceability](shared/traceability.md) and [release gates](shared/release-plan.md)
- [Requirement ownership](shared/module-map-alignment.md), [current decisions](shared/governance.md) and [backend-map references](shared/backend-map.md)
- [Later delivery workflow](workflow.md) — used when deriving role work from a reviewed module PRD

## Current scope

There are 135 active requirements and 39 active acceptance scenarios across the module and shared product requirements. Eleven native-mobile/device-push requirements and three native-only scenarios remain [deferred](deferred/mobile.md). Existing active release assignments and pending institutional policy approvals remain unchanged.

Current delivery sequencing, when implementation work is derived, is Backend + AI + UI/UX → Frontend → QA, with security after each phase and no UI/UX security assessment. This process does not require role-specific files in the PRD.
