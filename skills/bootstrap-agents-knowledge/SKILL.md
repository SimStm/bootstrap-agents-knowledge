---
name: bootstrap-agents-knowledge
description: >-
  Bootstraps a portable AGENTS.md and knowledge/ tree (always-on rules, product
  docs, OKF change bundles with spec/plan/tasks before implementation, CHANGELOG
  self-learning loop) for any repository, plus an optional Roadmap module
  (knowledge/roadmap/ index + one folder per future-work item) the user opts into.
  Use when setting up agent docs in a new project, aligning an existing repo to
  the OKF/knowledge pattern, or when the user asks for AGENTS.md + knowledge
  bootstrap / self-learning agent docs.
disable-model-invocation: true
---

# Bootstrap AGENTS.md + knowledge system

**On start**, state in one line that you are following this skill.

Install a **portable, agent-agnostic** knowledge system. Do **not** assume language, framework, monorepo layout, or IDE. Infer from the target repo.

## Goals

1. Create/update root `AGENTS.md` so agents know how *this* project works.
2. Install or align:

```
knowledge/
  rules/      # short always-on constraints (any agent)
  product/    # durable product / domain behavior
  changes/    # OKF change bundles
```

3. Encode a **self-learning loop**: meaningful work writes back into the correct knowledge layer.
4. Source of truth for always-on rules = `knowledge/rules/`, **not** only IDE folders (`.cursor/rules` may mirror later).
5. **Optional — Roadmap module:** only if the user opts in (Phase 0.5), add `knowledge/roadmap/` as the single home for future work.

## Non-goals

- Do not invent a parallel docs system that replaces this tree.
- Do not put long essays in `knowledge/rules/` — keep rules short and enforceable.
- Do not copy unrelated product docs from other companies/repos; **link** shared playbooks, do not duplicate blindly.
- Do not commit secrets or real env values into knowledge files.

## Additional resources

- Section templates and file skeletons: [reference.md](reference.md)
- Optional Roadmap module (rule, index, AGENTS.md additions): [roadmap.md](roadmap.md)
- OKF spec: https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md

---

## Phase 0 — Discover

Inspect before writing:

- README, manifests, lockfiles, scripts
- Source layout
- Existing agent docs (`AGENTS.md`, `CLAUDE.md`, `.cursor/rules`, `CONTRIBUTING.md`)
- Existing `docs/` / `knowledge/` / ADRs
- Test / lint / typecheck / scaffold commands
- Sibling / multi-repo relationships
- Domain glossary clues

If `AGENTS.md` or `knowledge/` already exists, **extend and align** — do not wipe domain truth.

Build a private discovery table (purpose, stack, structure, commands, hard invariants, gaps). Ask the user only when blocked.

Also note whether the repo already has a roadmap / backlog / "later" / TODO list (in docs, README, issues folder, etc.).

---

## Phase 0.5 — Ask about the Roadmap module (mandatory question)

Before writing anything, **ask the user** whether to install the optional **Roadmap** module. This is the one question
you always ask, even when nothing else blocks you.

1. Show the short description from [roadmap.md](roadmap.md) ("What to tell the user before asking") in the user's
   language: it creates `knowledge/roadmap/` (index with Blocked / Open / Done tables + one folder per item),
   an always-on rule `knowledge/rules/roadmap.md` with statuses and the add → plan → start → finish workflow, and
   short additions to `AGENTS.md`. Mention any existing roadmap/backlog list you found in Phase 0 and that, on yes,
   you will **propose** migrating it (do not migrate without confirmation).
