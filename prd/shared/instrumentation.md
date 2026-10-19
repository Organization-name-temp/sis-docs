# Instrumentation and feedback

[PRD index](../README.md) · [System parts](../README.md)

## 15. Product instrumentation and feedback loop

### 15.1 Main event taxonomy

| Event | Core properties (no unnecessary PII) | Why collected |
|---|---|---|
| `application_started` | tenant/institution, form version, career, locale | Conversion baseline |
| `application_step_saved` | step ID, elapsed time, validation-count | Usability drop-off |
| `application_submitted` | application ID (pseudonymous), form version, career | Completion |
| `application_submission_failed` | safe validation/code, step | Investigate friction |
| `offer_accepted` | program, cohort, decision version | Admissions funnel |
| `matriculation_completed` | career, program, idempotency result | Handoff reliability |
| `registration_eligibility_checked` | policy version, structured result codes | Student friction / rule clarity |
| `registration_confirmed` | section ID, program context, request duration | Reliability and load |
| `registration_rejected` | stable reason codes only | Identify policy/UX patterns |
| `degree_audit_run` | curriculum version, runtime, rule count | Correctness/performance |
| `grade_correction_processed` | approval ID, reason category | Academic integrity |
| `research_milestone_updated` | template/version, status | Research workflow |
| `request_submitted` / `request_decided` | request type, elapsed time, actor role | Workflow SLA |
| `webhook_delivery_failed` / `retried` | subscription, event ID, attempt count | Integration reliability |
| `security_access_denied` | reason class, actor scope, endpoint | Security monitoring |
| `ui_locale_fallback` | key + locale (no student content) | Translation completeness |
| `waitlist_ranked` / `seat_offer_expired` | criterion version, reason codes, offer ID | Ranking explanation and capacity health |
| `tuition_calculated` / `payment_reconciled` | policy version, provider reference, discrepancy category | Calculation correctness and reconciliation |
| `workflow_version_published` / `workflow_step_escalated` | version, node, scoped role, timer result | Configuration/execution integrity |
| `ai_plan_evaluated` / `document_extraction_reviewed` | tool/model version, source ID, review status | Grounding and human confirmation |
| `ai_tool_denied` | tool, policy reason, tenant | AI authorization and abuse detection |

### 15.2 Feedback-to-change mechanism

1. Collect error/event, UAT observation, support report or institution-requested policy difference.
2. Categorize as **bug**, **unmet accepted requirement**, **new requirement**, **policy-data/config issue**, **security incident**, or **training/usability issue**.
3. Link to impacted outcome `O-*`, requirement `*`, journey `J-*`, screen `SC-*`, domain invariant `INV-*`, test `T-*` and source fixture; if none exists, write a new requirement proposal.
4. Prioritize by severity, academic integrity, number of affected students, compliance and effort; record approver and decision rationale.
5. Update specs, policy fixture, design, OpenAPI, implementation and tests together; security/tenant-impact review mandatory on sensitive changes.
6. Deploy using version/release flags. Observe telemetry and compare measured outcome to expectation.
7. Accept and close **only when** linked evidence and policy acceptance exist; reopen if regression or live results contradict closure.

**Example closed-loop case:**

```text
Feedback FB-027: Students with concurrent careers see wrong cumulative GPA
  → Impact O-03, O-06
  → Requirements ACA-02, REG-08, REC-02
  → Invariant INV-03, INV-14
  → Tests AC-005, T-ACA-02, T-REC-02
  → Correct query scope + program-switch context
  → Run released-language critical-path checks, AR/EN/FR reference regressions and existing degree-audit fixtures
  → Pilot metric: incorrect-cross-career-context count → 0
  → Registrar+QA acceptance → CLOSED
```

No product issue is closed because “AI says it works.” It closes with checked domain truth and relevant independent evidence.

## Module-map signals

Track configuration/calendar publication rejections, invalid student status transitions, advisor-scope denials, aid-budget reservation conflicts, disbursement acknowledgment lag, scholarship-to-ledger discrepancies, preference/mandatory-notice outcomes and tenant lifecycle/support-access expiry. Events include tenant, object/version, correlation and safe reason codes; avoid applicant evidence, payment secrets or raw student answers in telemetry.
