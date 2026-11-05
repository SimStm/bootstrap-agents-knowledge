# Roadmap module — optional add-on for bootstrap-agents-knowledge

Apply this file **only** when the user said yes to the Roadmap question (SKILL.md, Phase 0.5). If they said no, do not
create anything from this file and do not mention roadmap in `AGENTS.md`, `knowledge/rules/` or the definition of done.

Adapt names and paths to the project, but keep the structure and the rules.

---

## What to tell the user before asking

Present this summary (in the user's language), then ask yes/no:

> **Roadmap (optional).** Adds a single home for *future* work: `knowledge/roadmap/`.
>
> - `knowledge/roadmap/README.md` — index with three tables: **Blocked**, **Open**, **Done**, with one global
>   priority order (1 = next), idea, status, dependencies and dates.
> - `knowledge/roadmap/<item-name>/README.md` — one folder per item (kebab-case, no date, never renamed) with a status
>   block, the idea, origin, dependencies, open questions and draft acceptance criteria. `spec.md`, `plan.md`,
>   `tasks.md` are added when the item is planned.
> - `knowledge/rules/roadmap.md` — always-on rule: statuses (`proposed → accepted → planned → in progress → done` /
>   `dropped`), blocked items, and the workflow add → update → plan → start → finish/drop. On **start**, the planned
>   files move (`git mv`) into `knowledge/changes/<start-date>/<item-name>/` and the normal change flow takes over.
> - `AGENTS.md` gets a **Roadmap** subsection, a "Before you start" step, a self-learning row, a definition-of-done
>   item and a "What not to do" bullet. No "later"/TODO lists anywhere else.
>
> Nothing in the roadmap is implemented or promised until its row is **Done**. Install the Roadmap module?

---

## Parameters (fill or infer)

- `KNOWLEDGE_DIR`: where internal knowledge lives. Use the one this skill installs or aligns (`knowledge/` by default).
  If the repo already uses another one (`docs/internal/`, or `knowledge~/` in a Unity package, since Unity ignores
  folders ending in `~`), use that.
- `CHANGES_DIR`: implementation bundles (spec/plan/tasks per change). Default `KNOWLEDGE_DIR/changes/` — the OKF tree
  from Phase 2.C, layout `<yyyy-MM-dd>/<feature-or-fix-name>/` with `README.md`, `spec.md`, `plan.md`, `tasks.md`.
- `RULES_DIR`: always-on rules. Default `KNOWLEDGE_DIR/rules/` with its index `README.md`.
- `OWNER`: whoever decides priorities (the maintainer). Ask if it is not obvious; otherwise write "the owner".
- Language: the project's documentation language (English if unsure — same rule as the rest of this skill).

Before writing, read `AGENTS.md` / `CLAUDE.md` and the existing knowledge folders, and follow the patterns already
there. Do **not** invent product decisions and do **not** seed fake roadmap items. Only add items the user explicitly
gives you during the bootstrap; otherwise every table keeps one `—` row.

---

## 1. Rule file — `RULES_DIR/roadmap.md`

Create it and add a row to `RULES_DIR/README.md`
(`| [`roadmap.md`](./roadmap.md) | Future work lives only in KNOWLEDGE_DIR/roadmap/ |`).

```markdown
---
type: Playbook
title: Roadmap
description: Future work lives only in KNOWLEDGE_DIR/roadmap/; how to add, plan, start, finish, block or drop an item.
tags: [agents, rules, roadmap]
---

# Roadmap

## Scope

Applies whenever someone asks to put something on the roadmap, plan work for later, or change, block, start or
finish a roadmap item. Future work lives **only** in `KNOWLEDGE_DIR/roadmap/`. No roadmap / "later" / TODO lists in
product docs, module notes, change bundles or chat. A change bundle may say "out of scope, see roadmap item
`<name>`" and link it.

## Layout

    KNOWLEDGE_DIR/roadmap/
      README.md                   # index: convention + tables Blocked, Open, Done
      <item-name>/                # kebab-case, no date, never renamed
        README.md                 # required: status block, idea, origin, dependencies
        spec.md plan.md tasks.md  # added when the item is planned (same meaning as in CHANGES_DIR)
        *.md                      # optional: research, technique notes of removed code, references

- Every file has frontmatter: `type` (required), `title`, `description`, `tags` (include `roadmap`).
- Item `README.md` starts with a status block:
  `**Status:** <status> · **Priority:** <n> · **Created:** yyyy-MM-dd · **Updated:** yyyy-MM-dd · **Depends on:** <items or —> · **Origin:** <where it came from>`,
  then: the idea (goal, what exists today with real file paths, proposal), open questions, draft acceptance criteria.
- Separate observed facts from proposals. Do not invent behaviour, files or owner decisions.
- **One item = one deliverable that can become one change bundle.** Split ideas that would need two unrelated
  bundles; merge into an existing item when it is the same work.

## Status values

| Status | Meaning | Index table |
| --- | --- | --- |
| proposed | Written down, not decided by the owner | Open (or Blocked) |
| accepted | The owner wants it; not planned yet | Open (or Blocked) |
| planned | `spec.md`, `plan.md`, `tasks.md` exist and match the current code | Open (or Blocked) |
| in progress | Implementation started; the bundle is in `CHANGES_DIR` | Open |
| done | The change bundle is finished | Done |
| dropped | The owner decided not to do it (keep the folder and the reason) | Done |

**Blocked** = cannot start now: an unfinished item it depends on, a missing owner decision, or a hook in another
repository. Write the reason in the index and in the item README. Move the row back to Open the day the blocker is
gone.

## Index rules (`KNOWLEDGE_DIR/roadmap/README.md`)

- Three tables, in this order: **Blocked**, **Open**, **Done**. Every item folder has exactly one row.
- **Priority** is one global order over Blocked and Open (1 = next). Renumber when an item is added, finished or
  re-ranked. Done rows have no priority.
- Columns:
  - Blocked: Priority · Item (link to folder) · Idea · Blocked by · Created · Blocked since
  - Open: Priority · Item · Idea · Status · Depends on · Created · Last updated
  - Done: Item · Idea · Result (bundle link, CHANGELOG version, or reason it was dropped) · Created · Done on
- **Idea:** macro but concrete, one to three sentences: what the user gets and the main technique.
- **Depends on / Blocked by:** name the item and what is needed from it (e.g. "`auth-rewrite`: the session API
  (task W3)"). Other repositories: `<Repo>: <hook>`.
- Dates `yyyy-MM-dd`. *Last updated* changes whenever files, status, priority or dependencies change.

## Workflow

1. **Add** ("put X on the roadmap"): create `<item-name>/README.md` and the index row; set priority with the owner
   (default: after every existing Open/Blocked item) and dependencies. Removed code the item may bring back gets a
   **technique note** in the folder (what it did, the maths/logic, where it lived, which commit) — to understand
   behaviour, not to copy code.
2. **Update:** edit the item files and its row together; bump *Last updated*.
3. **Plan:** write `spec.md`, `plan.md`, `tasks.md` in the item folder. Status `planned`.
4. **Start:** `git mv` the planned files (everything except `README.md`) into
   `CHANGES_DIR/<start-date>/<item-name>/`, write that bundle's `README.md` from the item README, fix relative links.
   Replace the item `README.md` body with the status block plus a link to the bundle. Status `in progress`. Then
   follow the normal change rules (CHANGELOG entry, changes index row).
5. **Finish:** move the row to Done with the bundle link and CHANGELOG version.
6. **Drop:** move the row to Done, status `dropped`, with the owner's reason.

Several same-day edits in one pass are fine; never leave a folder without a row or a row without a folder.
```

---

## 2. Index — `KNOWLEDGE_DIR/roadmap/README.md`

```markdown
---
type: Reference
title: Roadmap
description: All future work for <repo-name> — one folder per item, one row per item, one global priority order.
tags: [roadmap]
---

# Roadmap

Future work for **<repo-name>**. Nothing here is implemented or promised until its row is **Done**. Rules:
[`RULES_DIR/roadmap.md`](<relative link>).

    roadmap/
      README.md                   # this index
      <item-name>/
        README.md                 # status block, idea, origin, dependencies
        spec.md plan.md tasks.md  # once planned
        *.md                      # research, technique notes, references

- **Priority:** one global order over Blocked and Open (1 = next); renumber on add, finish or re-rank.
- **Status:** proposed → accepted → planned → in progress → done (or dropped).
- **Blocked:** cannot start now (unfinished dependency, missing owner decision, hook in another repo); reason in the
  row and in the item README.
- **Start:** planned files move to `CHANGES_DIR/<start-date>/<item-name>/`; the item README keeps the status block and
  a link to the bundle.
- **Finish / drop:** the row moves to Done with the bundle link + CHANGELOG version, or the owner's reason.

## Blocked

| Priority | Item | Idea | Blocked by | Created | Blocked since |
| --- | --- | --- | --- | --- | --- |
| — | — | — | — | — | — |

## Open

| Priority | Item | Idea | Status | Depends on | Created | Last updated |
| --- | --- | --- | --- | --- | --- | --- |
| — | — | — | — | — | — | — |

## Done

| Item | Idea | Result | Created | Done on |
| --- | --- | --- | --- | --- |
| — | — | — | — | — |
```

---

## Item `README.md` skeleton (used later, when an item is added)

```markdown
---
type: concept
title: <Item title>
description: <one line>
tags: [roadmap, <item-name>]
---

# <Item title>

**Status:** proposed · **Priority:** <n> · **Created:** yyyy-MM-dd · **Updated:** yyyy-MM-dd · **Depends on:** — · **Origin:** <conversation, issue, removed feature…>

## Idea

### Goal

### What exists today

<Observed facts with real file paths.>

### Proposal

<Clearly labelled as proposal.>

## Open questions

- …

## Draft acceptance criteria

- [ ] …
```

---

## 3. `AGENTS.md` (and `CLAUDE.md` if it is separate)

Add these on top of the Phase 1 / Phase 3 content. Keep them short.

- **Roadmap** subsection under `## Knowledge`:

  ```markdown
  ### Roadmap — `knowledge/roadmap/`

  All future work, and only future work. Rule: `knowledge/rules/roadmap.md`. Index: `knowledge/roadmap/README.md`.

      knowledge/roadmap/
        README.md        # Blocked, Open, Done tables
        <item-name>/     # README.md (+ spec.md, plan.md, tasks.md once planned)

  1. Add = item folder + index row (priority, idea, dependencies, created date). Never keep roadmap lists elsewhere.
  2. Every change to an item updates its row the same day.
  3. Start moves the planned files into `knowledge/changes/<start-date>/<item-name>/` with a stub README; finish moves
     the row to Done.
  ```

- **Before you start:** add the step "If the work is a roadmap item, read its folder in `knowledge/roadmap/`."
- **Project structure** tree: add `roadmap/   # future work (index + one folder per item)` under `knowledge/`.
- **Self-learning table:** row
  `| Future work, ideas for later, removed features that may return | `knowledge/roadmap/<item>/` + a row in the index |`.
- **Definition of done:** `- [ ] Roadmap index updated if the work started, finished, blocked or unblocked a roadmap item`.
- **What not to do:** `- Do not keep roadmap or "later" lists outside `knowledge/roadmap/`, or add an item without its index row.`

Replace `knowledge/` with `KNOWLEDGE_DIR` if the repo uses another directory.
