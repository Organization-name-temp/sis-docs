# Modern SIS — Product Vision

**Version:** 0.3 (web-first delivery; mobile deferred)  
**Date:** 2026-10-19  
**Status:** Draft for product discovery and stakeholder approval  
**Companion:** [Modular PRD](prd/README.md)  
**Product:** Modern SIS (working name)

> This vision states our current product direction. It is not a claim that requirements have been approved by a real university, security-certified, or already validated in production. The PRD identifies assumptions requiring discovery.

## 1. Executive vision

**Vision:** Make the academic lifecycle of every university student—from application and admission to studies, research, graduation, and lifelong academic record—clear, configurable, trustworthy, and accessible through a modern multilingual platform.

**Product statement:** Modern SIS is an AI-assisted, multi-tenant higher-education Student Information System (SIS) that unifies undergraduate, taught-postgraduate, and research-postgraduate academic journeys in one configurable platform. It provides versioned curricula and academic rules, admissions and registration, academic records, graduate-research tracking, student self-service, workflows, and integration-ready APIs. Its administrative web experience is built with React, its domain and APIs with Laravel, with responsive React portals for applicants, students and staff. Native mobile delivery is deferred.

**Positioning:** **University-grade academic integrity with modern SaaS usability**—not a generic school administration application and not an attempt to reproduce the entire enterprise ERP stack.

**Primary promise:** Institutions can configure and operate their academic policies without changing source code for routine differences; students can understand their status, obligations, and next actions; staff can make traceable, rule-grounded decisions.

## 2. Problem and opportunity

### Problems observed / to validate

1. Academic data is fragmented between admissions, student records, registration, finance, departmental records, and separate LMS tools.
2. Rigid workflows force institutions to customize software or rely on spreadsheets for exceptions and approvals.
3. Curriculum and registration rules evolve, but student historical entitlements must remain stable and explainable.
4. Undergraduate, taught-postgraduate, and research-postgraduate students share a person/identity layer but follow different academic processes.
5. Student experiences are inconsistent across web, mobile, departments, and languages.
6. AI features often risk inventing policy or exposing information without robust authorization and deterministic validation.

These are **hypotheses to validate with institutional interviews**, not proven findings about every university.

### Why now

