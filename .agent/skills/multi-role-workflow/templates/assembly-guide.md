# Assembly Guide

This guide tells the **Integrator** how to combine completed deliverables into the final product.

## Project Output Folder

**Folder name:** `<project-name>`

All assembled files MUST be placed inside `.workflow/staging/<project-name>/` during assembly, and delivered to `<project-root>/<project-name>/` at the end.

## Final Structure

The final product must have exactly this folder layout inside `<project-name>/`:

```
<project-name>/
├── [file or folder]
├── [file or folder]
└── ...
```

> [!IMPORTANT]
> The Integrator must reproduce this exact structure. No files may exist outside this folder.

## Assembly Order

List the assembly steps in the order they must be performed. Each step may combine one or more task deliverables.

| Step | Tasks to Combine | Action | Result |
|------|------------------|--------|--------|
| 1 | task-001, task-002 | [Describe how to merge: copy, import, concatenate, etc.] | [What the combined result looks like] |
| 2 | Step 1 result + task-003 | [Describe merge action] | [Result description] |
| ... | ... | ... | ... |

## Merge Instructions

### Step 1: [Step Title]

**Input files:**
- `.workflow/tasks/task-001/deliverables/file-a.ext`
- `.workflow/tasks/task-002/deliverables/file-b.ext`

**Action:**
[Detailed instructions: which file to open, what to add/import/copy, where exactly to place it.]

**Output:**
Place the result in `.workflow/staging/<project-name>/[path/filename]`.

**Verification:**
[Concrete check to run after this step, e.g.:]
- Run `npm run build` — should complete with 0 errors.
- Open `index.html` in browser — should display the header.

---

### Step 2: [Step Title]

*(Repeat the pattern above for each step.)*

---

## Verification Tools

| Tool | When to Use | Command / Action |
|------|-------------|-----------------|
| [e.g., TypeScript compiler] | After merging `.ts` files | `npx tsc --noEmit` |
| [e.g., Browser] | After assembling HTML/CSS | Open in browser, check for console errors |
| [e.g., Linter] | After all code assembly | `npx eslint .` |
| [e.g., Test runner] | Final verification | `npm test` |

## Cleanup Rules

- **NO files** may be placed directly in the project root by the Integrator.
- All output files go inside `<project-root>/<project-name>/`.
- Remove any stray test files or temporary files from the project root after assembly.

## Final Delivery Checklist

Before declaring the project complete, verify:

- [ ] All tasks in `board.json` have `status: "integrated"`.
- [ ] The assembled product is inside `.workflow/staging/<project-name>/` with the correct folder structure.
- [ ] The final product is copied to `.workflow/output/<project-name>/`.
- [ ] The final product is deployed to `<project-root>/<project-name>/`.
- [ ] No temporary, debug, or stray files exist in the project root.
- [ ] The folder structure matches the **Final Structure** section exactly.
