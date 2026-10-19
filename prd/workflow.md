# From a business-module PRD to delivery work

[Module PRD index](README.md)

## Current work: module product requirements

Take one business module and analyze its purpose, users, scope/boundaries, business process/states, information and business rules, exceptions, dependencies, user-facing behavior, policy questions and acceptance criteria. Update the module's single PRD file. Resolve policy assumptions with the relevant institutional owner; do not invent approved rules.

The output is a reviewed, coherent module PRD. It is not a set of Backend/AI/UI-UX/Frontend/QA/Security work files. Those tasks and technical/design specifications are derived later from the PRD and retain its stable requirement/acceptance IDs.

## Later delivery sequence

```text
Analyze and refine module PRD
             ↓
Derive role work when ready for delivery
             ↓
Phase 1: Backend + AI + UI/UX
             ↓
S1: Backend/AI security assessment
UI/UX: product/design review only
             ↓
Phase 2: Frontend
             ↓
S2: Frontend/integration security assessment
             ↓
Phase 3: QA
             ↓
S3: QA/security evidence review
             ↓
Module acceptance / closure
```

There is no UI/UX security assessment. AI participates in Phase 1 and supports integration/fixes later. QA planning and implementation checks may start early; integrated QA follows Frontend/S2. Mobile, native offline/widgets/device push are deferred and have no active work or release gate.

## PRD readiness

- Product goal, users, lifecycle, boundaries and system-of-record ownership are explicit.
- Required behavior, business rules, permitted transitions, failure/exception cases and approval authority are defined at the appropriate institutional-policy depth.
- Requirements and user-facing outcomes have stable IDs, release scope and demonstrable acceptance criteria.
- Dependencies/shared capabilities and unresolved policy questions have owners and required decision evidence.
- Role work can be derived without changing or rediscovering product intent; unresolved assumptions remain visible.

A file reorganization is not proof that every institutional rule is resolved or the module is implementation-ready. Design/implementation/test/security artifacts and institutional acceptance remain pending until separately produced and verified.
