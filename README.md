# bootstrap-agents-knowledge

An agent skill that bootstraps a portable, agent-agnostic knowledge system in any repository:

- a root **`AGENTS.md`** describing how *this* project works;
- **`knowledge/rules/`** — short always-on rules every agent loads;
- **`knowledge/product/`** — durable product / domain behaviour;
- **`knowledge/changes/`** — [OKF](https://github.com/GoogleCloudPlatform/knowledge-catalog/blob/main/okf/SPEC.md)
  change bundles with `spec.md`, `plan.md` and `tasks.md` written **before** implementation;
- **`CHANGELOG.md`** linked to those bundles, plus a self-learning loop so finished work is written back into the
  right layer.

It does not assume a language, framework or IDE: it inspects the repo first and only writes rules that are already
true.

## Optional: Roadmap module

During the run the skill asks whether to install the **Roadmap** module. On yes it also creates:

```
knowledge/
  rules/roadmap.md           # always-on rule: statuses, blocked items, workflow
  roadmap/
    README.md                # index: Blocked, Open, Done tables, one global priority order
    <item-name>/README.md    # one folder per future-work item (+ spec/plan/tasks once planned)
```

Items move `proposed → accepted → planned → in progress → done` (or `dropped`). Starting an item moves its planned
files into `knowledge/changes/<start-date>/<item-name>/`. On no, nothing roadmap-related is created.

## Install

With the [`skills`](https://github.com/vercel-labs/skills) CLI:

```bash
npx skills add SimStm/bootstrap-agents-knowledge
```

Global install (all your projects), for a specific agent, non-interactive:

```bash
npx skills add SimStm/bootstrap-agents-knowledge -g -a claude-code -y
```

## Update

```bash
npx skills check
```

```bash
npx skills update
```

Updates are detected from the skill folder (`skills/bootstrap-agents-knowledge/`) on the default branch, so pushing
to `main` is a release.

## Use

The skill is user-invoked (`disable-model-invocation: true`). In Claude Code:

```
/bootstrap-agents-knowledge
```

In other agents, ask it to "run the bootstrap-agents-knowledge skill" in the target repository.

## Repository layout

```
skills/bootstrap-agents-knowledge/
  SKILL.md        # entry point: phases, rules, done criteria
  reference.md    # AGENTS.md, rules, OKF and CHANGELOG skeletons
  roadmap.md      # optional Roadmap module
```

## Contributing

Edit files under `skills/bootstrap-agents-knowledge/`, add a line to [CHANGELOG.md](CHANGELOG.md), open a PR. CI checks
that the skill is still discoverable by the `skills` CLI.

## License

[MIT](LICENSE)
