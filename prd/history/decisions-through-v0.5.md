# Historical decisions through PRD v0.5 — superseded where noted

These records are historical, not current workflow or mobile release obligations. Current decisions are in [governance](../shared/governance.md).

# Governance and decisions

[PRD index](../README.md) · [System parts](../README.md)

## 0. Document governance and decision status

### 0.1 Status vocabulary

- **DECIDED:** User-established strategic choice; change requires explicit decision entry. This does not imply institutional sign-off or implementation verification.
- **R0.5:** Required delivery milestone before R1 acceptance; included in R1 regression and release evidence.
- **PROPOSED:** Product-team recommendation awaiting stakeholder validation.
- **UNRESOLVED:** Requires stakeholder/policy input; architectural support should allow multiple outcomes when feasible.
- **OUT OF R1:** Explicitly excluded from first graduation release but may appear in R2/R3.

### 0.2 Baseline decisions

| ID | Decision | Status | Rationale / evidence |
|---|---|---|---|
| D-001 | One SIS for undergraduate and postgraduate students | DECIDED | Shared identity and academic careers; Oracle precedent [REF-01] |
| D-002 | Career/program models support UG, taught PG and research PG | DECIDED | Reuse academic core; research-specific candidature extension [REF-02] |
| D-003 | React for applicant, student and staff web; Laravel authoritative API/domain | DECIDED | Product conversation |
| D-004 | One student-only mobile app, not applicant/admin/faculty native apps | DECIDED | Product conversation |
| D-005 | More than 99 supported languages with genuine RTL; AR/EN/FR reference validation | DECIDED | Vision v0.2, 2026-10-19 update |
| D-006 | Configurable multi-step applications, saving/resuming, forms versioned | DECIDED | Product conversation |
| D-007 | Multi-tenant SaaS; modular monolith; external integrations | DECIDED | Product conversation |
| D-008 | Deterministic rules for academic/financial record decisions; AI assists | DECIDED | Product conversation and risk control |
| D-009 | R1 includes advanced waitlists, rich tuition/scholarships/integrations and full visual workflows; research depth remains basic | DECIDED | Vision v0.2; supersedes narrow-slice scope |
| D-010 | R1 has a rich tuition engine, scholarships and agreed payment/accounting integrations | DECIDED | Vision v0.2; supersedes basic balance-only scope |
| D-011 | Flutter student mobile uses the same authoritative Laravel API as React web | DECIDED | Vision v0.2; framework selection resolved |
| D-012 | Tenant contains one or more institutions; shared DB + layered isolation first | PROPOSED | Scales to SaaS while allowing enterprise deployment later |
| D-013 | R1 configurable forms authored by schema/config UI, advanced visual admissions form designer R2 | PROPOSED | Allows versioning without building an entire no-code platform in R1 |

### 0.3 Revision decision log — 2026-10-19

These entries record user-established product direction, not institutional acceptance or implementation evidence. Existing requirement IDs are preserved; release changes supersede the prior scope, without claiming historical approvals.

| ID | Decision | Status | Affected requirements / evidence |
|---|---|---|---|
| D-014 | Tenant administrators configure RBAC; titles, qualifications, academic membership, appointments and teaching assignments are separate | DECIDED | IAM-02/03/07–09; AC-023/024; SC-O08/09 |
| D-015 | Full offline student operations and widgets delivered before R1 at R0.5 | DECIDED | MOB-08–10; AC-028/029; SC-M06/07 |
| D-016 | Planning agents, analytics Q&A and document extraction delivered before R1 at R0.5; institution controls activation | DECIDED | AI-01–07; AC-019/030–032; SC-X01–03 |
| D-017 | Advanced waitlist prioritization/optimization belongs in R1; room/timetable solver remains R2 | DECIDED | REG-09/11; AC-025; SC-S02/O10 |
| D-018 | Full visual workflow builder belongs in R1; admissions visual form designer remains R2 | DECIDED | WFL-04; AC-021/027; SC-O07 |
| D-019 | Remove SAML/SCIM enterprise lifecycle automation from product scope | DECIDED | IAM-06 retains only separately validated identity-provider authentication integration; no lifecycle provisioning requirement |
| D-020 | Expand three academic journeys; explain advanced academic policies and official credential exchange, retaining their beyond-R1 scope | DECIDED | ACA-11/13, REC-08, PGT-01; J-UG/J-PGT/J-RES; AC-033 |

