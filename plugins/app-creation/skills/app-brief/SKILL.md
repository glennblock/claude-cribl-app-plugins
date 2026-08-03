---
name: app-brief
description: Generates the app brief from the app definition
when_to_use: After completing APP_DEFINITION.md, to generate a comprehensive implementation brief
---

**Trigger:** Run this skill in the same folder where the `APP_DESCRIPTION.md` resides.

**Input:** Contents of `APP_DESCRIPTION.md`

**Output:** A comprehensive app brief (with full editing capability) that Claude Code can use to implement the app which is stored in `APP_BRIEF.md`

## How to Use

1. `cd` into the app directory
2. Run this skill: `/app-brief`
3. Review and edit the generated brief
4. Run the `/app-validate` skill
5. If validation fails, fix the brief.
6. Pass the contents of `app-brief` to your agentic tool (Claude Code)

## Skill Workflow

## Generate the Brief

Generate the implementation brief `APP_BRIEF.md` based on answers provided in `APP_DEFINITION.md`

```markdown
# [App Name] - App Brief

## Problem & Vision
[Summary of the problem the app solves and how users benefit]

## Target Users
[Who uses this app and what they're trying to accomplish]

## Key Workflows
### Workflow 1: [Name]
- **User sees**: [What appears on screen]
- **User does**: [What they click/configure/select]
- **Result**: [What gets created/changed/shown]
- **Permissions**: [Who can do this? Any restrictions?]

### Workflow 2: [Name]
- **User sees**: [What appears on screen]
- **User does**: [What they click/configure/select]
- **Result**: [What gets created/changed/shown]
- **Permissions**: [Who can do this? Any restrictions?]

[+ Additional workflows as needed]

## Data & Actions
**The app will fetch from Cribl:**
- [Resource type 1]: [Why? Used in which workflow?]
- [Resource type 2]: [Why? Used in which workflow?]

**The app will create/modify/delete in Cribl:**
- [Resource type 1]: [Which workflow creates/modifies/deletes this?]
- [Resource type 2]: [Which workflow creates/modifies/deletes this?]

**The app will remember (general state):**
- [State 1]: [What should be saved? When should it persist?]
- [State 2]: [What should be saved? When should it persist?]

(Or "None" if no general state)

**User-specific settings (stored per user):**
- [Setting 1]: [What is it? Which user(s) need it?]
- [Setting 2]: [What is it? Which user(s) need it?]

(Or "None" if no per-user settings)

**Secure secrets:**
- [Secret 1]: [What is it? How is it used?]
- [Secret 2]: [What is it? How is it used?]

(Or "None" if no secrets)

## UI Structure
### Overall Layout
[Recommended structure: wizard, dashboard, form, table, etc.]

### Key Screens/Pages
1. **[Screen Name]**: [What it shows, what user can interact with]
2. **[Screen Name]**: [What it shows, what user can interact with]
3. **[Screen Name]**: [What it shows, what user can interact with]

[Add more screens as needed]

## Permissions & Access
- **Who can use this app?**: [All members, or specific roles?]
- **Permission-aware behavior**: [How does the app respond if a user lacks access to an action?]

## External Integrations (if any)
- **[Service name]**: [What does the app do with it?]
- **[Service name]**: [What does the app do with it?]

(Or "None" if the app only works with Cribl)

## MVP Scope
**Must-have:**
- [Feature 1]
- [Feature 2]

**Nice-to-have (defer):**
- [Feature 1]
- [Feature 2]

**Out of scope:**
- [Feature 1]

## Edge Cases & Error Handling
- **If user lacks permission**: [What should the app show?]
- **If data is unavailable**: [What should the app show?]
- **If an action fails**: [What should the app show?]

## Implementation guidance (include this section verbatim)
- Read AGENTS.md first
- Then read openapi.json
- NEVER EVER use local storage 

---
```

## Export

Once approved, the final brief is ready to copy/paste into Claude Code for implementation.

---

## Key Principles

- **No technical jargon**: Users describe problems; Claude figures out implementation
- **Markdown persistence**: Easy to read, edit, and version control
- **Direct file editing**: Users can edit `APP_BRIEF_ANSWERS.md` directly for fast iteration
- **APIs handled by Claude**: Claude uses `openapi.json` to determine which Cribl APIs to call
- **User-specific storage**: Per-user settings automatically prefixed with user ID
- **MVP-first**: Encourage shipping the minimum and iterating
- **External integrations only**: List third-party services if the app calls them

## Resources

- **Cribl Apps Docs**: https://docs.cribl.io/apps/
- **Builder Guide**: https://docs.cribl.io/apps/builder-guide/
- **GitHub Examples**: https://github.com/criblapps and https://github.com/Cribl-Community

