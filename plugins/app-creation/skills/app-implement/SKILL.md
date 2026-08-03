---
name: app-implement
description: Implements app based on APP_BRIEF.md delta, supports dry-run capability
---

<command-name>
app-implement
</command-name>

<command-description>
Implement app changes based on APP_BRIEF.md. Compares against last execution, implements delta.
</command-description>

<command-arguments>
--dry-run (optional): Print planned changes without executing
</command-arguments>

<command-examples>
/app-implement
/app-implement --dry-run
</command-examples>

# Instructions

You are implementing a Cribl app based on an APP_BRIEF.md file. **Before starting, review `./references/cribl-apps-guidance.md`** to avoid common pitfalls with KV store, state management, error handling, and other Cribl-specific patterns.

Follow this workflow:

## 1. Validate Prerequisites
- Check that `APP_BRIEF.md` exists in the working directory
- If missing, error with: "APP_BRIEF.md not found. Run /app-brief first."
- Create `/executions` directory if it doesn't exist

## 2. Load & Compare Versions
- Read the current `APP_BRIEF.md`
- Find the most recent `APP_BRIEF_*.md` in `/executions` (sort by timestamp in filename)
- If no prior execution: treat entire current brief as new work (delta = all of it)
- If prior execution exists: diff the two to identify:
  - New sections/requirements
  - Modified sections/requirements
  - Sections removed (note: don't undo these)

## 3. Parse Delta into Action Items
- Extract specific implementation tasks from the delta
- For each task, create one clear, actionable line describing what will be done
- Example lines:
  - "Create new component: UserInput"
  - "Update API endpoint /search to support filtering"
  - "Add test coverage for AuthService"

## 4. Dry-Run Mode (if `--dry-run` flag provided)
- Print header: "=== DRY RUN: Planned Changes ==="
- Print each action item as one line (no descriptions, just the action)
- Print footer: "=== End Dry Run ==="
- Example output:
  ```
  === DRY RUN: Planned Changes ===
  Create new component: UserInput
  Update API endpoint /search to support filtering
  Add test coverage for AuthService
  === End Dry Run ===
  ```

## 5. Implementation Mode (normal execution, no `--dry-run`)
- Execute the delta: create files, modify code, update configs as needed
- Remove all linter errors, run `npm run lint`

```

## 7. Archive This Run
- Copy current `APP_BRIEF.md` to `/executions/APP_BRIEF_[ISO8601-timestamp].md`
- Use format: `APP_BRIEF_2026-07-31T14-30-45Z.md` (ISO 8601, with colons replaced by hyphens for filesystem compatibility)
- This becomes the baseline for the next run

## Key Principles
- **Source of truth**: Current `APP_BRIEF.md` always wins
- **Idempotency**: Skill can run repeatedly; each run only touches what changed
- **Stateless between runs**: `/executions` folder is the only state persistence
