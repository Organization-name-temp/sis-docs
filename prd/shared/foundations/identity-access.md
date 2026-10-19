# 01. Identity & Access Management

[PRD index](../../README.md) · [Group index](../backend-map.md) · [Map alignment](../module-map-alignment.md)

## Ownership and boundaries

Owns Person/account identity, authentication, configurable RBAC and scoped memberships. Academic appointments are owned by Institution Management; academic enrollment status is owned by Student Lifecycle.

## Scope from the module map

- Person registry
- User accounts
- Authentication
- Role assignments
- Permission policies
- Scoped memberships
- Security history

## Requirements

| ID | Release | Requirement | Minimum acceptance condition / test |
|---|---|---|---|
| IAM-01 | R1 | Person identity is separate from applicant/student role records | Applicant-to-student conversion reuses identity, does not duplicate; T-IAM-01 |
| IAM-02 | R1 | Enforce permissions plus institutional/object scope server-side | Other students', advisors' and tenants' record IDs denied; T-IAM-02 |
| IAM-03 | R1 | Allow multi-role users and role-scoped navigation | Taught-PG student who is also TA and academic member switches context with no duplicate identity, self-grading or privilege leak; T-IAM-03 |
| IAM-04 | R1 | Secure login/session/logout and recovery | Expired/revoked sessions cannot access APIs; T-IAM-04 |
| IAM-06 | R2 | Validated external identity-provider authentication integration | Authentication mapping and denied access tested; no enterprise lifecycle provisioning automation in scope; T-IAM-06 |
| IAM-07 | R1 | Tenant administrator configures roles, permissions and scoped assignments | Custom role creation/edit/revocation audited; cross-tenant grants and privilege escalation denied; changed grants invalidate cached access; T-IAM-07 |

## Acceptance scenarios

**AC-023 | Tenant-configured RBAC and revocation**  
**Given** a tenant administrator and a custom scoped role  
**When** the administrator creates, assigns, edits and revokes it  
**Then** grants apply only within allowed tenant scope, escalation is denied, cached access is invalidated and each change is audited.  
**Covers:** IAM-02/03/07; **Proof:** permission matrix, API/session/job negative suite.

## Shared contracts and verification

Every requirement above retains its T-* family. Apply [API contracts](../api-contracts.md), [architecture/invariants](../architecture.md), [screens](../screens.md), [quality](../quality.md), [traceability](../traceability.md) and [release gates](../release-plan.md). See [acceptance coverage](../acceptance-index.md) and [delivery/cross-cutting capabilities](../system-capabilities.md).
