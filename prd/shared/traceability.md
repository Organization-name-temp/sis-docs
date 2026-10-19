# Traceability and closure

[PRD index](../README.md) · [System parts](../README.md)

## 11. Traceability matrix (objectives → features → evidence)

This is the primary *macro-level* closure matrix. Each row below refers to requirement IDs listed in §4; every requirement in §4 in turn has a one-to-one named `T-*` test family. For detailed implementation traceability, add Figma screen URL, endpoint/symbol, issue/PR, test execution ID, build SHA and owner to the per-requirement registry.

| Outcome | Core journeys | Requirement IDs | Test families / acceptance | Production signal |
|---|---|---|---|---|
| O-01 Applicants complete applications | J-ADM, J-MAT | ADM-01–12, IAM-01 | T-ADM-*, AC-001–004, AC-021 | `application_started`, `step_saved`, `application_submitted`, abandonment |
| O-02 Correct registration | J-REG/J-WAIT | REG-01–09, REG-11, ACA-07, FIN-03 | T-REG-*, AC-007–009, AC-018/025 | `registration_confirmed`, `eligibility_denied_by_reason`, overcapacity alarms |
| O-03 Correct records / degree progress | J-ACA, J-REC, J-RES | ACA-01–10, REC-01–07, RES-01–06, PGT-01 | T-ACA-*, T-REC-*, T-RES-*, T-PGT-01, AC-005–006, AC-010–013/033 | audit-evaluation mismatch reports, grade correction rates |
| O-04 Controlled access | All journeys | TEN-01–05, IAM-01–05/07–09, RPT-02 | T-TEN-*, T-IAM-*, AC-015/023/024 | auth denials, audit anomalies (no raw PII) |
| O-05 More-than-99-language parity | All critical journeys | I18N-01–07, COM-01 | T-I18N-*, AC-014, AC-016 | locale fallback coverage, localized error reports |
| O-06 Student self-service adoption | J-UG/J-PGT and student web | WEB-01–03, WFL-01–04, PGT-01 | T-WEB-*, T-PGT-01, AC-033 | task completion, errors, request time |
| O-07 Safe configuration | J-ADM, J-ACA, J-RES, J-WFL | ADM-02–05, ACA-01/05/08, WFL-01–04, IAM-07 | T-ADM-*, T-ACA-*, T-WFL-*, T-IAM-07, AC-001, AC-006, AC-021/027 | published version failures, unsupported configuration requests |
| O-08 Operability | All journeys | INT-01–04, OPS-01–05 | T-INT-*, T-OPS-*, AC-017/020 | queue depth, delivery failures, restore test age |
| O-09 Responsible AI | J-AI; required pre-R1, controlled activation | AI-01–07, IAM-02 | T-AI-*, AC-019/030–032 | safety fail rate, tool authorization denials, cited answer coverage |
| O-03/O-06 Trustworthy student finance | J-FIN | FIN-01–07 | T-FIN-*, AC-018/026 | calculation mismatch and reconciliation discrepancy counts |

### 11.1 Minimum requirement registry record

```yaml
id: REG-04
title: Atomic idempotent registration
objective_ids: [O-02]
journey_ids: [J-REG]
release: R1
status: APPROVED # only after sign-off
owner: backend-lead
reviewers: [registrar-sme, qa-lead, security-lead]
assumptions: []
policy_fixture: university-a/registration/fall-2027.yaml
screen_ids: [SC-S02]
api_contracts: [POST /api/v1/registrations]
code_references: []       # fill after implementation
test_ids: [T-REG-04, AC-007, AC-017]
build_sha: null            # fill after deployment
execution_evidence_urls: []
telemetry: [registration_confirmed, section_capacity_violation]
acceptance_decision: PENDING
accepted_by: null
accepted_at: null
follow_up_issues: []
```

**Important:** `APPROVED` in this sample means a hypothetical future approved requirement record, not that the present draft is approved. The actual baseline remains `DRAFT` until stakeholder sign-off.

### 11.2 Closure states and allowed transitions

