# Naming Convention

Consistent naming ensures all roles can find files without ambiguity.

## Directory Names

| Directory | Purpose |
|-----------|---------|
| `.workflow/` | Runtime root — always at project root |
| `.workflow/plan/` | Planner outputs (plan + assembly guide) |
| `.workflow/tasks/` | All task folders |
| `.workflow/tasks/task-NNN/` | Individual task (NNN = zero-padded 3 digits) |
| `.workflow/tasks/task-NNN/deliverables/` | Worker's output files for that task |
| `.workflow/staging/` | Integrator's assembly workspace |
| `.workflow/output/` | Final product delivered to the user |

## Task IDs

- Format: `task-NNN` where NNN is a zero-padded 3-digit number.
- Examples: `task-001`, `task-012`, `task-100`.
- IDs are assigned by the Planner in creation order.
- IDs are **immutable** — never rename or renumber.

## File Names

| File | Location | Created By |
|------|----------|-----------|
| `board.json` | `.workflow/` | Planner |
| `board.lock` | `.workflow/` | Any role (ephemeral) |
| `master-plan.md` | `.workflow/plan/` | Planner |
| `assembly-guide.md` | `.workflow/plan/` | Planner |
| `spec.md` | `.workflow/tasks/task-NNN/` | Planner |
| *(deliverables)* | `.workflow/tasks/task-NNN/deliverables/` | Worker |

## Deliverable File Names

- Defined by the **Planner** in `spec.md` and listed in `board.json` under the task's `deliverables` array.
- Workers MUST use the **exact** filenames specified — no renaming, no adding prefixes/suffixes.
- Names should be descriptive and include the file extension (e.g., `header.html`, `auth-utils.ts`, `logo.png`).
- Must be unique across the entire project to avoid merge conflicts during integration.

## General Rules

1. Use **lowercase** with **hyphens** for all names (no spaces, no underscores, no camelCase).
2. Always include file extensions.
3. All paths in documentation and `board.json` are **relative** to the project root.
