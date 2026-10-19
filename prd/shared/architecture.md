# Domain model, invariants and architecture

[PRD index](../README.md) · [System parts](../README.md)

## 7. Data/domain model and invariants

### 7.1 Essential entities

```text
PlatformTenant ── Institution ── Campus / AcademicUnit
      │                ├─ AcademicCareer ── Program ── ProgramVersion
      │                │                       └─ CurriculumVersion
      │                │                            └─ RequirementSet / RuleVersion
      │                ├─ AcademicYear / Term / Session
      │                └─ Course ── CourseVersion ── Section ── SectionMeeting
      │
      ├─ Person ── ApplicantProfile ── Application ── FormVersion / SubmissionRevision
      │        ├─ AcademicMembership / Qualification / Rank
      │        ├─ Appointment (TA / Dean / DepartmentHead / Other)
      │        ├─ TeachingAssignment (Section / Responsibilities / EffectiveDates)
      │        └─ StudentProfile ── StudentProgramEnrollment
      │                                 ├─ Registration ── GradeHistory
      │                                 ├─ AppliedCurriculumVersion
      │                                 ├─ DegreeAuditEvaluation
      │                                 ├─ TaughtPGProject / Advisor / Submission / Assessment
      │                                 ├─ ResearchCandidature? ── Supervisors/Milestones/Thesis
      │                                 ├─ StudentAccount ── LedgerEntry / TuitionCalculation
      │                                 │                  └─ ScholarshipAward / Installment / Payment / Refund
      │                                 └─ Hold / Request / ApprovalHistory
      │
      ├─ User ── Membership / TenantConfiguredRole / Permission / ScopedGrant
      ├─ WaitlistEntry / PriorityPolicyVersion / SeatOffer / OptimizationTrace
      ├─ FormDefinition / PublishedFormVersion / LocalizedFieldSchema
      ├─ WorkflowDefinition / WorkflowVersion / Instance / Action
      ├─ TemplateTranslation / NotificationLog
      ├─ OutboxEvent / WebhookSubscription / DeliveryAttempt
      └─ AuditEvent / PolicyEvaluationTrace
```

This is **logical modeling only**; physical keys, indexing and normalization will be decided in ERD/ADR review.

### 7.2 Critical invariants (INV)

| ID | Invariant | Enforcement location / evidence |
|---|---|---|
| INV-01 | An application binds to one immutable published form version | DB foreign key + submission snapshot + T-ADM-05 |
| INV-02 | Rejected or draft applicant is not automatically a matriculated student | Domain state transition + T-ADM-11 |
| INV-03 | A person may have many applications and program enrollments; institutional student IDs may differ | Unique constraints scoped correctly + T-ACA-02 |
| INV-04 | Each program enrollment has a known effective curriculum/rule reference | FK + validation + T-ACA-05 |
| INV-05 | Active registrations in a section never exceed effective capacity | Transactional allocation + stress T-REG-04 |
| INV-06 | Enrollment cannot be created by bypassing eligibility engine | Domain/service-level authorization/validation + T-REG-03–05 |
| INV-07 | Published grades are not silently overwritten | Append-only corrections + approval + T-REC-03 |
| INV-08 | Degree audit identifies curriculum version and evidence inputs | Snapshot/report + T-REC-05 |
| INV-09 | Award publication requires authorized explicit approval | Authorization + event + T-REC-07 |
| INV-10 | Every protected record read/write is institution/tenant/role/object scoped | RLS/application policy + T-TEN-01 / T-IAM-02 |
| INV-11 | Domain transaction and its outbox events succeed or fail atomically | DB transaction + T-INT-02 |
| INV-12 | Student balance derives from ledger transactions, not manual single field edits | Ledger constraint + T-FIN-01 |
| INV-13 | A research milestone or thesis belongs to an authorized candidature | FK + policies + T-RES-04/06 |
| INV-14 | Locale affects presentation, not canonical academic calculations | Language-pack matrix + T-I18N-05/07 |
| INV-15 | AI answers cannot commit unauthorized mutations or create new policy | Tool gateway + T-AI-02/03 |
| INV-16 | Titles/qualifications/appointments never implicitly confer teaching or grade rights | T-IAM-07–09; AC-023/024 |
| INV-18 | Payment/refund replay cannot duplicate ledger entries or corrupt holds | Verified provider identity + T-FIN-05/07; AC-026 |
| INV-19 | Workflow instances, waitlist rank and tuition decisions retain their applied policy versions | T-WFL-04, T-REG-09/11, T-FIN-02 |

### 7.3 Effective dating, identities, histories

- Use immutable published version IDs plus effective dates for policies, academic catalogs, curriculum, form schemas and applicable financial rules.
- Do not assume `student.status` captures entire academic state. Track application, person identity, student profile, career membership, program enrollment, candidature and term registration states independently.
- Do not assume a student has only one career, one degree, one advisor or one university affiliation.
- Do not merge Person records based solely on similar full names or emails; implement review for uncertain duplicates.
- Store UTC instants with institution-specific timezone settings; deadlines interpreted in institution local time with explicit offset/zone in API.
- Store official numeric values with fixed decimal precision. Display rounding and GPA rounding rules are policy-bound.
- Retention/deletion must respect institutional academic-record obligations and jurisdictional policy; do not promise blanket physical deletion of all education records.