2. Ask a clear yes/no question (use the agent's question tool if it has one).
3. Record the answer as `ROADMAP = yes | no` and follow it in every later phase:
   - **yes** → apply everything marked *(Roadmap: yes)* below, using [roadmap.md](roadmap.md).
   - **no** → install the rest exactly as before. Create **nothing** roadmap-related: no `knowledge/roadmap/`, no
     `knowledge/rules/roadmap.md`, no roadmap lines in `AGENTS.md`.
4. If the user does not answer or is unreachable (non-interactive run), default to **no** and say so in the report.

If the repo already has `knowledge/roadmap/` following this convention, skip the question, treat it as `yes`, and
only align what is missing.

---

## Phase 1 — `AGENTS.md`

Write durable agent docs in **English** unless the project already mandates another language for agent artifacts.

Required sections (adapt headings; see [reference.md](reference.md)):

1. Identity — what this repo is / is not; siblings if any
2. Language & naming — prose vs code identifiers
3. Before you start — ordered checklist (AGENTS → all `knowledge/rules/*.md` except README → skim relevant product docs → planning bundle for a feature or fix → implement)
4. Stack — concise table
5. Project structure — important folders only
6. Conventions that matter — only real invariants from the code
7. Knowledge system — three layers + self-learning loop
8. CHANGELOG — macro + OKF links
9. README — when to update human onboarding
10. Code quality bar
11. Commands — copy-pasteable
12. What not to do — repo-specific anti-patterns

Keep `AGENTS.md` actionable. Depth goes to `knowledge/product/` or OKF.

*(Roadmap: yes)* Also add the Roadmap subsection under Knowledge, the "Before you start" step, the project-structure
line, and the self-learning / definition-of-done / what-not-to-do lines from [roadmap.md](roadmap.md) §3. Apply them to
`CLAUDE.md` too if it is a separate file (not a symlink/pointer to `AGENTS.md`).

### Planning and implementation (required in `AGENTS.md`)

Add the short operational section from [reference.md](reference.md) (“Planning and implementation”). Do not remove or rewrite existing OKF, self-learning, or definition-of-done text. Where they overlap, make them agree:

- Start or update `knowledge/changes/<yyyy-MM-dd>/<feature-or-fix-name>/` **before** editing code for a feature or fix. Reuse that directory for follow-ups on the same change.
- Require `spec.md` (contract), `plan.md` (technical approach), and `tasks.md` (ordered execution) **in addition to** the existing OKF `README.md` and concept frontmatter. The README issue table does not replace `tasks.md`.
- The files must stand alone for a new session. Separate facts observed in the repo from proposals and assumptions. Do not invent behavior, files, commands, or product decisions.
- Plan-only: write and review the bundle; do not implement. Implementation: confirm the bundle still matches current code, update it before editing, and keep it current.
- On finish: check every `spec.md` acceptance criterion, run relevant tests, record results and limitations, mark tasks done, and justify technical deviations in `plan.md`. State the decision before changing behavior defined in `spec.md`.
- Keep the `AGENTS.md` section short. Change detail lives in the bundle.

Canonical wording: [reference.md](reference.md).

---

## Phase 2 — Knowledge layers

### A. `knowledge/rules/`

- Create `knowledge/rules/README.md` (index + “read every `*.md` except this README”).
- Add only rules that are **already true** or explicitly requested.
- Typical starters when grounded: artifact language, secrets/config, auth/tenancy, destructive/chargeable confirmations, naming glossary.
- Format: short OKF-ish frontmatter (`type`, `title`, `description`, `tags`) + enforceable do/do-not bullets. Link to product for depth.

### B. `knowledge/product/`

- Durable “how the domain works” that outlives one PR.
- One topic per file; ground in the repo.
- Prefer honest stubs with “Known / Open questions” over fiction.

### C. `knowledge/changes/` (OKF)

```
knowledge/changes/<yyyy-MM-dd>/<feature-or-fix-name>/
  README.md
  spec.md
  plan.md
  tasks.md
  *.md
```

Rules:

1. One directory = one implementation set; follow-ups stay there.
2. Cross-repo work shares the same `<feature-or-fix-name>`.
3. Bundle `README.md` lists issues/tasks (id + URL when known) and affected repos.
4. Concepts need frontmatter: `type` required; prefer `title`, `description`, `tags`. `spec.md`, `plan.md`, and `tasks.md` use that same frontmatter.
5. Every feature or fix bundle also includes `spec.md`, `plan.md`, and `tasks.md` (see Planning and implementation). They do not replace `README.md`.

File shapes: [reference.md](reference.md). Bootstrap folders + convention note. Do **not** invent fake historical bundles (including empty spec/plan/tasks history).

If the repo has scaffolding (Plop, Hygen, etc.), note an `okf-change` generator as a follow-up — do not block on it.

### D. `knowledge/roadmap/` — *(Roadmap: yes only)*

Skip this section entirely when `ROADMAP = no`.

- Create `knowledge/rules/roadmap.md` and its row in `knowledge/rules/README.md` ([roadmap.md](roadmap.md) §1).
- Create `knowledge/roadmap/README.md` with the three empty tables (Blocked, Open, Done — one `—` row each)
  ([roadmap.md](roadmap.md) §2).
- Do **not** seed fake items. Add items only if the user gives them; if an existing backlog was found and the user
  confirmed migration, create one folder + row per item (status `proposed` unless the owner said otherwise) and
  remove the old list, leaving a link to `knowledge/roadmap/`.

### E. `CHANGELOG.md`

If missing, create Keep a Changelog–style:

- Macro what/why; newest first
- Each change set links to its OKF path
- Deep detail stays in OKF

---

## Phase 3 — Self-learning loop (mandatory in AGENTS.md)

Encode write-back explicitly:

| After the work… | Write to… |
| --- | --- |
| Constraint for **every future task** | `knowledge/rules/<topic>.md` + index in `rules/README.md` |
| Durable product/domain behavior | `knowledge/product/<topic>.md` |
| Notes for **this** feature/fix set | `knowledge/changes/<date>/<name>/` — open before code (`spec.md`, `plan.md`, `tasks.md`); finish after the work |
| Human macro history | `CHANGELOG.md` (link OKF) |
| Human onboarding / run / structure changed | `README.md` |
| *(Roadmap: yes)* Future work, ideas for later, removed features that may return | `knowledge/roadmap/<item>/` + a row in the index |

Also encode:

- Prefer updating existing docs over near-duplicates.
- Do not put always-on rules only in IDE-specific config.
- Chat conclusions that change product behavior are incomplete until written into `knowledge/`.
- Cross-repo features share OKF folder names.

Include an agent definition-of-done checklist:

- [ ] Code + tests as needed (skip code when the request was plan-only)
- [ ] OKF bundle created/updated, including `spec.md`, `plan.md`, and `tasks.md`
- [ ] For an implementation: each `spec.md` acceptance criterion checked; relevant tests run; results and limitations recorded; `tasks.md` marked; technical deviations justified in `plan.md`
- [ ] CHANGELOG entry linked to OKF
- [ ] Product and/or rules updated if durable behavior/constraints changed
- [ ] README updated if human onboarding surface changed
- [ ] *(Roadmap: yes)* Roadmap index updated if the work started, finished, blocked or unblocked a roadmap item

---

## Phase 4 — Done criteria

Repo must have:

1. Root `AGENTS.md` with project truth + knowledge instructions + self-learning loop
2. `knowledge/rules/README.md` + minimal true always-on rules
3. `knowledge/product/` (real docs or honest stubs)
4. `knowledge/changes/` with convention documented, including `spec.md` / `plan.md` / `tasks.md` before implementation
5. `CHANGELOG.md` present or clearly aligned
6. No stack cargo-cult (no frameworks not in the repo)
7. Shared canonical policies linked, not blindly copied
8. *(Roadmap: yes)* `knowledge/rules/roadmap.md` (indexed), `knowledge/roadmap/README.md` with Blocked / Open / Done
   tables, and the Roadmap additions in `AGENTS.md`. *(Roadmap: no)* no roadmap files or mentions anywhere.

## Report to the user

- Roadmap module: installed or not (and whether that was the user's answer or the non-interactive default)
- Files created/updated
- Invariants inferred
- Open questions / rules intentionally **not** invented
- Suggested next scaffolds (e.g. OKF generator) if relevant

## Working style

- Match existing naming and doc tone.
- Prefer editing existing agent docs over parallel competing guides.
- Keep rules short; product durable; OKF change-scoped.
- User-supplied invariants → correct layer (rule vs product vs AGENTS section).
