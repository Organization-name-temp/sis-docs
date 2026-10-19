# 20. Multi-Tenancy Management

[PRD index](../../README.md) · [Group index](../backend-map.md) · [Map alignment](../module-map-alignment.md)

## Ownership and boundaries

Owns tenant provisioning/lifecycle/settings/isolation, support access, commercial entitlements and deployment options. Operations/recovery is cross-cutting. SaaS plans are R2 and are distinct from student tuition or aid.

## Scope from the module map

- Tenant provisioning
- Tenant lifecycle
- Isolation controls
- Tenant settings
- Feature entitlements
- Platform operations
- Support access
- Commercial plans
- Deployment options

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| TEN-01 | R1 | Provision distinct tenants and tenant settings | Data/config from tenant A inaccessible to B via API, UI, jobs or exports; T-TEN-01 |
| TEN-03 | R1 | All academic tables and events have accountable tenant scope | Scoped query/job/webhook cannot run without tenant context; T-TEN-03 |
| TEN-04 | R1 | Maintain immutable security and academic audit metadata | Actor, action, object, before/after or hashes, reason and time available; T-TEN-04 |
| TEN-06 | R2 | Dedicated tenant database/deployment option | Isolation/recovery proven in dedicated mode; T-TEN-06 |
| SAS-01 | R1 | Controlled tenant lifecycle and exceptional support access | Provision/activate/suspend/archive transitions audited; suspension blocks interactive access without deleting official records; approved time-bound support access expires and is logged; T-SAS-01 |
| SAS-02 | R2 | Commercial plans and feature entitlements | Versioned plan assignment enforces entitlements server-side; disabling a feature preserves official history and cannot bypass tenant isolation or academic correctness; T-SAS-02 |

New map-derived requirements are draft extensions; their release assignments are planning proposals pending institutional policy/acceptance review. Existing requirement release commitments are unchanged.

## Acceptance scenarios

**AC-015 | Tenant data exfiltration negative**  
**Given** authenticated user A and valid object ID belonging to tenant B  
**When** A tries read, update, export, signed-document URL and background-job access  
**Then** all attempts are denied or safely not found; event and security log contain no unauthorized PII.  
**Covers:** TEN-01, TEN-03, IAM-02, RPT-02; **Proof:** API, storage, queue and audit tests.

## Map-derived acceptance scenario

**AC-041 | Tenant lifecycle and support expiry**  
**Given** an active tenant with official records and approved expiring support access  
**When** an operator suspends/archives the tenant or support authorization expires  
**Then** unauthorized interactive/support access is denied, history and ledger remain intact, approved lifecycle changes are audited and no cross-tenant access becomes available.  
**Covers:** SAS-01, TEN-01/03/04; **Release:** R1; **Proof:** lifecycle, retained-record reconciliation and support-expiry negative tests.

## Map-derived acceptance scenario

**AC-042 | Commercial entitlements preserve academic integrity**  
**Given** versioned tenant plan and entitlements with existing academic records  
**When** an operator changes plan or disables a feature  
**Then** server-side access follows approved entitlements while official history remains retrievable under retention policy; tenant isolation, correctness and required audit cannot be bypassed by a plan setting.  
**Covers:** SAS-02, TEN-01/04; **Release:** R2; **Proof:** plan-change/entitlement API negatives and retained-history assertions.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../api-contracts.md), [architecture/invariants](../architecture.md), [screens](../screens.md), [quality](../quality.md), [traceability](../traceability.md) and [release gates](../release-plan.md). See [acceptance coverage](../acceptance-index.md) and [delivery/cross-cutting capabilities](../system-capabilities.md).
