# Governance and current decisions — PRD v0.6

[Business module index](../README.md) · [Workflow](../workflow.md)

## Decision status

DECIDED records user-established product/workflow choices, not implementation, institutional acceptance or security approval. PROPOSED map-derived institutional policies and schedules still require review. DEFERRED has no active release requirement. Earlier decisions are retained in [historical records](../history/decisions-through-v0.5.md), and superseded mobile/workflow statements there are not current instructions.

| ID | Current choice | Status |
|---|---|---|
| D-001/002 | One SIS/shared identity for UG, taught-PG and research journeys | DECIDED |
| D-003 | React frontend and authoritative Laravel backend | DECIDED |
| D-005/006 | More than 99 language packs with RTL/LTR and versioned dynamic admissions | DECIDED |
| D-007/008 | Multi-tenant modular monolith; deterministic official decisions; AI assistance only | DECIDED |
| D-009/010/017/018 | Advanced waitlists, rich tuition/scholarships/integration breadth and full visual workflow builder remain R1 | DECIDED |
| D-014 | Tenant-configured RBAC; titles/appointments/teaching duties separate; student/TA overlap supported | DECIDED |
| D-016 | AI explanations/planning/analytics/extraction remain pre-R1 (R0.5) | DECIDED |
| D-019 | Enterprise identity lifecycle automation removed | DECIDED |
| D-024 | Added map-derived institutional policies and schedules need stakeholder review | PROPOSED |
| D-028 | Business-module organization; analyze/update a module before assigning role work | DECIDED |
| D-032 | Mobile removed for now: Flutter app, native offline operations, widgets and device push have no active release | DECIDED |
| D-033 | Phase 1 Backend + AI + UI/UX; Phase 2 Frontend; Phase 3 QA; security review follows each phase | DECIDED |
| D-034 | No UI/UX security assessment; S1 assesses Backend/AI, S2 Frontend/integration, S3 QA/security evidence | DECIDED |

D-032 supersedes earlier native mobile and offline/widget/device-push release commitments. D-033/034 supersede the old per-role security gates, standalone design security review and separately parallel AI workflow. UI/UX retains product/design/accessibility review. Existing active domain requirements/releases and policy approval limits remain binding.

No document edit grants permission to deploy or proves implemented/tested/secure behavior. Owners, policy fixtures, designs, executions, institution approval and deployment authorization remain PENDING until supported by evidence.

## Single module PRD correction — v0.7

D-035 — DECIDED: Each business module has one product requirements document. Separate Backend, AI, UI/UX, Frontend, QA and Security files are not needed in the PRD; work for those roles is derived later from the reviewed module PRD. This supersedes earlier organizational references requiring role files inside a module. Existing phase order, mobile deferral, active requirement scope and security boundaries remain unchanged.
