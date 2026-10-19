# API contracts and domain events

[PRD index](../README.md) · [System parts](../README.md)

## 8. API contracts and domain event definitions

### 8.1 API organization (illustrative, not final URLs)

```http
POST   /api/v1/applications
GET    /api/v1/applications/{applicationId}
PATCH  /api/v1/applications/{applicationId}/draft
POST   /api/v1/applications/{applicationId}/submit
POST   /api/v1/applications/{applicationId}/decision
POST   /api/v1/applications/{applicationId}/accept-offer
POST   /api/v1/applications/{applicationId}/matriculate

GET    /api/v1/students/me/program-enrollments
GET    /api/v1/students/me/degree-audits/{programEnrollmentId}
GET    /api/v1/terms/{termId}/sections
POST   /api/v1/registration/eligibility-check
POST   /api/v1/registrations
POST   /api/v1/registrations/{registrationId}/drop

POST   /api/v1/sections/{sectionId}/grades/submit
POST   /api/v1/grade-publications/{id}/approve
GET    /api/v1/students/me/academic-records
GET    /api/v1/students/me/research-candidatures
POST   /api/v1/requests
GET    /api/v1/students/me/financial-summary
POST   /api/v1/roles
POST   /api/v1/scoped-role-assignments
POST   /api/v1/academic-appointments
POST   /api/v1/teaching-assignments
POST   /api/v1/sections/{sectionId}/waitlist
POST   /api/v1/seat-offers/{offerId}/accept
POST   /api/v1/tuition/quotes
POST   /api/v1/scholarship-awards
POST   /api/v1/payment-providers/{providerId}/callbacks
GET    /api/v1/finance/reconciliation
POST   /api/v1/workflow-versions/{id}/validate
POST   /api/v1/workflow-versions/{id}/simulate
POST   /api/v1/workflow-versions/{id}/publish
POST   /api/v1/taught-pg-projects/{id}/submit
POST   /api/v1/ai/plans
POST   /api/v1/ai/analytics-questions
POST   /api/v1/ai/document-extractions
POST   /api/v1/document-extractions/{id}/review
```

**Contract additions:** Payment callbacks use provider verification rather than user sessions. AI proposal/extraction endpoints never silently commit official records; reviewed writes follow normal authorization and domain validation.

**Required envelope concepts:** stable error code, correlation ID, locale-agnostic machine reason(s), safe human message key, structured validation issues, and HTTP status; avoid exposing raw policies or other students' data.

**Mutating high-risk endpoints:** `Idempotency-Key`, CSRF/token protection as appropriate, server-side rate limits, actor scope, documented transaction and webhook delivery semantics.

**Illustrative eligibility response:**

```json
{
  "eligible": false,
  "studentProgramEnrollmentId": "spe_123",
  "sectionId": "sec_456",
  "reasonCodes": ["PREREQUISITE_NOT_MET", "FINANCIAL_HOLD"],
  "policyVersionIds": ["pol_v17", "pol_v22"],
  "evaluationId": "eval_789",
  "evaluatedAt": "2026-10-08T09:00:00Z"
}
```

The eligibility preview is **non-binding**; atomic final authorization happens on commit.

### 8.2 Event contracts

| Event | Trigger / producer | Consumers |
|---|---|---|
| `ApplicationSubmitted` | Valid application transaction | Admissions queue, notification |
| `ApplicationDecisionRecorded` | Authorized decision committed | Applicant status + communication |
| `StudentMatriculated` | New program enrollment committed | Student provisioning, student portal, integration |
| `CurriculumVersionPublished` | Approved version published | Catalog cache / evaluator invalidation |
| `RegistrationConfirmed` | Section seat committed | Mobile/web, roster sync, notifications |
| `RegistrationDropped` | Valid drop committed | Roster sync, finance reevaluation |
| `GradePublished` | Grades finalized | Transcript, degree audit invalidation |
| `GradeCorrectionApproved` | Corrective action approved | Audit/degree audit recalculation |
| `ResearchMilestoneStatusChanged` | Scoped milestone action | Candidate and supervisors |
| `FinancialHoldChanged` | Hold created/released | Eligibility cache invalidation |
| `ScopedRoleAssignmentChanged` | Authorized role/grant change | Access-cache invalidation, security audit |
| `SeatOfferCreated` / `SeatOfferExpired` | Waitlist allocation/expiry transaction | Notifications, next promotion |
| `TuitionChargePosted` / `ScholarshipAwardApproved` | Authorized financial transaction | Ledger, hold reevaluation, accounting adapter |
| `PaymentReconciled` / `RefundReconciled` | Verified provider/reconciliation transaction | Ledger projections, finance audit |
| `WorkflowVersionPublished` | Validated visual workflow publication | Execution engine, configuration audit |
| `TaughtPGProjectAssessed` | Authorized project assessment | Completion evaluation, student status |
| `AwardApproved` | Registrar approves eligibility outcome | Final record and graduation communication |

**Event minimum schema:** `event_id`, `event_type`, `schema_version`, `tenant_id`, `institution_id`, `aggregate_id`, `occurred_at`, `correlation_id`, `causation_id`, authorized payload; respect data-minimization and avoid full PII snapshots unless the receiver contract needs them.

**Delivery:** at least once + idempotent consumers; retry/backoff/dead-letter; explicit ownership and last-error observability. Never promise exactly-once network delivery.

## Module-map contract additions (planned)

| Contract family | Owner / required semantics |
|---|---|
| Institution/configuration/calendar publication | Areas 02–04; validate, preview, authorize and publish immutable effective versions with audit |
| Student status/advisor assignments | Area 09; transition validation, effective dates, scoped assignments and idempotency |
| Aid applications/awards/budgets | Area 16; evidence/review, human decision and transactional budget reservation |
| Award disbursement handoff | Area 16 emits approved instruction ID and version; Area 15 validates/posts once and returns ledger/reconciliation acknowledgment |
| Communication preferences/templates | Area 17; optional channel settings, required-notice rules, safe preview and versioned publication |
| Tenant lifecycle/entitlements | Area 20; approved transitions, no academic history deletion and server-side feature enforcement |

Proposed events: StudentStatusChanged, AdvisorAssigned, ScholarshipBudgetReserved, AidAwardApproved, DisbursementInstructionIssued, ScholarshipCreditPosted, AwardRenewed/Suspended/Terminated, TenantLifecycleChanged and FeatureEntitlementChanged. Each must follow existing tenant scope, outbox, authorization, schema version, idempotency and after-commit semantics. These names are planned contracts, not claims of running integrations.
