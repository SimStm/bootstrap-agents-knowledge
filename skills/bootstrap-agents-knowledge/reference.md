# Reference — templates for bootstrap-agents-knowledge

Use these skeletons as starting points. Replace placeholders with discovery results. Omit sections that do not apply.

The skeletons below are the base install (no roadmap). If the user opted into the Roadmap module, also apply the
additions in [roadmap.md](roadmap.md) §3 to `AGENTS.md` and `knowledge/rules/README.md`.

---

## `AGENTS.md` skeleton

```markdown
# AGENTS.md — <repo-name>

Agent-facing guide for this repository. Follow existing project patterns unless the task introduces something genuinely new.

**<one-line product identity>.** <What this repo is / is not. Sibling repos if any.>

**Language:** write OKF entries, technical docs for agents, code comments meant as durable guidance, CHANGELOG entries, and `knowledge/` docs in **English**. Chat with humans may use another language; durable artifacts stay in English.

**Naming (code, not prose):** <state identifier language rule; recommend English identifiers>. Retroactive: if you touch a file that violates the rule, rename it fully as part of that change.

## Before you start

1. Read this file.
2. Read every `knowledge/rules/*.md` file **except** `knowledge/rules/README.md`.
3. Skim relevant `knowledge/product/` docs when the task touches that domain.
4. <Optional: link to external canonical playbooks — do not copy them into this repo.>

## Stack

| Layer | Choice |
| --- | --- |
| Runtime | … |
| App / API | … |
| Data | … |
| Lint / format | … |
| Tests | … |
| Scaffolding | … |

## Project structure

\`\`\`
<important folders only>
knowledge/
  rules/     # short always-on rules (any agent)
  product/   # durable product knowledge
  changes/   # OKF change bundles
\`\`\`

## Conventions that matter

<Only invariants grounded in this repo — auth, tenancy, data ownership, API contracts, naming, etc.>

## Knowledge

### Always-on rules — `knowledge/rules/`

Short, **tool-agnostic** rules. Load them at task start. Add a new short rule when a constraint should apply to every future task.

Do **not** rely on IDE-only rule folders alone. Prefer `knowledge/rules/` for portability.

### Product knowledge — `knowledge/product/`

When work introduces or changes **durable** product/domain behavior, create or update `knowledge/product/<topic>.md`.

- Change-specific notes → `knowledge/changes/…` (OKF)
- Stable behavior that outlives one PR → `knowledge/product/`
- Link product docs from the OKF change `README.md` when relevant

### OKF changes — `knowledge/changes/` (required for every feature or fix)

Spec: [Open Knowledge Format (OKF)](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md).

\`\`\`
knowledge/changes/<yyyy-MM-dd>/<feature-or-fix-name>/
  README.md
  spec.md
  plan.md
  tasks.md
\`\`\`

1. One directory = one implementation set. Follow-ups stay in the same directory.
2. Cross-repo work: identical `<feature-or-fix-name>` in every affected repository.
3. Bundle `README.md` lists issues/tasks (id and URL) and affected repositories.
4. Concepts need OKF frontmatter (`type` required; prefer `title`, `description`, `tags`).
5. Also include `spec.md`, `plan.md`, and `tasks.md` (Planning and implementation). They do not replace `README.md`.

### Planning and implementation

Before implementing a feature or fix, create or update one bundle (reuse it for follow-up work on the same change):

\`\`\`
knowledge/changes/<yyyy-MM-dd>/<feature-or-fix-name>/
  spec.md
  plan.md
  tasks.md
\`\`\`

Keep the project's existing OKF `README.md` and frontmatter when those are already required.

- **spec.md** — contract: problem, goal, scope, expected behavior, main and error scenarios, edge cases, observable acceptance criteria. Implementation must satisfy the spec.
- **plan.md** — how to meet the spec: current-code analysis with real file references, affected components, technical decisions, data or API changes, risks, implementation sequence. Update the plan when discoveries appear; justify deviations and stay within the spec.
- **tasks.md** — ordered execution: small tasks, dependencies, per-step checks, applicable test commands. Update progress while implementing.

A new session must be able to execute the change from these files alone. Separate facts observed in the repository from proposals and assumptions. Do not invent behavior, files, commands, or product decisions.

Plan-only requests: write and review the bundle; do not change code. Implementation requests: confirm the bundle still matches the current code, create or update it before editing code, and keep it current as discoveries and decisions appear.

When implementation finishes: check every acceptance criterion in `spec.md`, run the relevant tests, record results and limitations, and mark finished tasks in `tasks.md`. Record and justify technical deviations in `plan.md`. If a discovery requires changing behavior defined in `spec.md`, state that decision before editing the spec.

Keep this section short. Change detail belongs in the bundle.

### Self-learning loop

| After the work… | Write to… |
| --- | --- |
| Constraint for every future task | `knowledge/rules/<topic>.md` + index |
| Durable product/domain behavior | `knowledge/product/<topic>.md` |
| This feature/fix set | `knowledge/changes/<date>/<name>/` — open before code (`spec.md`, `plan.md`, `tasks.md`); finish after the work |
| Human macro history | `CHANGELOG.md` (link OKF) |
| Human onboarding / run / structure | `README.md` |

Chat conclusions that change how the product works are incomplete until written into `knowledge/`.

### Agent definition of done

- [ ] Code + tests as needed (skip code when the request was plan-only)
- [ ] OKF bundle created/updated, including `spec.md`, `plan.md`, and `tasks.md`
- [ ] For an implementation: each `spec.md` acceptance criterion checked; relevant tests run; results and limitations recorded; `tasks.md` marked; technical deviations justified in `plan.md`
- [ ] CHANGELOG entry linked to OKF
- [ ] Product and/or rules updated if durable behavior/constraints changed
- [ ] README updated if human onboarding surface changed

## CHANGELOG.md

Append a macro entry after each feature or fix set (English, newest first). Mirror the OKF identity (same date folder and name). Link the OKF path. Prefer Keep a Changelog categories: Added, Changed, Fixed, Removed.

## README.md

When a change is significant for humans using or onboarding to the repo, update `README.md` in the same change set.

## Code quality bar

<Project-true bullets: simplicity, security boundaries, performance hazards, tests.>

## Commands

\`\`\`bash
<install>
<dev>
<test>
<lint>
<typecheck>
\`\`\`

## What not to do

- Do not leave a feature/fix without OKF + CHANGELOG.
- Do not edit code for a feature or fix before `spec.md`, `plan.md`, and `tasks.md` exist and match the current code.
- Do not change behavior defined in `spec.md` without stating that decision first.
- Do not put always-on rules only in IDE-specific config — use `knowledge/rules/`.
- Do not invent parallel abstractions when project helpers already fit.
- <Repo-specific anti-patterns.>
```

---

## `knowledge/rules/README.md`

```markdown
---
type: Reference
title: Always-on agent rules
description: Short, agent-agnostic rules. Load every file in this directory before implementing.
tags: [agents, rules]
---

# Always-on rules

These files are **tool-agnostic**. `AGENTS.md` instructs agents to read every `*.md` here (except this README) at the start of a task.

Keep each rule short and enforceable. Prefer linking to `knowledge/product/` for deep context.

| File | Topic |
| --- | --- |
| [`english-artifacts.md`](./english-artifacts.md) | English for durable artifacts |
```

---

## Rule file skeleton

```markdown
---
type: Playbook
title: <short title>
description: <one line>
tags: [agents, rules]
---

# <Rule name>

- Do …
- Do not …
- See also: `knowledge/product/<topic>.md`
```

---

## Product doc stub

```markdown
# <Topic>

## Purpose

<What problem this domain solves.>

## Invariants

- …

## Open questions

- …
```

---

## OKF change `README.md`

```markdown
---
type: Reference
title: <Feature or fix name>
description: <one line>
tags: [change, okf]
---

# <Feature or fix name>

## Summary

<What changed and why.>

## Issues / tasks

| Id | URL |
| --- | --- |
| … | … |

## Affected repositories

- `<this-repo>`

## Product knowledge

- …

## Related concepts

- …
```

---

## `spec.md`

```markdown
---
type: concept
title: <Feature or fix name> — spec
description: <one line>
tags: [change, <feature-or-fix-name>]
---

# Spec — <feature or fix name>

## Problem

<Observed problem. Label repository facts separately from assumptions.>

## Goal

## Scope

### In scope

### Out of scope

## Expected behavior

## Scenarios

### Main

### Error

### Edge cases

## Acceptance criteria

- [ ] <Observable criterion an implementer can check>
```

---

## `plan.md`

```markdown
---
type: concept
title: <Feature or fix name> — plan
description: <one line>
tags: [change, <feature-or-fix-name>]
---

# Plan — <feature or fix name>

## Current code

<Analysis with real file paths. Label assumptions.>

## Affected components

## Technical decisions

## Data or API changes

## Risks

## Implementation sequence

## Deviations

<Empty until implementation. Each entry: what changed, why, and how it still meets spec.md.>
```

---

## `tasks.md`

```markdown
---
type: concept
title: <Feature or fix name> — tasks
description: <one line>
tags: [change, <feature-or-fix-name>]
---

# Tasks — <feature or fix name>

Update status while implementing. Record the command used for each check.

| # | Task | Depends on | Verification | Status |
| --- | --- | --- | --- | --- |
| 1 | … | — | `<test command>` | pending |

## Results

<After implementation: tests run, pass/fail, limitations.>
```

---

## OKF concept file

```markdown
---
type: Playbook
title: <concept title>
description: <one line>
tags: [change, <feature-or-fix-name>]
---

# <Concept>

<Implementation-scoped detail for this change set.>
```

---

## Minimal `CHANGELOG.md`

```markdown
# Changelog

All notable changes to **<repo-name>** are documented here (English, macro view).

Deep detail lives in OKF bundles under `knowledge/changes/<yyyy-MM-dd>/<feature-or-fix-name>/`.
Format inspired by [Keep a Changelog](https://keepachangelog.com/).

## [Unreleased]

### Added

- …
```

---

## Bootstrap OKF note (optional first change)

Only if the user wants the bootstrap itself recorded:

`knowledge/changes/<today>/agents-knowledge-bootstrap/README.md` — summary of AGENTS + knowledge install; list this repo under affected repositories.
