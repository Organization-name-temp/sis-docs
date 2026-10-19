# Localization and accessibility

[PRD index](../README.md) · [System parts](../README.md)

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| I18N-01 | R1 | More than 99 supported language packs across React web | At least 100 identified released packs have translation ownership and critical-path coverage; preference persists through routes, deep links and sessions; T-I18N-01 |
| I18N-02 | R1 | Correct Arabic RTL for all critical web components | Navigation, tables, wizard, dialogs and mixed scripts tested; T-I18N-02 |
| I18N-03 | R1 | Localize university metadata and dynamic form labels | Program/field names render per locale or explicit fallback; T-I18N-03 |
| I18N-04 | R1 | Notifications and templates localized per recipient preference | Email/push/doc template uses approved locale; T-I18N-04 |
| I18N-05 | R1 | Locale-aware dates, names, number and currency presentation | Storage canonical; display follows locale without altering values; T-I18N-05 |
| I18N-06 | R1 | Keyboard/assistive and accessibility coverage | Critical paths checked against WCAG 2.2 AA target; T-I18N-06 |
| I18N-07 | R1 | Language-pack quality gates, script metadata and explicit fallback policy | Missing critical translations block publication; every released pack passes fonts, direction, pluralization, layout and template checks; AR/EN/FR complete reference regressions; T-I18N-07 |

## Acceptance scenarios

**AC-014 | Released-language workflow equality**  
**Given** the same application and registration fixtures  
**When** tested across at least 100 released language packs, with complete Arabic/English/French reference regressions  
**Then** validation, rules and outcomes are identical; Arabic direction/mixed identifiers are correct; translated messages are comprehensible.  
**Covers:** I18N-01–07, ADM-07, REG-05; **Proof:** released-language roster, critical-path matrix, reference E2E and human translation/script checks.

**AC-016 | Locale-independent calculations**  
**Given** the same academic record displayed in AR/EN/FR  
**When** locale changes  
**Then** GPA numeric value, fees and deadline instants are unchanged; only formatting changes.  
**Covers:** I18N-05, REC-02, FIN-01; **Proof:** canonical API comparison.

## Related acceptance coverage

[AC-021](foundations/configuration-workflows.md). Scenario ownership and complete coverage are listed in the [acceptance index](acceptance-index.md).

## Shared contracts and release gates

Apply the shared [domain invariants](architecture.md), [API contracts](api-contracts.md), [screen inventory](screens.md), [quality requirements](quality.md), [traceability](traceability.md) and [release gates](release-plan.md). Requirement IDs, release assignments and T-* families remain unchanged by this organization.
