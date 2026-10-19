# 19. AI Assistant — backend service boundary

[Architecture](../architecture.md) · [Whole-system PRD](../../README.md) · [AI product requirements](../system-requirements.md)

The backend module map includes the AI service integration boundary. The AI product requirements and AC-019/030–032 are canonical in [AI requirements](../system-requirements.md).

## Backend responsibilities

- Authenticate AI requests and apply tenant, role, record and purpose scope before tool/data access.
- Expose allowlisted, versioned domain tools for policy retrieval, plan validation, metrics and reviewed extraction writes; reject arbitrary SQL or unauthorized identifiers.
- Revalidate all confirmed writes through ordinary domain services; generated text cannot grant access or replace official decisions.
- Keep redacted audit, model/tool/correlation references, usage controls and institution activation settings.
- Handle model/provider failure without corrupting official academic or financial state.

The AI team owns grounding, planning/extraction/reporting behavior and evaluations; frontend own their supported interaction and review surfaces; security and testing verify the complete boundary. These are responsibilities of existing AI-01–07 and IAM/INT requirements, not a second requirement registry.
