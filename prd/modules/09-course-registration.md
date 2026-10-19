# 09. Course registration and waitlists — Product Requirements Document

[Business module index](../README.md) · [Module workflow](../workflow.md)

## Purpose, users and product journey

**Purpose:** Search sections, explain eligibility, book seats, add/drop and operate prioritized optimized waitlists.

**Actors:** Students, advisors and registrars.

**Business flow:** Choose program/term → preview eligibility → confirm → transactional recheck/seat commit → committed schedule; waitlist rank → expiring offer → atomic acceptance or next promotion.

**Core invariant:** Never oversell or duplicate enrollment; cached availability is advisory; expired/ineligible/network-interrupted actions cannot claim a confirmed seat.

**Policy questions / fixtures to resolve:** Institutional load/repeat/hold/override policies, waitlist criteria/ties/optimization, offer expiry and add/drop consequences.

**Screen references:** SC-S02, SC-O10. Screen/design artifacts and institutional fixture approval remain pending; listed IDs are planned traceability.

**PRD status:** Draft product requirements based on the product vision, existing requirements and backend module map. No institutional policy answers, designs, implementation or execution evidence are assumed. Resolve applicable policy questions and refine the product requirements before deriving implementation/design/QA work.

## Ownership and boundaries

Owns registration/search, add/drop, overrides, seat booking and advanced waitlists/optimization (REG-09/11 remain R1 even though omitted from the map). Uses published section capacity and authoritative eligibility at commit.

## Detailed business scope

- Section search
- Eligibility checks
- Seat allocation
- Duplicate protection
- Add and drop
- Registration exceptions
- Schedule and history
- External roster projection

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| REG-02 | R1 | Timetable collision check for same student | Overlapping applicable sections rejected or explicitly overridden; T-REG-02 |
| REG-03 | R1 | Determine student registration eligibility | Return allow/deny, all relevant reason codes and human message key; T-REG-03 |
| REG-04 | R1 | Atomic, idempotent seat booking within capacity | Two users racing for final seat cannot both enroll; T-REG-04 |
| REG-05 | R1 | Enforce window, holds, max/min credits, prerequisite and repeat policies | Every blocked scenario yields stable code and no enrollment; T-REG-05 |
| REG-06 | R1 | Add/drop with deadline/status and academic record effects | Late drops not treated as early drops; fee effects delegated appropriately; T-REG-06 |
| REG-07 | R1 | Authorized overrides with reason, duration and context | Applied only to named rule/student/section; T-REG-07 |
| REG-08 | R1 | Registration history and current schedule across careers | Student can switch active program without mixing grades; T-REG-08 |
| REG-09 | R1 | Advanced waitlist prioritization, expiry and transactional seat promotion | Versioned criteria and tie-breaks produce explained order; concurrent offers, acceptance and expiry never oversell; overrides audited; T-REG-09 |
| REG-11 | R1 | Waitlist optimization using approved constraints and objectives | Ineligible candidates excluded, hard constraints respected, tie-breaks reproducible and recommendations/overrides explained; T-REG-11 |

## Journeys and behavior

### 5.4 Course registration (J-REG)

**Preconditions:** Student is matriculated, has active eligible program/career, valid registration window, program term, acceptable academic/financial standing and any required advisor permissions.

**Check flow:**
1. Choose career/program and term; show sections and available seats.
2. Student selects candidate sections; view calendar and eligibility warnings.
3. API returns `eligible | denied | conditional`, list of stable rule codes, localized message keys, policy/rule versions, and optional approved override path.
4. On confirmed submit, API **re-evaluates** all rules inside appropriate transaction/locking boundaries, reserves seats atomically and commits registrations.
5. Only after commit, emit events for notifications and integrations.
6. For denied bookings, return a safe explanation; no seat is reserved, and state remains consistent.