Mature systems demonstrate established higher-ed patterns—academic careers, program-specific policy, research candidates, transcripts, registration and advisement—while modern standards and AI increase expectations for interoperable, self-service experiences. The competitive opportunity is delivering the most valuable higher-ed capabilities without the legacy complexity of enterprise implementations. See [S1–S6](#17-research-and-standards-references).

## 3. Target institutions and users

### Beachhead segment (proposed)

- **First target:** small-to-mid-sized universities and specialist higher-education institutions seeking a modern multilingual SaaS SIS; design for Egypt/MENA, but do not bake any country's policies into the domain model.
- **Secondary:** private universities, branch campuses, graduate-focused institutions, and institutions in Arabic/French-speaking markets.
- **Later:** larger multi-campus institutions, regulated international campuses, and dedicated deployments.

### Personas / jobs to be done

| Persona | Core job | Their definition of value |
|---|---|---|
| Applicant | Apply for program/intake and follow decision | Clear, resumable application; no redundant data entry |
| Undergraduate student | Enroll in courses and progress toward degree | Eligibility explained; accurate timetable, grades, fees, credits |
| Taught-postgraduate student | Complete advanced coursework and dissertation | Correct program-specific rules and milestones |
| Research-postgraduate candidate | Manage candidature and thesis lifecycle | Supervisor, progress reviews, milestone and submission transparency |
| Registrar | Safeguard official student record | Policy-grounded changes, overrides, audit, reliable reporting |
| Admissions officer | Process applicant pipeline | Configurable forms, review queues, decisions and requirements |
| Academic administrator | Maintain catalogs, sections and policies | Publish versions without corrupting historical records |
| Academic member | Maintain academic affiliation and perform assigned academic duties, including non-teaching work | Appropriate access without requiring a teaching assignment |
| Instructor / examiner | Submit grades, manage approved teaching and assessment tasks | Scoped workflows, safe edits and deadlines |
| Teaching assistant (TA) | Support assigned teaching, tutorials, labs and assessment preparation | Explicit task permissions and section scope; compatible with concurrent postgraduate study |
| Academic leader / dean / department head | Govern a faculty or department, whether teaching or not | Scoped oversight, approvals and reporting independent of teaching assignments |
| Academic advisor | Support student progression | Trusted degree progress, warnings, planning and case notes |
| Graduate coordinator / supervisor | Manage research candidates | Milestones, assignments, approvals and thesis progress |
| Finance officer | Manage tuition-related student balances and holds | Reconciled, auditable academic finance interfaces |
| Tenant administrator | Configure institution and control access | Tenant-configured RBAC, scoped role assignments, delegated administration and predictable operations |
| SaaS operator | Operate subscriptions and platform | Tenant lifecycle, usage, support and observability without routine access to academic PII |

## 4. Product principles (non-negotiable)

1. **One person, many academic journeys.** Do not duplicate `undergraduate_student` and `postgraduate_student` identity records. Support sequential and concurrent academic careers/program enrollments.
2. **Deterministic rules are authoritative.** Eligibility, fee, grading and degree decisions come from versioned domain logic. AI may retrieve, explain and simulate—not decide or mutate implicitly.
3. **Academic history is preserved.** Published curricula, applied requirement versions, grade changes, admission form versions and consequential approvals have traceable history.
4. **Configuration over per-client forks.** Programs, admissions steps, policy thresholds, workflows, terminology and translations are configuration, with guardrails.
5. **Security and tenancy by design.** Every access includes tenant, role, object scope and purpose. Cross-tenant leakage is a release blocker.
6. **Responsive web for current delivery.** Applicants, enrolled students, staff and administrators use React web portals. Native mobile, offline operations, widgets and device push are deferred with no active release commitment.
7. **Localization is domain-wide.** Support more than 99 languages across interfaces, metadata, dynamic fields, communications, documents and accessibility, including RTL. Arabic, English and French are initial reference languages for validation, not a product language limit.
8. **Interoperate rather than rebuild everything.** An SIS should integrate with LMS, identity, payment, accounting and communications systems.
9. **Truthful product maturity.** Distinguish implemented, tested, piloted, planned and excluded capabilities. A production-ready slice is not a complete enterprise SIS.
10. **Every requirement has a closure path.** The linked PRD requires acceptance evidence, negative tests, operational signals and feedback resolution.

## 5. System-of-record boundaries

**Owned by the SIS:** applicant/application record; admission decision and matriculation; person/student identity mapping; academic careers and program enrollments; institution/campus/academic structure; calendar, curriculum and academic rules; class sections and official registration; official grades, academic standing, transcript and degree audit; research candidature and milestones (as enabled); student academic requests; official approvals and audit; finance-related charges, student balance and academic holds at defined scope.

**Integrated, not rebuilt in initial scope:** LMS learning content and assignment-by-assignment teaching workflow; full general ledger and payroll; CRM marketing automation; video conferencing; library circulation; campus housing; identity federation; payment processing; complex HR/ERP.

**Data authority rule:** For each connected system, configure a source-of-truth mapping and conflict resolution policy. A synchronized LMS enrollment is a projection of authoritative SIS registration, not an independent registration authority.

## 6. UG + PG in a single architecture

**Decision: one SIS, shared identity, configurable `AcademicCareer` and optional lifecycle modules.** Oracle PeopleSoft uses academic careers (undergraduate, graduate, professional, etc.) and allows separate or concurrent programs within one student structure [S1]. Research management adds candidatures, supervisors, topics and thesis tracking [S2].

```text
Tenant
 └─ Institution(s)
     ├─ AcademicUnit / Campus / Department
     ├─ AcademicCareer [configured by institution]
     │   ├─ Undergraduate
     │   ├─ Graduate Coursework
     │   ├─ Graduate Research
     │   └─ Other permitted careers
     ├─ Programs / ProgramVersions / CurriculumVersions
     └─ Terms / Sessions / Sections

Person
 ├─ ApplicantProfile → Application(s)
 ├─ AcademicMembership(s) → Unit / Rank / Qualification(s)
 ├─ Appointment(s) → TA / Dean / DepartmentHead / Other
 ├─ TeachingAssignment(s) → Section / Responsibilities
 ├─ ScopedRoleAssignment(s) → TenantConfiguredRole / Permissions
 └─ StudentProfile
     ├─ CareerMembership(s)
     └─ ProgramEnrollment(s) → AppliedCurriculumVersion
          ├─ Registrations / AcademicRecords
          └─ ResearchCandidature? → Supervisors / Milestones / Thesis
```

| Journey | Shared capabilities | Distinct extension |
|---|---|---|
| Undergraduate | Admission, enrollment, registration, marks, transcript, degree audit | Prerequisite, cohort and general-elective policies |
| Taught postgraduate | All relevant academic core modules | Dissertation/project; different grade, progression and credit rules |
| Research postgraduate | Admission, identity, program status, documents, requests | Candidature, supervisor(s), topic/proposal, progress reviews, leave/extensions, submission/assessment |

**Cross-career rules:** a student may have concurrent programs; grades and credits belong to the correct offering/enrollment and are included in other requirements only when explicit credit-transfer/cross-list rules allow. The student must never be conflated with a single global `level`.

### 6.1 Academic membership, appointments and tenant-configured RBAC

- **One person can hold overlapping identities.** A taught-postgraduate student may also be a teaching assistant and an academic member. Student enrollment, academic membership and employment/appointment records remain separate associations of the same person.
- **Titles and qualifications are descriptive data.** Doctor/Dr., a PhD qualification, academic rank and faculty/department membership do not automatically grant teaching duties or system permissions. A PhD holder is distinct from a research student pursuing a PhD.
- **Leadership does not imply teaching.** Faculty deans, department deans/heads and other academic leaders may hold governance appointments without any teaching assignment. Tenant terminology can label these appointments according to institutional practice.
- **Teaching is an explicit assignment.** Instructors and TAs receive section/course assignments with effective dates and responsibilities. Academic membership alone does not make someone an instructor.
- **Access uses role-based access control (RBAC).** Tenant administrators create and configure roles, compose permissions and assign users within institution, faculty, department, program, section or record scopes. Persona names in this vision are examples of jobs, not a fixed role list. Published permission definitions and platform tenant-isolation constraints bound what tenant administrators can delegate.
- **Overlapping access remains controlled.** A student/TA can assist assigned sections only within granted permissions; they cannot approve their own academic requests or alter their own official grades. Role changes, scoped assignments and consequential actions are audited.

### 6.2 End-to-end student journeys

| Stage | Undergraduate | Taught postgraduate | Research postgraduate |
|---|---|---|---|
| Discovery and application | Explore programs, submit prior-school qualifications and program preferences | Explore advanced programs, submit prior degree, prerequisites and experience where required | Explore research areas, submit prior qualifications, proposal and prospective supervisor information where required |
| Admission and entry | Meet offer conditions, accept offer, matriculate and complete orientation | Resolve bridging/qualifying requirements, accept offer and matriculate | Complete proposal/supervisor review and funding conditions where applicable, accept offer and establish candidature |
| Academic planning | Adopt curriculum version, plan core/elective courses and consult advisor | Adopt program rules, plan modules, specialization and dissertation/project route | Agree research plan, supervisory team, milestones and any required research coursework |
| Active study | Register/add/drop, attend via LMS integration, complete assessments and track credits/GPA | Register advanced coursework, complete assessments and progress through dissertation/project approvals | Complete induction and applicable ethics approvals, conduct research, record supervision and submit progress reviews |
| Progress and changes | Receive progression decisions; request leave, repeats, transfer or curriculum exceptions | Track coursework and project progression; request leave, extensions or permitted route changes | Complete confirmation/upgrade where applicable; manage leave, extensions, supervisor/topic changes and progress interventions |
| Completion and award | Satisfy degree audit, resolve holds, receive graduation approval and award | Satisfy coursework and dissertation/project assessment, resolve holds and receive award | Submit thesis, complete examination/viva and corrections where required, obtain final approval and deposit thesis |
| Lifelong record | Access official academic record and request transcripts/credentials | Access official academic record and request transcripts/credentials | Access official record, thesis outcome and request transcripts/credentials |

All three journeys share status visibility, tuition/scholarship information, notifications, accessible multilingual responsive web experiences, requests, appeals and traceable approvals. A taught-postgraduate student serving as a TA can switch between student and authorized academic web contexts without creating a second person record. Staff duties remain in the web workspace. The table expresses product lifecycle coverage; release depth follows the priorities and roadmap below.

## 7. Main product surfaces

### 7.1 Public applicant portal — responsive web
- Program discovery and requirements.
- Configurable application wizard that adapts to program, intake, citizenship and academic career.
- Draft/resume/autosave; supporting documents; prior-education repeaters; conditional fields; validation; review and final declaration.
- Submission confirmation; document requests; interview/offer status; offer acceptance and matriculation.

### 7.2 University web workspace — React
- Role-appropriate surfaces for admissions, registrar, academic administrators, finance, advisors, instructors, TAs, non-teaching academic members, faculty/department leaders, supervisors and tenant administrators, using tenant-configured RBAC.
- Operational dashboards, work queues, catalog setup, course sections, grades, degree audit, bulk operations, requests, reporting and audit history.
- Destructive or high-impact actions have permissions and reason capture.

### 7.3 Student web self-service — React
- Current program(s), registration, academic history, degree progress, holds, finance, requests, communications and research (when applicable).

### 7.4 Native mobile — deferred

Native Flutter, mobile offline operations, widgets and device push are removed from current delivery. No mobile role, design, implementation, QA or security gate applies. Prior scope is retained in the PRD deferred record; a future explicit decision is required to reactivate it. Responsive web remains usable on narrow browser screens; native removal does not introduce a new offline-web requirement.

## 8. Core capabilities and priorities

| Platform capability | Graduation release R1 (includes pre-R1 deliveries) | Beyond R1 |
|---|---|---|
| Tenant/institution onboarding | Manual/provisioned onboarding, isolation, settings | Self-service SaaS billing and enterprise hosting |
| Identity/security | Tenant-admin-configured RBAC, scoped assignments, overlapping student/academic identities, audit, tenant context | Expanded identity-provider integrations as validated |
| Admissions forms | Versioned multilingual multistep and resumable | Visual no-code form editor, advanced fraud tools |
| UG + taught PG | Full example admission→graduation journeys, including coursework and dissertation/project | Transfer/dual-degree at broad institutional depth |
| Research PG | Candidate identity, supervisors, milestones, submission tracking | Committees, examination panel, advanced ethics/publication processes |
| Academic rules | Prerequisites, holds, loads, repeat policy, required credits | Sophisticated substitutions, articulation, full policy DSL (explained below) |
| Enrollment | Sections, timetable clash, add/drop, capacity, transactional seat booking; advanced waitlist prioritization and optimization | Further institution-specific enrollment policies |
| Records | Grades, GPA, transcript, versioned degree audit | Official credential exchange (explained below) |
| Finance | Rich tuition engine, scholarships, charges/balance/holds, provider abstraction and integration breadth across agreed payment/accounting providers | Additional jurisdiction-specific funding and reporting adapters |
| Workflow | Configurable linear/conditional approved templates and full visual workflow builder | Additional institutional workflow packs |
| Localization | Support more than 99 languages, dynamic content and RTL; AR/EN/FR reference regression plus validation of every released language pack | Ongoing translation quality and terminology expansion |
| Native mobile | Deferred; no current R1 delivery or acceptance gate | Reconsider only after explicit scope decision |
| AI | Permission-aware explanations plus planning agents, analytics Q&A and document extraction delivered before R1 | Further evaluated AI use cases |
| Integrations | Versioned REST, webhooks, demonstrable LMS-facing integration and finance integration breadth | Broader EduAPI alignment, LTI, ERP integrations |

**Release timing:** pre-R1 AI delivery is the R0.5 milestone below and remains part of R1 acceptance. Earlier mobile/offline/widget delivery commitments are superseded. R1 includes the expanded waitlist, finance and visual workflow scope above. **R2:** institution pilot expansion. **R3:** enterprise depth. The companion PRD must be reconciled with these updated priorities before it is used as the execution contract.

### 8.1 Academic-rule terms explained

- **Sophisticated substitutions:** controlled exceptions allowing one course, credit combination or approved prior learning to satisfy a different curriculum requirement. For example, two approved specialist modules may replace a required module for a particular student/cohort. Record applicability, credit/grade constraints, approval, effective dates and the affected curriculum version; prevent double-counting and preserve the reason shown in degree audit.
- **Articulation:** institution-approved mappings of external study or qualifications into local credit and requirements. For example, a partner institution's completed diploma may satisfy a defined block of first-year requirements. Agreements can vary by partner, program, grade, syllabus and effective period; preserve the original evidence and the mapping version used.
- **Full policy DSL:** a domain-specific language for expressing complex academic policies as versioned, validated rules, such as progression conditions that combine GPA, credits, course groups and exceptions. It extends the initial configurable rule primitives. It needs safe authoring, simulation, explainable outcomes and approval; it does not permit arbitrary tenant code or AI-generated rules to bypass review.

### 8.2 Official credential exchange explained

**Official credential exchange** means securely sending and receiving institution-issued transcripts, awards or other academic credentials between authorized institutions, employers or credential services, with the student's consent or another documented institutional authority. It goes beyond downloading or printing a transcript: the recipient must be able to verify the issuer, integrity, intended recipient, delivery and current credential status, including correction or revocation where supported. Protocols, partners, legal requirements and verification mechanisms require discovery; no standards certification or universal recipient compatibility is implied. This remains beyond R1.

## 9. Differentiation vs competitors

| Competitor cluster | Their documented strength | Modern SIS opportunity |
|---|---|---|
| Ellucian / Workday | Broad academic modules, rule-driven enterprise workflow, AI direction | Focused workflow clarity, simpler operations, regional language depth |
| PeopleSoft / SITS | Deep academic careers and research tracking | Modern cross-career navigation and configurable UX |
| Populi | Small-institution usability and simple pricing | More sophisticated career, curriculum and research model |
| openSIS / Classe365 | Accessible cloud pricing and modularity | More rigorous higher-ed-specific rules, degree audit and workflow traceability |

Do **not** assert these competitors have no localization, workflow or AI capabilities. Differentiation is an opportunity to validate through user tests, not an established absence in competing products.

## 10. Business model hypothesis

**Proposed commercialization:** base platform fee + active-student tiers + optional admissions/advanced research/analytics/AI/enterprise modules + one-time migration/implementation + metered third-party pass-through costs (messaging, AI, payment processing). Core academic correctness, security, audit and degree progress should not be paywalled.

**Packaging hypotheses:**
- **Core:** Admissions, academic core, registration, records, student portals, baseline audit and support.
- **Growth:** Advanced workflows, integrations, research management, deeper analytics.
- **Enterprise:** Dedicated tenancy, identity-provider integrations, configurable retention/residency, SLAs and premium support.
- **AI:** Usage-governed, separately activated, with consent/control and institutional data protections.

**Unit of account:** active students is the leading option; specify exact definition (e.g., current-term matriculated/enrolled as defined contractually), handling concurrent careers and seasonal peaks. Do not invent definitive prices before discovery.

## 11. Product objectives and measurable outcomes

All numerical targets below are **proposed acceptance hypotheses**, not validated market performance or promised SLAs.

| Objective | Leading metric | Initial target to validate |
|---|---|---|
| Faster admissions | Median completion time for eligible application | ≤20 minutes excluding document preparation |
| Better form usability | Started-to-submitted completion; save/resume success | ≥95% draft resume integrity in usability testing |
| Registration trust | Invalid registration accepted | **0** in rule test suite |
| Registration reliability | Successful confirmed concurrent booking without overcapacity | **100%** in concurrency test suite |
| Student clarity | Correct explanation of registration denial | ≥95% of test cases matched to policy/rule source |
| Data integrity | Cross-tenant reads/writes in negative suite | **0** |
| Localization | More than 99 supported languages; passed release-critical language-pack scenarios and AR/EN/FR + RTL reference regressions | **100%** for release-critical paths |
| Academic correctness | Degree-audit golden-case agreement | **100%** on approved fixture corpus |
| Product adoption | Weekly student self-service active usage | Baseline first; target after pilot |
| Support | Time to resolve common registration problem | Baseline first; monitor improvement |

**North-star outcome:** percentage of eligible student lifecycle tasks completed accurately through self-service **without staff correction**. Instrument carefully to avoid rewarding invalid transactions or low-quality support.

## 12. Non-goals and deliberate limits

- No separate undergraduate and postgraduate codebases.
- No standalone native staff/teacher/admin application.
- No built-in LMS course content engine or synchronous classroom platform.
- No payroll/full accounting or complete CRM/marketing suite.
- No autonomous AI academic decisions or unsupervised record mutations.
- No claim of compliance certification, standards certification or universally deployable policies before formal assessment.
- No promise to support every country's student-funding or government reporting regime in R1.

## 13. High-level technical direction

```text
Web: React applicant + student + staff workspaces
Native mobile: deferred — no current release
                  │
             Versioned API
                  │
       Laravel modular monolith
      Identity / Tenancy / Admissions
      Academics / Curriculum / Enrollment
      Records / Research / Finance / Workflow
      Integration / Reporting / AI Gateway
             │          │          │
         PostgreSQL    Redis      S3-compatible
             │          │
          Outbox       Queues / Workers
             └──── domain events / webhooks
```

- Laravel owns business rules, persistence and authorization. React does not duplicate policies as a second source of truth.
- Prefer PostgreSQL with tenant-scoped keys, row-level security where practicable, and explicit job/transaction tenant context. Database-level controls complement application authorization; they are not a substitute for object- and purpose-level policy checks.
- Policy sets, curricula, forms and consequential workflows are immutable after publication or explicitly revised through controlled versioning.
- Record changes propagate via after-commit jobs/transactional outbox so downstream integration failures cannot silently corrupt official academic transactions.
- Document uploads require size/type controls, malware scanning where available, signed URLs and access logging.
- Every AI tool call uses the same scoped APIs as human user actions, with redacted prompts and human confirmation for write operations.

## 14. Roadmap and milestones

### R0 — Product discovery and foundations
- Interviews with registrar, admission, academic admin, advisor and research coordinator.
- Define policy fixtures and reference institution; approve core domain vocabulary.
- Create a design system and translation infrastructure supporting more than 99 languages, with AR/EN/FR reference designs and responsive RTL patterns.
- Define tenant-configured RBAC, academic titles/appointments, TA responsibilities and overlapping student/academic identity fixtures.
- Establish tenant, identity, audit, domain architecture and test harnesses.

### R0.5 — Pre-R1 AI delivery
- Deliver scoped policy explanations, planning agents checked through deterministic rules, analytics Q&A and document extraction with sources and human review before official-record writes.
- Validate authorization, grounding, prompt-injection/PII handling, confirmation and provider failure with versioned evaluations.
- Use Phase 1 Backend+AI+UI/UX → Phase 2 Frontend → Phase 3 QA with security S1/S2/S3; UI/UX is not security-assessed.
- Native mobile, offline operations, widgets and device push are deferred and excluded from current gates.

### R1 — Graduation demonstration + production-grade slice
- Configure reference institution: programs across UG, taught MSc and a limited research MSc/PhD pathway.
- Complete 2–3 application wizard templates; admission decision and matriculation.
- Publish a versioned curriculum; create sections; validate registration policies under concurrent load.
- Deliver advanced waitlist prioritization and optimization with configurable criteria, explainable ordering, audited overrides and transactional seat offers.
- Deliver the rich tuition engine, scholarships and agreed payment/accounting integrations with charge calculation, award application, payment/refund reconciliation and holds.
- Deliver the full visual workflow builder with conditional routing, scoped approvals, version publication, validation and execution history.
- Submit official grades, calculate GPA, issue unofficial transcript, run degree audit.
- Register research candidature and evaluate basic milestones.
- Operate React web roles using tenant-configured RBAC, including TAs, non-teaching leaders and student self-service.
- Support more than 99 languages with dynamic-content localization, RTL and release-critical language-pack validation; retain AR/EN/FR reference regression journeys.
- Demonstrate webhook/LMS-facing integration and auditable notification.
- Include the pre-R1 AI deliveries in release evidence; validate planning, analytics Q&A, extraction and permission-aware explanations under institution-controlled activation.
- Prove with passing functional, authorization, isolation, concurrency, backup-restore and performance evidence.

### R2 — Institutional pilot
- More complete withdrawal/leave and transfer policies; expand validated finance integrations beyond the R1 provider set.
- More sophisticated research workflow, flexible approvals and bulk data operations.
- Operational runbooks, support process, institution configuration pack, validated migration.

### R3 — Enterprise expansion
- Dedicated tenant deployments, enhanced governance, expanded analytics, institution-specific terminology packs and country reporting adapters.

## 15. Strategic risks and validation activities

| Risk | Why it matters | Mitigation / validation |
|---|---|---|
| Institutional rules are too diverse | Unbounded engine configuration | Identify minimum rule primitives; validate with 2+ contrasting institutional case sets |
| Research workflow scope explodes | Thesis/examination policy is complex | R1 basic candidature/milestone track; advance only with reference program workflows |
| University trust requires migration | Legacy data may be inconsistent | Import staging + mappings + preview + reject report + reconciliation |
| Admissions forms become arbitrary scripting | Security and maintainability | Safe declarative schema; no arbitrary tenant-supplied code |
| AI gives wrong eligibility | Consequential student harm | Tool-only policy retrieval, deterministic checker, human escalation, eval corpus |
| Multi-tenancy leak | Highly sensitive academic records | Composite constraints, isolation tests, audit and gated reviews |
| Multilingual content completeness | English-only user-generated policy/form descriptions | Publish-time localization completeness checks + explicit fallback labels |
| Regulatory variation | Egypt, EU and other regimes differ | Per-jurisdiction legal/privacy assessment before institution onboarding |

## 16. Decisions and open discovery

### Locked by product discussion
- One SIS; multiple academic careers; UG, taught PG and research PG supported in product model.
- Applicant, student and staff responsive web experiences; native mobile is deferred.
- React frontend and Laravel backend; modular monolith. Native mobile has no active release.
- More than 99 supported languages with genuine RTL and AR/EN/FR reference validation; dynamic/versioned multi-step applications.
- Tenant-admin-configured RBAC; titles, qualifications, academic membership, teaching assignments and leadership appointments are separate from access permissions.
- Advanced waitlists, rich tuition/scholarships/integration breadth and the full visual workflow builder in R1.
- Planning agents, analytics Q&A and document extraction delivered before R1; native offline operations/widgets/device push deferred.
- Rule-driven academic decision-making; AI support cannot override official rules.

### Recommended but requiring stakeholder ratification
- Reference-institution research depth and acceptance fixtures within the updated R1 scope.
- Exact supported language roster (more than 99), translation ownership and release-quality evidence.
- R1 finance integration provider set, tuition/scholarship policy fixtures and waitlist optimization criteria.
- Initial target institutional segment/region and payment/integration providers.
- Shared SaaS database + strong tenant defenses vs dedicated enterprise databases.
- Identity provider and notification provider; React/Laravel are selected; native mobile is deferred.
- Precise grading, repeat, transfer, financing and thesis policies for the reference university.
- Availability, data-retention and regulatory commitments.

### Discovery questions (do not block architecture)
1. Which two real university handbooks/programs will define acceptance fixtures (one UG, one PG/research)?
2. Are applications tied to intake or independently to multiple program preferences? What are the ranked-choice rules?
3. Which admissions decisions may be conditionally accepted, and what proof clears conditions?
4. Should a student register in several careers concurrently; who authorizes cross-career credits?
5. Does research coursework coexist with milestone obligations? What are required approvals?
6. Which external identity/LMS/payment tools are mandatory for first pilot?
7. What is the university's own exact meaning of credit hours, GPA, tuition hold and graduation approval?
8. What does a public vs internal vs official transcript legally require in the launch jurisdiction?

## 17. Research and standards references

- **S1 — Oracle, PeopleSoft Academic Careers:** https://docs.oracle.com/en/applications/peoplesoft/campus-solutions/9.2.038/campus-solutions-application-fundamentals/defining-academic-careers.html
- **S2 — Oracle, Research Candidates / Thesis:** https://docs.oracle.com/en/applications/peoplesoft/campus-solutions/9.2.039/student-records/managing-research-candidates.html ; https://docs.oracle.com/en/applications/peoplesoft/campus-solutions/9.2.039/student-records/managing-candidate-s-thesis.html
- **S3 — 1EdTech Edu-API:** https://www.1edtech.org/standards/edu-api (confirm standard version/maturity at implementation; use for alignment rather than claim certification).
- **S4 — 1EdTech LTI:** https://www.1edtech.org/standards/lti (an LMS/tool integration standard, not a substitute for SIS data APIs).
- **S5 — OWASP API Security Top 10:** https://api-security.owasp.org/editions/2023/en/0x11-t10/
- **S6 — W3C internationalization & WCAG:** https://www.w3.org/International/techniques/authoring-html#direction ; https://www.w3.org/TR/WCAG22/
- **S7 — NIST AI RMF / Generative AI profile:** https://airc.nist.gov/ ; https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf

---

**Document control:** The [business-module companion PRD v0.7](prd/README.md) (2026-10-19) organizes whole-system requirements by business module with one PRD per module and role work derived later and uses the module map as a backend reference in its decision log, requirements, journeys, planned screen/API contracts and acceptance gates. Implementation, design assets, test executions and institutional sign-off remain pending. Future architectural or scope changes must update both documents and trace affected requirement IDs, tests and release evidence. The aligned PRD is the execution contract; this vision expresses the product intent.

**Current delivery workflow:** Analyze/update one business module → Backend + AI + UI/UX → Frontend → QA. Security follows every phase: Backend/AI S1, Frontend/integration S2, QA/results S3. UI/UX has product/design review only, with no security assessment.
