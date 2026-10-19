# Quality requirements and verification plan

[PRD index](../README.md) · [System parts](../README.md)

## 9. Localization, accessibility, security, reliability and performance

### 9.1 Localization acceptance

| Requirement area | Standard / acceptance |
|---|---|
| Language support | More than 99 languages (at least 100 released packs) on applicant/student/staff React web; full AR/EN/FR reference regression and critical-path evidence for every released pack |
| Script direction | Set document/element `dir` correctly; properly isolate mixed-language snippets, student codes, numbers and URLs [REF-05] |
| Institution content | Core program/course names, admission labels, messages and official-use templates available per approved locale |
| Date/time | UTC storage + institution time zone; localized display; deadlines never shift with phone locale |
| Numeric grading | Locale changes cannot alter persisted decimals, arithmetic, credit values or ordering |
| Documents | Use localized templates with controlled institutional approvals; PDFs/RTL tested before classifying a document as supported |
| Accessibility | **Target WCAG 2.2 AA** web critical journeys;  no claim of certification [REF-06] |

### 9.2 Security/privacy NFRs

- Risk model references OWASP API Security Top 10 including **Broken Object Level Authorization**, broken authentication, mass assignment/property authorization, resource exhaustion and function-level authorization [REF-04].
- Authentication: token/session expiration and revocation, brute-force protection, MFA capability for privileged accounts (R1 preferred; mandatory policy for real pilot), secrets rotation and secure cookies/headers.
- Authorize *each object and action*: student ID, application ID, section ID, document key, approval task, download, export and webhook scope.
- Tenant separation: composite tenant-aware foreign keys where appropriate; transaction-scoped tenant context; DB row-level security as additional defense if used; central policy tests; queue workers/jobs must fail closed if tenant context absent.
- Document handling: private storage, signed URLs with expiry, MIME/extension/size validation, scan as supported, safe previews, explicit retention and download audit for sensitive categories.
- Data protection: encrypt in transit and at rest; separate application audit, security event logs and restricted PII; no sensitive credentials in traces; pseudonymize test data.
- Staff exceptions and impersonation are not routine privileges. Any break-glass support must have reason, approval, expiry and explicit log.
- Privacy/legal policy requires jurisdiction-specific assessment before pilots. Supporting French text does not automatically equal GDPR readiness; supporting Arabic does not automatically satisfy all Egyptian or Gulf legal obligations.

### 9.3 Performance and availability **proposed test targets**

These are **engineering acceptance hypotheses**, subject to measurement and acceptance with a reference deployment; they are not historical metrics or customer SLAs.

| NFR ID | Proposed R1 target | How to verify |
|---|---|---|
| NFR-01 | p95 API read endpoint latency ≤600 ms under defined normal pilot load | Load test with reference dataset/trace, excluding external provider latency |
| NFR-02 | p95 registration submit ≤2 s with 100 concurrent active transactions under reference data | Load/concurrency suite, queue and DB metrics |
| NFR-03 | No seat oversell across 1,000+ scripted final-seat races | Parallel test fixture + DB reconciliation |
| NFR-04 | Main student dashboard render ≤3 s p75 on a stated browser/network profile | Lighthouse/device lab, record device/network |
| NFR-05 | Outbound webhooks have retry and dead-letter visibility; no lost committed events | Induced provider outage and replay |
| NFR-06 | Backup restore succeeds from encrypted snapshot with data reconciled | Restore drill on separate environment |
| NFR-07 | R1 recovery target (proposal): RPO ≤24h / RTO ≤8h on pilot topology | Controlled recovery drill; must be approved before live deployment |
| NFR-08 | Future commercial availability target: 99.9% monthly, excluding agreed maintenance | R2+ commercial SLO with monitoring and exclusions |
| NFR-10 | At least 100 released language packs pass critical-path checks; full AR/EN/FR reference regressions, LTR/RTL and keyboard accessibility | Versioned language roster, translation review, automated checks and human script/assistive QA |

**Reference load fixture (proposal):** 10,000 enrolled students in one tenant; 100 simultaneously registering users; 300 catalog courses; 1,000 sections; 40,000 enrollment-history entries; realistic indexing/pagination. Adjust based on actual institutional pilot and repeat tests after schema changes.

### 9.4 Maintainability and operability

- API contracts OpenAPI versioned; backward-compatibility policy, error taxonomy and deprecation guidance.
- Observability: structured logs, metrics and distributed trace/correlation across Laravel jobs and API; PII-safe defaults.
- Provisioned preview/staging/production environments; reproducible migrations and seed fixtures.
- Database constraints required for tenant uniqueness, section capacity consistency and domain state validity where possible.
- Recovery: migration rollback path, snapshot retention and documented incident handling.
- CI: lint/static checks, functional API/E2E, permission negative tests, transaction/retry and regression fixtures. Unit/integration tests are not excluded from this project; testing mix based on reliability needs.

---

## 13. Verification plan by test layer

| Layer | Required focus | Example tools (non-binding) | Ownership |
|---|---|---|---|
| Domain tests | Academic rules, GPA, form condition evaluation, program transitions | PHPUnit/Pest | Backend |
| API/contract | Authorization, validation, errors, idempotency, OpenAPI contract | Laravel tests, schema validator | Backend/QA |
| DB integrity / concurrency | Atomic seats, unique idempotency, migrations, ledger integrity | PostgreSQL integration harness | Backend/QA |
| Browser E2E | Application wizard, registrar tasks, localization, admission→graduation | Playwright | QA/web |
| Security | OWASP API risks, object-level access, mass assignment, uploads, tenant bypass | ZAP + manual authorization matrix | Security/QA |
| Performance | Registration bursts, normal-load latency, degraded integrations | k6/Locust or equivalent | Backend/DevOps |
| Accessibility/i18n | Keyboard, screen reader, layout, dynamic forms, Arabic mixed text | axe + human testing | Design/QA |
| Recovery/chaos | Storage errors, queue retries, restore, provider outage | Infrastructure test scripts | DevOps |
| Required pre-R1 AI eval | Hallucinated policies, prompt injection, PII leak, source grounding | Versioned prompt fixture corpus | AI/security/QA |

**Fixture discipline:** For each academic case, specify input student history, applied curriculum/policy version, expected eligibility and expected degree audit output **signed by a registrar/domain expert**. Failing fixtures must not be loosened just to accommodate current software behavior.

**API testing must include client-side bypass.** A UI hidden button is not proof of backend authorization. Every relevant object-ID/role/tenant combination must be assessed.

## Current delivery scope

Use the module Phase 1 Backend+AI+UI/UX → Phase 2 Frontend → Phase 3 QA workflow. Security S1/S2/S3 follows phases, with no UI/UX security assessment. Native mobile/offline/widgets/device-push quality targets are deferred; responsive browser and network-error recovery remain active.
