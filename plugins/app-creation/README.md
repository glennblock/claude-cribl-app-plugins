# app-creation

This plugin provides a structured workflow for authoring Cribl Apps. It guides you from problem definition through app brief generation, supporting validation at each step to ensure your app is well-scoped and ready for implementation.

---

## Quick Start

### What is this plugin?

The `app-creation` plugin supports a three-phase workflow for Cribl app development:

- **Phase 1 — Problem Definition:** Use `/app-questions` to define what problem your app solves, who uses it, and the key workflows it supports. Answers are saved to `APP_DEFINITION.md`.
- **Phase 2 — Brief Generation & Validation:** Use `/app-brief` to generate a comprehensive implementation brief (`APP_BRIEF.md`) from your definition. Use `/app-validate` to validate it against quality gates (scope, clarity, completeness) before implementation.
- **Phase 3 — Implementation:** Use `/app-implement` to incrementally implement app changes based on your `APP_BRIEF.md`. The skill compares against prior executions and only implements the delta.


### Before you start

- **Scaffold an app first.** Create a new app in Cribl (Apps > Create App) and copy the starter code to your local machine.
- **Run in the app directory.** All skills operate on the current working directory — `cd` into your app folder before invoking commands. Run `npm run dev` to launch the app.
- **Launch Preview** - Launch the preview in Cribl so you can test out the app after it is built.

### Walk-through: new app from scratch after it has been scaffolded

```
$ cd ./apps/my-app-name
$ npm run dev
$ open another terminal in the same folder
$ /app-questions
  # Answer questions about the app's problem, workflows, and scope.
  # Answers are saved to APP_DEFINITION.md.

$ /app-brief
  # Generates APP_BRIEF.md (comprehensive, editable implementation brief).
  # Review and edit as needed.

$ /app-validate
  # Validates the brief against quality gates.
  # If validation fails, fix the brief and re-run.

$ /app-implement
  # Implements the app based on APP_BRIEF.md.
  # Run again to implement subsequent changes.

$ /app-implement --dry-run
  # Preview what will be implemented without making changes.
```

### Where files land

```
[app-folder]/
  APP_DEFINITION.md        # Problem, workflows, scope (Phase 1)
  APP_BRIEF.md             # Implementation brief (Phase 2)
  /executions/             # Archived APP_BRIEF snapshots (Phase 3)
    APP_BRIEF_[timestamp].md
    APP_BRIEF_[timestamp].md
  ... (starter app code)
```

## Skills

| Skill | Phase | Description |
|-------|-------|-------------|
| `app-questions` | 1 | Define the problem, workflows, and scope of your app via guided Q&A |
| `app-brief` | 2 | Generate an implementation brief from `APP_DEFINITION.md` |
| `app-validate` | 2 | Validate the brief against quality gates (scope, clarity, completeness) |
| `app-implement` | 3 | Implement app changes incrementally based on `APP_BRIEF.md` |

## FAQ / Gotchas

**"Do I have to answer all 8 phases?"**
Yes. Each phase gathers essential context. Skip one and your brief will have gaps. The skill enforces this.

**"Can I edit `APP_DEFINITION.md` directly?"**
Yes, absolutely. You can edit the file or re-run `/app-questions` — both work. Direct edits are faster for tweaks.

**"What if my app is super simple?"**
Even simple apps benefit from explicit scope definition. You may find that "simple" clarifies into "has 3 workflows" or "no external integrations" — both valuable. Answer honestly and briefly.

**"Do I need to validate before handing off?"**
Yes. `/app-validate` catches gaps and ambiguities that will bite you during implementation. A few minutes validating now saves hours debugging later.

**"What does `/app-implement` do?"**
It reads your `APP_BRIEF.md`, compares it to the last execution snapshot (in `/executions`), and implements only the delta. Run it as many times as you like — each time it only touches what changed. 

**"Can I use `/app-implement --dry-run` to preview changes?"**
Yes. Dry-run shows you exactly what will be implemented without making changes.

**"Can the skills open a PR or commit?"**
No. You manage git — the skills only produce local files. Run `git add` and `git commit` yourself.

---

## Sample Files

This repository includes example documents to illustrate the plugin workflow:

- **[APP_DEFINITION.md](./samples/APP_DEFINITION.md)** — Example problem definition from Phase 1 (`/app-questions`)
- **[APP_BRIEF.md](./samples/APP_BRIEF.md)** — Example implementation brief from Phase 2 (`/app-brief`)

Use these as references to understand the expected structure and detail level for your own app documentation.

---

## Resources

- **Cribl Apps Docs:** https://docs.cribl.io/apps/
- **Builder Guide:** https://docs.cribl.io/apps/builder-guide/
- **GitHub Examples:** https://github.com/criblapps and https://github.com/Cribl-Community
