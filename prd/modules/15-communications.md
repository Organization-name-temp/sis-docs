# 15. Notifications and communications — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Resolve recipients and localized templates; deliver committed-event inbox/email with preference and delivery operations.

**Actors:** Applicants/students, staff communication administrators and operators.

**Business flow:** Committed event → resolve authorized audience/locale/preferences → render approved template → dispatch/retry → inbox/read/delivery history.

**Core invariant:** No foreign-recipient disclosure; preview sends nothing; retries cannot duplicate official state; mandatory-notice exceptions are explicit.

**Policy questions / fixtures to resolve:** Template ownership/placeholders/locales, required versus optional notices, audiences, read/delivery states and channel preferences.

**Screen references:** SC-S08, SC-S04. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns recipient resolution, inbox, email/push, templates, preferences and delivery operations. Messages originate from committed domain events and do not decide academic or financial state.

## Detailed business scope

- In-app inbox
- Email delivery
- Student push
- Recipient resolution
- Template management
- Delivery operations
- Preferences

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| COM-01 | R1 | In-app/email notification from committed domain events | Recipient, locale and event id correct; T-COM-01 |
| COM-02 | R1 | Idempotent notification retries and delivery log | Failure retry cannot duplicate critical status changes; T-COM-02 |
| COM-04 | R1 | Student inbox, recipient resolution and delivery preferences | In-app read state consistent across devices; audiences resolved by tenant/record scope; optional channel preferences respected and mandatory-notice exceptions explained; T-COM-04 |
| COM-05 | R1 | Localized template lifecycle and delivery operations | Preview cannot send real messages; publication validates placeholders/locales; channel/provider failures visible with retry and deduplication; T-COM-05 |

New map-derived requirements are draft extensions; their release assignments are planning proposals pending institutional policy/acceptance review. Existing requirement release commitments are unchanged.

## Map-derived acceptance scenario

**AC-039 | Inbox preferences and safe template publication**  
**Given** a student inbox, optional channel preferences, a mandatory notice and localized draft template  
**When** staff preview/publish the template and dispatch committed domain notices  
**Then** preview sends nothing, missing required placeholders/locales block publication, optional preferences and documented mandatory exceptions apply, recipients remain scoped and retries do not duplicate notice state.  
**Covers:** COM-04/05, COM-01/02; **Release:** R1; **Proof:** template/preferences fixtures, recipient-scope and delivery replay tests.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