D-003, D-005 and D-009–011 above are amended by this revision. D-013 concerns the admissions **form** designer and does not defer the R1 **workflow** builder. Associated APIs, fixtures, screens and test families below are planned contracts; executions and design artifacts remain pending.

### 0.4 Required PRD artifact links (add during implementation)

| Artifact | Suggested location | Owner |
|---|---|---|
| Product vision | `docs/product/vision.md` | Product owner |
| PRD | `docs/product/prd.md` | Product owner |
| Requirement registry | `docs/product/requirements.csv` or issue tracker | Product owner / QA |
| Domain vocabulary & ERD | `docs/architecture/domain-model.md` | Backend lead |
| Design index + Figma references | `docs/design/screen-index.md` | Design lead |
| OpenAPI contract | `docs/api/openapi.yaml` | Backend lead |
| Policy fixture pack | `tests/fixtures/institutions/` | Registrar + QA |
| Traceability / evidence | `docs/quality/traceability.md` | QA lead |
| ADRs | `docs/architecture/adr/` | Tech lead |
| Decision log | `docs/product/decision-log.md` | Product owner |
| UAT results | `docs/quality/uat/` | QA + university SME |
| Release checklist | `docs/releases/R1-checklist.md` | Release owner |

No paths above assert existing repositories or files.

## Module-map revision — PRD v0.3

| Decision | Status | Scope and traceability |
|---|---|---|
| D-021 | DECIDED | Follow the supplied module map's six groups and twenty numbered areas; preserve prior IDs and release commitments |
| D-022 | DECIDED | Retain academic staffing under Institution Management, taught-PG projects/student requests under Student Lifecycle, and omitted delivery/infrastructure requirements under cross-cutting |
| D-023 | DECIDED | AI “decision making” is audit assistance and decision support under D-008, not autonomous official record decisions |
| D-024 | PROPOSED | New map-derived requirements use R1 institutional depth; commercial plan/entitlement automation uses R2; reference policy, provider, budget and acceptance review required |

The [ownership registry](../shared/module-map-alignment.md) identifies all added requirements and canonical owners. Existing R0.5/R1/R2 assignments are unchanged. Scope and document structure are updated; designs, code, test executions and institutional approval remain pending.

## Whole-system clarification — PRD v0.4

| Decision | Status | Effect |
|---|---|---|
| D-025 | DECIDED | The supplied module map is backend-based; its twenty areas organize backend domain/services rather than the whole PRD |
| D-026 | DECIDED | PRD top-level parts are Backend, Frontend, AI, Testing, Product Design, Security and Mobile; each has a dedicated specification/index |
| D-027 | DECIDED | Retain a single canonical requirement and trace all applicable parts to it; backend delivery alone does not close a system feature |

D-025 clarifies the organizational scope of D-021. Existing 146 requirement rows, 42 acceptance scenarios, release assignments and pending policy/acceptance states remain unchanged. No new implementation, certification or deployment approval is implied.

## Business-module workflow correction — PRD v0.5

| Decision | Status | Effect |
|---|---|---|
| D-028 | DECIDED | Product PRD is organized by business modules; analyze one module in detail and update its files before assigning its role work |
| D-029 | DECIDED | Backend and UI/UX are the first work tracks after module analysis; Frontend and Mobile follow; integrated Testing follows client work |
| D-030 | DECIDED | Security reviews analysis and each Backend, UI/UX, Frontend, Mobile and Testing output before the next applicable handoff; AI changes also receive security review |
| D-031 | DECIDED | AI work runs in parallel with analysis and every delivery stage, coordinated with approved contracts and client/test outputs |

D-028 supersedes D-026's top-level role-folder organization. The roles still cover the whole system, but their work packages now live inside each business module. D-025 remains valid: the backend map is a service/domain reference, not the product's top-level PRD. The corrected [workflow](../workflow.md) governs handoffs. All 146 canonical requirement rows, 42 acceptance scenarios and existing release assignments remain unchanged. Draft analysis and role briefs do not imply institutional approval, implementation or passed security/testing evidence.