**R1 modeled rules:**
- Section is published, belongs to allowed career/program, registration window open.
- Capacity must not be exceeded; pending other students' requests must not double count.
- Prerequisite/co-requisite and repeat policy checked using applicable grade and registration history.
- Max/min term credits; student status, academic standing and relevant holds.
- Time collision, already registered section/course, closed/cancelled section and cross-listing constraints.
- R1 cross-career registration is **denied by default** unless a defined policy/approved override allows it. Sophisticated credit-transfer handling is R2.

**Atomic behavior:** Use transaction and row-lock or equivalent atomic seat allocation; unique active registration key; request idempotency key; retry-safe DB deadlock policy; outbox write in same transaction. A cached seat count is advisory only, never authority.

**Drop vs withdrawal:** Both are separately effective-dated, have different transcript/financial consequences and different deadlines. R1 supports add/drop; **withdrawal with transcript ‘W’** is only included if the reference institution's signed-off fixture is provided; otherwise an explicitly modeled request is created for manual registrar handling and fuller automated withdrawal is R2.

**Negative scenarios:** seat taken between eligibility preview and commit; another tab books same course; late window; financial hold appears at commit; credit load exceeds policy; two concurrent careers share a section; advisor override expires; event webhook down; API retry; database deadlock.

- **J-WAIT:** student joins an eligible section waitlist → versioned criteria rank candidates with explanation → authorized optimization respects hard constraints → auditable time-limited offer → atomic acceptance and rule recheck → expiry/decline safely promotes next candidate. Enrollment capacity and eligibility remain authoritative.

- **J-WAIT:** student joins an eligible section waitlist → versioned criteria rank candidates with explanation → authorized optimization respects hard constraints → auditable time-limited offer → atomic acceptance and rule recheck → expiry/decline safely promotes next candidate. Enrollment capacity and eligibility remain authoritative.

## Acceptance scenarios

**AC-007 | Last seat contention**  
**Given** one remaining seat in section S and two eligible different students  
**When** both submit registration concurrently  
**Then** exactly one succeeds, the other receives `SECTION_FULL`; no overcapacity or orphan event.  
**Covers:** REG-04, INT-02, INV-05; **Proof:** concurrent test, DB and outbox state.

**AC-008 | Stale eligibility preview**  
**Given** a prior eligible preview and a financial hold placed before commit  
**When** student submits registration  
**Then** commit reevaluates hold, denies the transaction, and returns a localized reason.  
**Covers:** REG-03, REG-05, FIN-03; **Proof:** API reason, no registration and audit.

**AC-009 | Course schedule conflict**  
**Given** an enrolled Monday 10:00 section  
**When** student attempts an overlapping section with no approved exception  
**Then** booking is denied with stable timetable-conflict reason and non-overlapping options displayed where available.  
**Covers:** REG-02, REG-03; **Proof:** schedule fixture and response.

**AC-025 | Waitlist priority, optimization and concurrent promotion**  
**Given** versioned priority criteria, tied candidates, an expired offer and one remaining seat  
**When** ranking/optimization and simultaneous acceptance/promotion run  
**Then** approved hard constraints/tie-breaks are reproducible, ineligible candidates excluded, reasons/overrides audited and only one valid seat commits.  
**Covers:** REG-04/05/09/11; **Proof:** criterion fixtures, simulation trace and concurrency report.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../shared/api-contracts.md), [architecture/invariants](../shared/architecture.md), [screens](../shared/screens.md), [quality](../shared/quality.md), [traceability](../shared/traceability.md) and [release gates](../shared/release-plan.md). See [acceptance coverage](../shared/acceptance-index.md) and [delivery/cross-cutting capabilities](../shared/system-capabilities.md).

## Scope and completion

This document is the product requirements source for the whole business module, independent of implementation role. Backend, AI, UI/UX, Frontend, QA and Security tasks/specifications will be derived from the reviewed module PRD later; separate role files are not part of the current PRD structure. Applicable shared requirements remain binding. Mobile/native offline/widgets/device push are deferred. Requirement/acceptance IDs and release assignments are preserved; no design, implementation, test execution or institutional approval is claimed.
