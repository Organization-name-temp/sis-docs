# 03. Configuration & Policy Management

[PRD index](../../README.md) · [Group index](../backend-map.md) · [Map alignment](../module-map-alignment.md)

## Ownership and boundaries

Owns shared configuration publication/preview and visual workflow automation. Domain modules own the meaning of their policies and approvals; this engine does not become an independent academic decision authority.

## Scope from the module map

- Configuration registry
- Localized settings
- Policy publication
- Template configuration
- Change control
- Configuration preview

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| TEN-05 | R1 | Config changes are previewable and publishable with audit | Published config history shows responsible actor and version; T-TEN-05 |
| WFL-04 | R1 | Full visual workflow builder with conditional routing, approvals, SLA timers and escalation | Authorized user visually builds/previews/publishes version; invalid loops, missing resolvers and bypass denied; running instances retain version; reassignment/escalation audited; T-WFL-04 |
| CFG-01 | R1 | Typed configuration registry with localized settings and template bindings | Scope, type, defaults, locale/fallback, owner and version are validated; preview produces no live writes; approved publication and rollback preserve history; T-CFG-01 |

New map-derived requirements are draft extensions; their release assignments are planning proposals pending institutional policy/acceptance review. Existing requirement release commitments are unchanged.

## Journeys and behavior

- **J-WFL:** authorized designer drags supported nodes, configures conditions and scoped approvers → validates and simulates fixtures → publishes immutable workflow version → instances execute with approvals, timers/escalation and audit → version migration requires explicit approval. The workflow builder is distinct from the R2 admissions form designer.

## Acceptance scenarios

**AC-027 | Full visual workflow builder version safety**  
**Given** a designer configuring conditional nodes, approvers, timers and escalation  
**When** they simulate and publish v2 while v1 instances run  
**Then** invalid routes/loops/resolvers fail publish, existing instances retain v1, new instances use v2 and approvals/escalations preserve scope and audit.  
**Covers:** WFL-01–04; **Proof:** visual editor UAT, simulation and instance/version assertions.

**AC-021 | Publish-time validation of workflow/form config**  
**Given** a draft application form with missing critical labels in a released language or a workflow whose approver role has no valid resolver  
**When** admin tries to publish  
**Then** publish is blocked (or explicit approved fallback permitted for non-critical copy) with actionable validation errors and no partial active version.  
**Covers:** ADM-05, WFL-01/04, I18N-03/07; **Proof:** validation tests and active-version DB reference.

## Map-derived acceptance scenario

**AC-034 | Configuration, institution and calendar publication**  
**Given** versioned institution/unit responsibility, typed settings and career calendars  
**When** an authorized administrator previews and publishes a valid change and then attempts an invalid date/type/locale change  
**Then** preview makes no live writes; invalid publication fails atomically; valid history retains scope, timezone, actor and effective dates without changing historical student affiliations.  
**Covers:** INS-01, CFG-01, CAL-01; **Release:** R1; **Proof:** version snapshots, date/type/localization fixtures and scoped API negatives.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../api-contracts.md), [architecture/invariants](../architecture.md), [screens](../screens.md), [quality](../quality.md), [traceability](../traceability.md) and [release gates](../release-plan.md). See [acceptance coverage](../acceptance-index.md) and [delivery/cross-cutting capabilities](../system-capabilities.md).
