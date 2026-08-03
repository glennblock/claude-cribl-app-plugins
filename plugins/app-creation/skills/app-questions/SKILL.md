---
name: app-questions
description: Guides defining the problem the app solves, and associated workflows
when_to_use: At the start of a new Cribl app project to define the problem and workflows
---

**Trigger:** Run this skill inside a Cribl app project directory to generate an app description.

**Input:** Answer questions about the app's purpose, workflows, and problems it solves.

**Output:** An app description capturing all the answers

## How to Use

1. Scaffold an app in Cribl (Apps > Create App)
2. Copy the starter code to your local machine
3. `cd` into the app directory
4. Run this skill: `/app-questions`
5. **Describe what your app does** (be specific about the problem and who uses it)
6. Answer clarifying questions if needed to sharpen the description
7. Answer the full 8-phase questionnaire about your app
8. Review and edit the generated app description

**💡 Tip:** Provide specifics in your initial description. Instead of "monitoring tool", say "help our platform team track when customer data pipelines fall behind their SLAs".

## Skill Workflow

The skill first validates clarity, then gathers information through 8 phases:

### Phase 0: Validate Initial Clarity
When the user first describes the app, the skill assesses whether the description provides sufficient detail:
- **Clear description?** If yes, proceed directly to Phase 1
- **Vague or incomplete?** Ask targeted follow-up questions before proceeding

**Clarity check triggers on:**
- Very short descriptions (< 2 sentences)
- Missing key context (no mention of: who, what problem, or why)
- Abstract statements without concrete details
- Multiple ambiguous terms

**Follow-up questions may include:**
- "Who specifically will use this app?"
- "What problem does it solve for them?"
- "How is this different from what they do now?"
- "What's the main action/outcome users want?"
- "Are there specific tools or workflows it needs to integrate with?"

The skill will loop through clarifying questions until it has enough detail to proceed confidently.

**Examples:**
- ❌ "A dashboard app" → Ask: "What will appear on this dashboard? Who will use it?"
- ❌ "Helps with monitoring" → Ask: "What specifically are you monitoring? Who's doing the monitoring?"
- ✅ "Displays CPU alerts for our infrastructure team so they can respond faster to production issues"
- ✅ "Lets data engineers bulk-reload pipelines when a config change goes out"

---

The skill then gathers information through 8 phases:

### Phase 1: Understand the Problem
- What's the main problem this app solves?
- Who will use this app?
- How are they currently solving this problem?

### Phase 2: Map User Workflows
- Walk through a typical user's journey
- Are there multiple workflows/use cases?
- Any decision points or branches?

### Phase 3: Identify Key Tasks
- What will users create, modify, or delete?
- What will users see or review?

### Phase 4: Explore Data & Integration Points
- What data does the app display?
- What does the app create or change?
- Does the app call any external services (Slack, OpenAI, etc.)?

### Phase 5: Consider Permissions & Access
- Should different users see different data?
- How should the app behave if a user lacks permission?

### Phase 6: Ask About State & Secrets
- Does the app need to save any state?
- Are there general settings?
- Are there any user-specific settings?
- Does the app handle any secure secrets?

### Phase 7: Narrow the Scope
- What's the absolute minimum (MVP)?
- What would be nice to add later?
- Anything definitely out of scope?

### Phase 8: Clarify Preferences
- What's the overall structure? (wizard, dashboard, form, table, etc.)
- Any specific look/feel preferences?

---

## Persistence: APP_DEFINITION.md

After gathering all information, the skill persists answers to `APP_DEFINITION.md` in the app directory:

```markdown
# App Definition

## Problem
[User's answer about what problem this solves]

## Target Users
[Who uses this and why]

## Workflows
### Workflow 1: [Name]
[Full description]

### Workflow 2: [Name]
[Full description]

[+ Additional workflows]

## Data & Integration Points
### Data Display
[User's answer]

### Create/Modify/Delete
[User's answer]

### External Integrations
[User's answer or "None"]

## Permissions & Access
### Different Users See Different Data?
[User's answer]

### Permission-Denied Behavior
[User's answer]

## State & Secrets
### Saved State
[User's answer or "None"]

### General-settings
[User's answer or "None"]

### User-Specific Settings
[User's answer or "None"]

### Secure Secrets
[User's answer or "None"]

## Scope
### Must-Have (MVP)
[User's answers]

### Nice-to-Have (Defer)
[User's answers]

### Out of Scope
[User's answers]

## UI Preferences
### Overall Structure
[User's answer]

### Look/Feel
[User's answer]
```

**Users can edit this file directly** in their editor, or re-run the skill to update via Q&A.


## Resources

- **Cribl Apps Docs**: https://docs.cribl.io/apps/
- **Builder Guide**: https://docs.cribl.io/apps/builder-guide/
- **GitHub Examples**: https://github.com/criblapps and https://github.com/Cribl-Community