### 7.4 Translation strategy

- Maintain an explicit roster of at least 100 released language packs with owner, script/direction, fonts, quality state and test evidence; no hard-coded three-language limit.
- UI messages: locale keys and tested pluralization/gender-neutral formulations where possible.
- Institution-controlled content: `translations` relation keyed by entity/version/field/locale or equivalent normalized localization store.
- Canonical identifiers and enum codes are language-independent; avoid `*_en/*_ar/*_fr` duplication as the primary model.
- Published critical workflows require complete locale coverage for essential field labels, options, help/error text and policy explanations or an explicitly approved documented fallback.

---

## 14. Technical architecture and implementation guardrails

### 14.1 Domain architecture

**Recommended Laravel bounded contexts:**

```text
Platform / Tenancy / Identity
Admissions / PersonRegistry / StudentLifecycle
Institutions / AcademicCareers / Programs / Curriculum
AcademicRules / Sections / Registration
AcademicRecords / DegreeAudit / GraduateResearch
StudentFinance / Holds / Requests / Workflows
Documents / Communications / Integrations
Reporting / Audit / AI (Phase 1 delivery, institution-controlled activation)
```

Use clean explicit domain services/actions with typed inputs; controllers thin; policies enforced in services (not just controllers); Eloquent model relations do not become a backdoor to bypass authorization; cross-context collaboration by application service/domain event with transaction boundaries.

### 14.2 Storage/infra decisions

| Component | Proposed option | Decision gate |
|---|---|---|
| Primary relational DB | PostgreSQL | Tenant-aware constraints, RLS model and performance evidence |
| Cache & queue broker | Redis | Clear invalidation; queues do not own academic truth |
| Object storage | S3-compatible | Signed URLs + access logs + encryption + malware policy |
| Auth | Laravel native/session + identity integration, or delegated IdP | Threat model / client auth ADR; one consistent identity owner |
| Job processing | Laravel Queue / Horizon | Reliable retry, dead-letter and idempotency |
| Observability | OpenTelemetry-compatible traces, structured logs, metrics | PII-safe design and correlation |
| Web | React | Selected; responsive applicant/student/staff clients; Laravel owns rules |
| CDN/deployment | Environment-specific | Hosting/data residency/privacy assessment |

**Database design warning:** database RLS alone cannot implement every advisor/grade/assignment scope; enforce domain authorization in Laravel. Conversely, Laravel alone should not be the single safeguard against accidentally omitted tenant filters.

**Workflow design warning:** R1 full visual builder uses bounded typed nodes, validated conditions, scoped approver resolution, timers/escalation, immutable published versions and simulation. Never execute arbitrary tenant-uploaded PHP/JS or raw SQL to implement policies.

**Operational warning:** do not send notification/webhook before a grade, registration or matriculation commit succeeds. Use transactional outbox and idempotent retries.

### 14.3 AI safety and tool boundary (required pre-R1 delivery)

```text
Authenticated user → AI Gateway → Tenant/role/object authorization
      → approved tool calls → Laravel domain APIs (read-only first)
      → grounded answer / validated plan / defined metric / extracted draft fields
      → explicit human confirmation + domain validation for any authorized write
```

- The only authoritative source of student eligibility, earned credits and financial hold is the deterministic SIS.
- No direct unrestricted SQL; no tool that accepts arbitrary student ID without server authorization.
- No training on institutional data by default; vendor retention/privacy must be explicitly reviewed before enabling AI.
- Prompts and external content treated as untrusted inputs. Test indirect prompt injection from uploaded documents and notes.
- No automatic decision for admissions rejection, academic standing/discipline, award, student finance or transcript alterations.
- AI answer identifies when no verified policy or enough evidence exists and routes to human advisor/registrar rather than hallucinating.
- Implement eval set and usage/cost quotas before opening tenant-wide access. Governance informed by NIST AI RMF [REF-07].

## Module-map ownership extensions

Logical entities added for review: InstitutionProfileVersion / UnitResponsibility / ConfigurationEntryVersion / PublishedCalendarVersion / StudentStatusTransition / AdvisorAssignment / ScholarshipProgramVersion / AidApplication / FundingAllocation / AwardCommitment / DisbursementInstruction / TenantLifecycleEvent / PlanVersion / FeatureEntitlement.

- Scholarship commitments reserve a defined approved budget atomically; cancellations/reversals preserve history. A disbursement instruction references the approved award; Finance owns the idempotent ledger credit and acknowledgment.
- Student status, account/session status, program enrollment, academic appointment and tenant lifecycle remain independent. Suspending a tenant cannot delete official records or erase financial history.
- Report definitions name their source, formula, effective version and row/column permissions. Report and AI projections never become independent academic/financial authorities.
- Numbered area files describe domain ownership, not a requirement for separate deployed microservices. The Laravel modular-monolith decision remains.

## Current client boundary

React responsive web is the active client. Native mobile/offline/widget/device-push entities, sync endpoints and device-specific gates are deferred rather than replaced by new web-offline requirements. Backend/AI work starts with UI/UX in Phase 1; frontend is Phase 2 and QA Phase 3. UI/UX is not security-assessed.