```text
DRAFT → DISCOVERY_COMPLETE → APPROVED → DESIGNED → IMPLEMENTED
       → VERIFIED → PILOT_MEASURED → ACCEPTED / CLOSED
                           ↘ FAILED → REWORK → VERIFIED
                       PILOT_MEASURED → CHANGE_REQUEST → IMPACT_REVIEW
                                                   → APPROVED (new revision)
```

**Exit criteria for each stage:**

1. **DRAFT → DISCOVERY_COMPLETE:** owner, actor, expected business outcome, open policy questions and referenced evidence known.
2. **→ APPROVED:** institutional SME/product owner signs requirement wording, rule fixtures, priority, exclusions and acceptance criteria.
3. **→ DESIGNED:** design link or explicit API-only justification, permission mapping, released-language coverage with AR/EN/FR references and failure states, and data-change impact reviewed.
4. **→ IMPLEMENTED:** PR merged to tested branch, migrations documented, API schema updated, feature flag/config stated.
5. **→ VERIFIED:** all requirement-linked positive/negative/tenant/security/locale tests pass in agreed target environment; evidence links and build SHA recorded.
6. **→ PILOT_MEASURED:** instrumentation validated and observed tasks/errors compared with target metrics; stakeholders see exceptions.
7. **→ ACCEPTED / CLOSED:** named approver signs off; unresolved critical defects zero, waived medium/low risks have owner, rationale and due date; operational/handoff artifacts complete.
8. **Regressions/feedback:** defects link back to requirement and failing scenario; change must update policy fixture, UI, API, relevant tests, release note and decision log before re-closing.

A merged PR, passing superficial UI test or launch announcement **does not** alone mean closed.

## Module-map extension traceability

| Outcome | New requirements | Planned journey / screen | Test families / evidence |
|---|---|---|---|
| O-03/O-07 Historical configuration correctness | INS-01, CFG-01, CAL-01, CUR-01, CAT-01 | Publish/preview/version comparison; SC-O11/12 | Matching T-INS/CFG/CAL/CUR/CAT families; effective-date/version fixtures |
| O-04/O-06 Student lifecycle | STU-01/02, SEC-01 | Matriculation/status/advisor/section assignment; SC-O13/O04 | Matching T-STU/SEC families; transition, scope and conflict fixtures |
| O-03/O-06 Aid operations | SCH-01–06 | Application→review→budget commitment→finance acknowledgment→renewal/suspension; SC-O14/S07 | T-SCH-01–06 plus T-FIN-06; budget races and finance replay reconciliation |
| O-05/O-06 Communications | COM-04/05 | Inbox/preferences/template lifecycle; SC-S08 | T-COM-04/05; required-notice, locale, privacy and retry cases |
| O-04/O-07 Reporting | RPT-04/05 | Approved definitions and aid/reconciliation analytics; SC-O15 | T-RPT-04/05; source/ledger agreement and export negatives |
| O-04/O-08 SaaS lifecycle | SAS-01/02 | Provision→activate/suspend/archive; SC-O16 | T-SAS-01/02; entitlements, support expiry and retained-record checks |

Per-requirement ownership is in the [module-map registry](module-map-alignment.md). Existing AC-001–033 are retained; AC-034–042 provide the new acceptance design; new requirements cannot be called VERIFIED until their T-* families, approved policy fixtures, implementation/design references and passing executions are linked. No evidence is supplied by the map itself.

## Current phase traceability

Each module has one canonical PRD. Backend, AI, UI/UX, Frontend, QA and Security work will be derived from it later. Analyze/update the module first; assign Phase 1 Backend+AI+UI/UX, Phase 2 Frontend, Phase 3 QA. Record S1 Backend/AI, S2 Frontend/integration and S3 QA evidence/finding dispositions. UI/UX uses a product/design handoff with no security assessment.

Mobile requirements/device push and AC-022/028/029 are deferred with no current release gate. Preserve their IDs in the deferred record; do not count them as current coverage gaps or claim native implementation. All active canonical requirements need matching T-* execution evidence, approved fixtures and module phase handoffs before VERIFIED/ACCEPTED.
