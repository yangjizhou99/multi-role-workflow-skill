# Board Protocol — Concurrent Access to board.json

This document defines the **only** way any agent may read or modify `.workflow/board.json`. All three roles (Planner, Worker, Integrator) MUST follow this protocol.

## File Paths

| File | Path |
|------|------|
| Board | `.workflow/board.json` |
| Lock | `.workflow/board.lock` |

## Read-Only Access

If you only need to **read** `board.json` (no modifications):

1. Check if `.workflow/board.lock` exists.
2. If **yes** → wait 2 seconds, then check again. Repeat up to 5 times.
3. If the lock still exists after 5 retries, check the `ts` field inside the lock file. If the timestamp is older than **60 seconds**, the lock is stale — delete it and proceed.
4. Read `.workflow/board.json`.

> [!NOTE]
> Read-only access does NOT require creating a lock, but you must wait if a lock exists to avoid reading mid-write.

## Read-Write Access (Claim, Update Status, etc.)

Follow these steps **exactly**, in order:

### Step 1: Acquire Lock

1. Check if `.workflow/board.lock` exists.
2. If **yes** → wait 2 seconds, retry. Maximum 5 retries.
3. If the lock still exists after 5 retries:
   - Read the lock file and check `ts`.
   - If `ts` is older than 60 seconds → delete the stale lock, proceed to step 4.
   - If `ts` is recent → **abort** and report: "Board is locked by another agent. Cannot proceed."
4. Create `.workflow/board.lock` with this content:

```json
{
  "agent": "<your-role>-<unique-id>",
  "ts": "<current ISO 8601 timestamp>"
}
```

### Step 2: Read & Modify

5. Read `.workflow/board.json`.
6. Note the current `version` number.
7. Make your changes to the data in memory.
8. Increment `version` by 1.

### Step 3: Write & Release

9. Write the modified data back to `.workflow/board.json`.
10. **Immediately** delete `.workflow/board.lock`.

> [!CAUTION]
> You MUST delete the lock file even if an error occurs during write. A forgotten lock will block all other agents for up to 60 seconds.

## Error Recovery

| Situation | Action |
|-----------|--------|
| Lock exists, `ts` < 60s old | Wait and retry (up to 5 times) |
| Lock exists, `ts` > 60s old | Delete stale lock, proceed |
| Lock exists, no `ts` field | Delete malformed lock, proceed |
| Write failed | Delete lock, report error, do NOT retry automatically |
| board.json missing | Only Planner may create it; Workers and Integrators must report error |

## Example Lock File

```json
{
  "agent": "worker-w1",
  "ts": "2026-02-11T14:30:00+09:00"
}
```

## Example board.json Version Bump

```diff
 {
-  "version": 3,
+  "version": 4,
   "project": "My Project",
   "tasks": [
     {
       "id": "task-001",
-      "status": "open",
-      "claimed_by": null,
-      "claimed_at": null,
+      "status": "claimed",
+      "claimed_by": "worker-w1",
+      "claimed_at": "2026-02-11T14:30:05+09:00",
       ...
     }
   ]
 }
```
