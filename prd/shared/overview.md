# Scope, people and lifecycle

[PRD index](../README.md) · [System parts](../README.md)

## 1. Context, value and scope

### 1.1 Problem statement

Institutions need one trustworthy academic record and configurable processes across admission, registration, study, graduate research, graduation and student services; students need clear, multilingual self-service; operational teams need policy enforcement, integration, scoped access and auditable changes. Generic CRUD designs fail when concurrent seats, curricular history, program-specific rules, approvals and person/career relationships become complex.

### 1.2 Target outcomes and constraints

| Outcome ID | User/business outcome | Indicator | Target / calibration |
|---|---|---|---|
| O-01 | Applicants finish valid applications | Application submission success, draft integrity, abandon rate | 100% approved validation fixtures; pilot completion baseline to be measured |
| O-02 | Registrations are policy-correct | Invalid successful enrollments; seat oversell | **0** in signed-off fixtures and concurrency suite |
| O-03 | Academic record is historically explainable | Golden-case transcript/GPA/degree-audit agreement | **100%** signed-off fixture agreement |
| O-04 | Staff can see appropriate student context | Unauthorized record disclosures, UAT task success | 0 tenant/object-scope exposures in negative suite |
| O-05 | More than 99 supported languages with usable multilingual journeys | Supported language roster, critical language-pack and LTR/RTL coverage | At least 100 released language packs; 100% release-critical paths per released pack, with full AR/EN/FR reference regressions |
| O-06 | Students complete core self-service tasks | Responsive web critical-path completion | UAT target ≥90% task success (proposed) |
| O-07 | Institutions configure roles, policies and visual workflows safely | Code-free supported configuration and publish validation | Zero code edits for supported R1 role, rule, field and workflow node classes |
| O-08 | Operators can recover and diagnose | Restore drill, tracing, error monitoring | Restore drill passes and incident runbook rehearsed |
| O-09 | AI is explainable and non-authoritative | Grounded answer / authorization checks | No unapproved mutation; policy source linked in covered cases |

### 1.3 Release scope

**R0.5:** AI-01–07 scoped explanations, planning, analytics Q&A and reviewed document extraction, with evaluation and required Backend/AI/frontend/QA phase evidence.

**R1:** Active tenancy/RBAC/institution, academic rules/catalog/curriculum, admissions/matriculation, UG/taught-PG/research slice, sections/registration and advanced waitlists, official record/GPA/degree review, rich tuition/aid/payment/accounting integrations, full visual workflows, requests/inbox/email, responsive React portals, more than 99 languages, APIs/events and operational/security readiness.

**Later releases:** Existing R2/R3 policy/research/exchange/warehouse/commercial/jurisdiction depth remains as assigned. **Removed for now:** native mobile, offline operations, widgets and device push have no active release or gate.

### 1.4 Explicitly out of scope for R1

Complete LMS; external library/HR/payroll/ERP rebuild; K–12 school specializations; applicant mobile app; faculty/admin native apps; broad government compliance certification; autonomous admit/reject/expel/scholarship decisions; production AI-based degree eligibility; official government-approved diplomas/credentials; fully customizable arbitrary code in rules and form editor.

---

## 2. Personas, responsibility and visibility

| Persona / example role (tenant-configurable) | Can do | Restrictions / review points |
|---|---|---|
| Applicant | Own applications, documents, offers | No enrolled student operations until matriculation |
| Student | View own programs, schedule, registrations, grades, bills, requests and relevant research | May access only own records; no official grade edits |
| Admissions officer | Review applications and issue authorized decisions | Assigned institution/intake; decision rights granular |
| Registrar | Academic records, registration exceptions, degree checks | Audited override reasons; sensitive edits require policy/approval |
| Academic administrator | Publish programs, curricula, terms, sections, academic policies | Published versions immutable; role scope by academic unit |
| Instructor / examiner | See assigned rosters; submit grades | Only assigned sections; can amend after close by approved workflow |
| Academic member | Perform authorized academic duties, including non-teaching work | Dr./PhD/rank and membership convey no implicit teaching or permissions |
| Teaching assistant (TA) | Assist assigned sections, tutorials, labs and permitted assessment preparation | May also be a taught-PG student and academic member; cannot alter own grades or approve own requests |
| Faculty dean / department dean or head | Authorized unit oversight, reporting and approvals | Appointment need not include teaching; access stays within assigned unit |
| Advisor | View assigned students and provide advice | No arbitrary access to all student PII |
| Graduate coordinator | Track candidate assignments/milestones | Assigned graduate unit/program scope |
| Supervisor | See assigned candidates; approve assigned milestones | No unrelated candidates |
| Finance officer | Charges, credit entries, holds, reconciliation | Cannot change academic grades |
| Tenant administrator | Configure locale, roles, units, tools, settings | Cannot bypass academic records policy by default |
| Platform operator | Provision tenants, view health/billing metadata | No routine academic PII access; exceptional support access time-bound, logged, approved |

**Authorization model:** Tenant administrators create, edit and assign configurable roles and permission sets within platform-defined permission and tenant-isolation boundaries; persona labels above are examples, not hard-coded roles. RBAC for functional privileges **plus** scoped access (tenant, institution, unit, student assignment, section, career, role). Titles, degrees (including PhD), academic membership, effective-dated leadership appointments and section teaching assignments remain separate from permissions. One Person may hold student, TA and academic associations concurrently; a PhD holder is distinct from a PhD candidate. Prevent self-grading/self-approval and revoke changed privileges across sessions, jobs and caches. Default deny. Every backend handler checks record-level authorization independently of UI gating and API URL structure.

---

## 3. End-to-end lifecycle map

```text
Visitor → Applicant Account → Application Draft (wizard) → Submitted
 → Admissions Evaluation / Missing Items / Interview (configured)
 → Offer / Reject / Waitlist → Offer Acceptance
 → Matriculation [ONE Person + ONE StudentProfile, many ProgramEnrollments]
 → Student Program/Curriculum Assignment → Enrollment Window + Holds
 → Course Section Selection → Validation → Transactional Registration
 → Attendance/Assessments in LMS (external) → Final Official Grades
 → Transcript + GPA + Degree Audit → Completion Review → Award

Research path (optional on ProgramEnrollment):
 → Candidature → Topic + Supervisor(s) → Milestones → Progress Reviews
 → Thesis Submitted → Research Evaluation / Committee [R2 depth] → Award
```

**Key separation:** application/admission and academic course registration are different domains, endpoints, states and permissions. A postgraduate student may complete course registration **and** separate research milestones. The same person may be concurrently enrolled in more than one career/program.

## Current module workflow

Analyze/update a business module first. Phase 1 is Backend + AI + UI/UX, Phase 2 Frontend, Phase 3 QA. Security follows each phase: S1 Backend/AI, S2 implemented Frontend/integration, S3 QA/results. UI/UX receives product/design/accessibility review with no security assessment. Native mobile is deferred. See the [workflow](../workflow.md) and [release gates](release-plan.md).
