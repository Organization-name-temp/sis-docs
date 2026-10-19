# Screen inventory and navigation

[PRD index](../README.md) · [System parts](../README.md)

## 6. Information architecture, screens and navigation

Every screen must have a Figma reference, role/access annotation, locale behavior and requirement IDs before implementation is marked design-complete. These are logical screen IDs, not assertions that Figma screens exist yet.

| Screen ID | Surface | Screen / main task | Connected requirements |
|---|---|---|---|
| SC-A01 | Public web | Institution/program and intake discovery | ADM-01, I18N-01 |
| SC-A02 | Applicant web | Sign up / sign in / application chooser | IAM-01, ADM-01 |
| SC-A03 | Applicant web | Dynamic stepper with save/resume and docs | ADM-02–ADM-07, I18N-02–05 |
| SC-A04 | Applicant web | Review, attestation, submission receipt | ADM-07–ADM-08 |
| SC-A05 | Applicant web | Application status, missing items, offer | ADM-10–ADM-12 |
| SC-S01 | Student web | Student home / multi-program switcher | WEB-02, ACA-02, IAM-05 |
| SC-S02 | Student web | Registration planner and schedule | REG-01–REG-09, REG-11 |
| SC-S03 | Student web | Results, transcript and degree audit | REC-01–REC-07 |
| SC-S04 | Student web | Balance, holds, requests, notifications | FIN-01–07, WFL-01–04, COM-01 |
| SC-S05 | Research student web | Supervisors, milestones, documents, thesis | RES-01–RES-06 |
| SC-O01 | Staff web | Admissions intake queue and decision panel | ADM-09–11 |
| SC-O02 | Staff web | Registrar / Student 360 and program history | ACA-02, REG-08, REC-03–07 |
| SC-O03 | Staff web | Academic units/programs/curriculum versions | TEN-02, ACA-01–10 |
| SC-O04 | Staff web | Sections, schedules, grade entry/publishing | REG-01, REC-01–03 |
| SC-O05 | Staff web | Tuition, scholarships, payments, reconciliation and holds | FIN-01–07 |
| SC-O06 | Staff web | Graduate coordinator and supervisor tasks | RES-01–06 |
| SC-O07 | Staff web | Forms, visual workflow builder, simulation and approval queue | ADM-02–05, WFL-01–04 |
| SC-O08 | Staff web | Tenant settings, identity, roles and audit | TEN-01–05, IAM-02–09 |
| SC-O09 | Staff web | Academic membership, titles, appointments and TA section assignments | IAM-08/09 |
| SC-O10 | Staff web | Waitlist criteria, optimizer, seat offers and override history | REG-09/11 |
| SC-S06 | Taught-PG web | Dissertation/project advisor, submission and assessment | PGT-01 |
| SC-X01 | AI web | Policy answers and academic planning agents | AI-01–05 |
| SC-X02 | Authorized web | Analytics Q&A and metric/source details | AI-02/04/06 |
| SC-X03 | Authorized web | Extracted document fields, evidence and human review | AI-02/04/07 |

**UI/UX standards:** responsive, no native HTML-like unfinished styling requirement; consistent component tokens; empty/loading/error/success states; optimistic updates only for reversible/low-risk UI; destructive mutations confirmed after server response; multilingual mixed text handled with proper direction metadata. Staff desktop navigation should not be forced into the mobile app paradigm.

**Special attention:** A registration button is not “successful” when tapped; the UI displays success only on confirmed server commit. Student-facing denial must show actionable reason and an approved escalation pathway, not a generic error. Dynamic forms preserve values across locale changes, but never change the meaning of the canonical stored answer.

## Additional module-map screens (design pending)

| Screen | Area / task | Requirements |
|---|---|---|
| SC-O11 | Institution profile, unit responsibility and structure history | INS-01, TEN-02, IAM-08/09 |
| SC-O12 | Configuration registry, calendar publication and version comparison | CFG-01, CAL-01, CUR-01, CAT-01 |
| SC-O13 | Student status/advisor history and matriculation handoff | STU-01/02, ADM-11 |
| SC-O14 | Aid program, applications, award review and budget/disbursement queue | SCH-01–06, FIN-06 |
| SC-S07 | Student aid application, evidence and award history | SCH-02/05, FIN-04 |
| SC-O15 | Report definitions, aid metrics and reconciliation | RPT-04/05, SCH-06 |
| SC-O16 | Tenant lifecycle, support access and plans/entitlements | SAS-01/02 |
| SC-S08 | Inbox and delivery preferences | COM-04/05 |

Screen IDs are planned design entries; no Figma or implemented UI evidence is asserted.
