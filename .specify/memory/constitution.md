# Speckit Framework Constitution

## Core Principles

### I. Template-Driven Consistency
All project artifacts must be created from the standardized templates in `.specify/templates/`. Templates exist for: `plan-template.md`, `spec-template.md`, `tasks-template.md`. Deviating from templates requires documented justification.

### II. Scripted Operations
Project management operations MUST be executed via the PowerShell scripts in `.specify/scripts/powershell/`. Manual ad-hoc operations are prohibited. The four canonical scripts are:
- `check-prerequisites.ps1` — validates environment before any development
- `setup-plan.ps1` — creates new project plans from the plan template
- `setup-tasks.ps1` — generates task lists from the tasks template
- `create-new-feature.ps1` — initiates feature development lifecycle

### III. Extension Isolation
Custom extensions live exclusively under `.specify/extensions/`. Each extension must be self-contained, independently versioned, and documented. No cross-extension dependencies without explicit integration manifest in `.specify/integrations/`.

### IV. Integration Declarativity
Integrations with external tools (Copilot, Speckit, etc.) are declarative only — defined via manifests in `.specify/integrations/`. No implicit integrations; every external tool relationship must have a corresponding manifest entry.

### V. Memory as Single Source of Truth
High-level governance, guidelines, and constitutional rules live in `.specify/memory/`. This directory is the authoritative source for project-wide conventions. Other configuration files reference memory, not vice versa.

### VI. Versioned Artifacts
Plans, specs, and task lists are versioned artifacts. Any change to an existing plan or spec requires a new version entry and a changelog note. Version format: `MAJOR.MINOR` where MAJOR = breaking change, MINOR = addition/refinement.

## Governance

### Amendment Process
1. Proposed changes to this constitution must be documented in a new plan before implementation.
2. Amendments require review and approval per the standard plan workflow.
3. After ratification, the `Last Amended` date must be updated and the change propagated to any affected templates.

### Compliance
- All pull requests and reviews must verify that new artifacts conform to the templates and principles in this constitution.
- Complexity must be justified; prefer simpler solutions over complex ones (YAGNI).

### Guidance Hierarchy
- This constitution supersedes all other practices.
- The `setup-plan.ps1` workflow provides runtime development guidance for the active development loop.

**Version**: 1.0.0 | **Ratified**: 2026-06-15 | **Last Amended**: 2026-06-15
