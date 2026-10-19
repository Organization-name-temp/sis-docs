# Release plan, module workflow gates and approval

[Business module index](../README.md) · [Workflow](../workflow.md) · [Current decisions](governance.md)

## Release scope

| Release | Active delivery |
|---|---|
| R0 | Domain vocabulary, reference institution/policy fixtures, tenancy/audit/contracts, responsive multilingual design foundations |
| R0.5 | AI-01–07: scoped policy explanations, planning, analytics Q&A and reviewed extraction; implementation/evaluation required before R1 |
| R1 | Active institutional/academic/admissions/lifecycle/sections/registration/records/degree/research slice, advanced waitlists, rich tuition/aid/payment/accounting integration scope, full visual workflows, React applicant/student/staff portals, localization, operational/security readiness |
| R2/R3 | Existing deferred depth: full research examination, advanced policy DSL/articulation/credential exchange, cohort warehouses, commercial entitlements and later enterprise/jurisdiction depth as assigned |
| No active release | Native mobile, offline operations/widgets/device push: see deferred record; no reintroduction without a new explicit decision |

Release labels are separate from the per-module phases. New map-derived institutional rules/schedules remain draft proposals requiring stakeholder fixtures/approval. No date, provider scope, compliance certification or implementation evidence is inferred.

## Ready for module work

- [ ] Detailed module purpose, actors, process/states, canonical requirements, dependencies, permission boundaries, exceptions, policy questions and acceptance criteria updated in its module PRD.
- [ ] Reference institution/SMEs identified; grading, credit/repeat, finance/aid, admission, research and calendar fixture ownership agreed.
- [ ] Domain/API/event/error contracts, proposed UI/UX content/states and more-than-99-language roster/translation ownership identified.
- [ ] R1 provider/operation matrix, tuition/scholarship budget/recovery rules, waitlist criteria and workflow approvers agreed before dependent behavior is marked APPROVED.

## Required phase handoffs

| Phase | Work | Security after phase | Entry to next phase |
|---|---|---|---|
| 1 | Backend + AI + UI/UX | S1 assesses Backend/AI contracts, data/tools, validation, tenant scope and implementation/eval evidence; UI/UX is not security-assessed | Backend/AI S1 disposition plus product/design handoff |
| 2 | Frontend | S2 assesses implemented React/session/data handling and Backend/AI integration changes | Reviewed implementation and reproducible module build/context |
| 3 | QA | S3 assesses QA/security/eval results, regressions and remaining findings | Module acceptance/closure only when evidence and applicable risk decisions pass |

UI/UX requires editable designs, content/permission annotations, responsiveness, accessibility and product review, with no UI/UX security assessment. QA plans fixtures/cases early; integrated QA execution follows Frontend/S2. AI's core work is Phase 1, with support/fixes during integration/QA under their phase reviews. No S0, per-role design gate, native-app gate or independent AI security gate is required.

## R0.5 acceptance

- [ ] T-AI-01–07 and AC-019/030–032 pass with authorized sources/tools, deterministic plan validation, scoped analytics, reviewed extraction, injection/PII negatives and confirmation tests.
- [ ] Backend/AI S1 and subsequent implemented integration/QA S2/S3 reviews are recorded for the delivered AI flows.
- [ ] Institution activation controls work; a disabled flag does not waive the required AI delivery/evaluation.

## R1 exit — active system acceptance

- [ ] All active R0.5/R1 requirements have passing linked T-* executions, approved policy fixtures and module phase/security evidence, or an explicit scope revision.
- [ ] Active AC-001–042 pass where assigned to R1; AC-022/028/029 are deferred and excluded, while AC-042 remains R2. See the acceptance index for precise ownership/scope.
- [ ] UG/taught-PG/research journeys and dissertation/project versus candidature distinction pass institutional fixtures/UAT at their assigned depth.
- [ ] GPA/degree-audit correctness, grade/award approvals, immutable history and version reproduction match signed reference cases.
- [ ] Tenant/custom-role/student/TA/non-teaching leader access, self-approval/grade denial, downloads/exports and background boundaries pass negative cases.
- [ ] Seat/waitlist/offer/budget races and submission/matriculation/payment/refund replay preserve capacity, uniqueness and ledger correctness.
- [ ] Rich tuition/aid, payment/accounting provider matrix, disbursement acknowledgment, hold/reconciliation and approved report definitions agree with source/ledger.
- [ ] Full visual workflow builder passes validation, simulation, publication/version retention, scoped approval and escalation cases.
- [ ] At least 100 released language packs pass critical-path checks; AR/EN/FR reference journeys, RTL/mixed-script, browser responsiveness and accessibility reviewed.
- [ ] Actual React/API state agrees; network errors, stale views, conflicts and confirmations are honest and recoverable.
- [ ] Security S1/S2/S3 for applicable module work has reviewed findings; no UI/UX security approval is required.
- [ ] Restore, queue/provider outage/replay, migration recovery, alerts/traces, operational runbooks and agreed performance targets have direct execution evidence.
- [ ] No unresolved P0/P1; applicable P2 waivers identify owner/rationale/expiry. Institution acceptance and deployment authorization are separately recorded.

## Severity

- P0 stop launch: cross-tenant disclosure, unauthorized official grade/award, corruption/oversell/lost record or broken restore.
- P1 stop launch: blocked critical journey, lost/duplicate matriculation, wrong approved GPA/audit/finance results or blocking localization/accessibility defect.
- P2 requires applicable explicit waiver: recoverable/non-critical issue with owner, rationale and expiry; assess accessibility impact.
- P3 backlog: low-impact change with no correctness/security consequence.

## Open decisions and approval

Policy/SME sign-off, exact integrations, grading/rounding/transfer rules, research milestone/examination depth, scholarship budget/eligibility/renewal/recovery, required notices, report definitions, jurisdiction/retention/hosting, support access and commercial entitlement details remain unresolved where fixtures are absent. Schema abstractions may be developed; policy-dependent outputs cannot be accepted until approved fixtures exist.

All phase artifacts, executions, security dispositions, institutional acceptance and rollout authorization remain PENDING unless directly evidenced. Mobile has no active approval item or release blocker.
