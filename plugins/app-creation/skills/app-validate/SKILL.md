---
name: app-validate
description: Validates that the app brief aligns with and solves the defined problem
when_to_use: After generating APP_BRIEF.md, to validate it meets the problem requirements
---

**Trigger:** Run this skill after you have generate an `APP_BRIEF.md`

**Input:** Reads 'APP_BRIEF'

**Output:** Analysis of how the app aligns with solving the problem

## Skill Workflow (do not run in the background)

## Validation

1. Read `APP_DEFINITION.md`, which defines the app's purpose and workflows
2. Read `APP_BRIEF.md`, the generated brief
3. Validate: "Does this brief accomplish the stated goals?". 
4. Report any gaps or misalignments.
5. If valid: indicate it is valid.
