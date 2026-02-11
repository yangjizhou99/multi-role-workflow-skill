---
name: multi-role-workflow
description: Orchestrate 4 AI agent roles (Planner, Worker, Integrator, Fixer) to collaboratively decompose, execute, assemble, and maintain project deliverables with file-lock concurrency control.
---

# Multi-Role Workflow Skill

This skill enables multiple AI agents to collaborate on a project by assuming one of four roles. It is **project-agnostic** — copy the entire `.agent/skills/multi-role-workflow/` folder into any project to use it.

## Quick Start

1. The user tells you which **role** to adopt: `Planner`, `Worker`, `Integrator`, or `Fixer`.
2. Read the corresponding role document below.
3. Follow the instructions exactly.

## Role Detection

| User says (keywords) | Role | Document to read |
|---|---|---|
| "planner", "plan", "decompose", "design tasks" | **Planner** | [roles/planner.md](roles/planner.md) |
| "worker", "work", "do task", "pick up task" | **Worker** | [roles/worker.md](roles/worker.md) |
| "integrator", "integrate", "assemble", "merge" | **Integrator** | [roles/integrator.md](roles/integrator.md) |
| "fixer", "fix", "bug", "debug", "repair" | **Fixer** | [roles/fixer.md](roles/fixer.md) |

> If the role is ambiguous, ask the user to clarify before proceeding.

## Shared Protocols (ALL roles must read)

- **[Board Protocol](protocols/board-protocol.md)** — How to safely read/write `board.json` with file locking. **Every agent MUST follow this before touching `board.json`.**
- **[Naming Convention](protocols/naming-convention.md)** — File and directory naming rules.

## Runtime Directory

All runtime data lives in `.workflow/` at the project root. This directory is created by the **Planner** and used by all roles:

```
.workflow/
├── board.json              # Task states — single source of truth
├── board.lock              # Ephemeral lock (auto-deleted)
├── fix-log.md              # Bug fix history (created by Fixer)
├── plan/
│   ├── master-plan.md      # Decomposed plan with dependency graph
│   └── assembly-guide.md   # Step-by-step assembly instructions
├── tasks/
│   └── task-NNN/
│       ├── spec.md         # Task specification
│       └── deliverables/   # Worker outputs
├── staging/
│   └── <project-name>/    # Integrator assembles here
└── output/
    └── <project-name>/    # Final clean deliverable
```

The final product is also deployed to `<project-root>/<project-name>/`.

## Templates

Blank templates are available in `templates/` for reference:
- [board.json](templates/board.json)
- [task-spec.md](templates/task-spec.md)
- [assembly-guide.md](templates/assembly-guide.md)

## Rules

1. **Never modify another role's active work.** Planner creates the plan; Workers execute tasks; Integrator assembles; Fixer patches bugs.
2. **Always follow the board protocol** when reading or writing `board.json`.
3. **All file paths are relative** to the project root.
4. **Deliverable filenames are defined by the Planner** — Workers must use the exact names specified.
5. **All final output goes inside `<project-name>/`** — never scatter files into the project root.
6. **If something is unclear, stop and ask the user** rather than guessing.
