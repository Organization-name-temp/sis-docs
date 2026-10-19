# Change control and evidence templates

[PRD index](../README.md) · [System parts](../README.md)

## 17. Change-control and release evidence templates

### 17.1 Decision log template

```markdown
| Decision ID | Date | Status | Choice | Alternatives | Rationale |
| D-021 | YYYY-MM-DD | PROPOSED | ... | ... | ... |

Affected: requirements [IDs], screens [IDs], APIs [paths], fixtures [IDs],
tests [IDs], docs [paths].
Approver: ...  Next review date: ...
```

### 17.2 Change request template

```yaml
change_id: CR-001
origin: [stakeholder, support, telemetry, UAT, security]
problem: ""
affected_outcomes: []
affected_requirements: []
impact: [data, API, UI, permissions, i18n, research, integrations, NFR]
alternatives: []
decision: PENDING
risk_severity: P2
owner: null
reviewers: []
changed_tests: []
release: TBD
rollback_plan: null
verification_evidence: []
measured_after_release: null
closure: PENDING
```

### 17.3 Release evidence folder suggestion

```text
docs/releases/R1/
  scope-baseline.md
  open-decisions.md
  requirement-traceability.csv
  security-review.md
  concurrency-report.md
  localization-matrix.md
  academic-golden-cases.md
  load-and-performance.md
  backup-restore-evidence.md
  UAT-signoff.md
  known-limitations.md
  final-go-no-go.md
```
