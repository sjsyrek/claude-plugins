# sjsyrek/claude-plugins

Personal Claude Code marketplace by [Steven Syrek](https://github.com/sjsyrek).

A marketplace is a Claude Code concept: a single repo that lists one or more plugins, so they can all be installed under one `marketplace add` step. Each plugin lives in its own source repo. This repo holds the catalog and the shared [card standard](docs/card-standard.md) the plugins follow.

## Installation

```
/plugin marketplace add sjsyrek/claude-plugins
/plugin install <plugin-name>@sjsyrek
```

Or run `/plugin` and browse the marketplace's plugins there. From a shell: `claude plugin install <plugin-name> --marketplace sjsyrek/claude-plugins`.

To get new versions: `/plugin marketplace update sjsyrek`, then update the plugin from `/plugin`.

## Plugins

| Plugin | Description | Source |
|---|---|---|
| [`design-council`](https://github.com/sjsyrek/design-council) | Convene parallel role-specialized peer agents to debate a cross-domain decision or audit a codebase. Invoking Claude acts as CEO. | [sjsyrek/design-council](https://github.com/sjsyrek/design-council) |
| [`red-team`](https://github.com/sjsyrek/red-team) | Adversarial security review by independent specialist attacker agents. The Incident Commander cross-correlates findings to surface compound risks. | [sjsyrek/red-team](https://github.com/sjsyrek/red-team) |
| [`compliance-panel`](https://github.com/sjsyrek/compliance-panel) | EU regulatory compliance review (GDPR, e-Privacy, NIS2, AI Act, Data Act) by independent specialist assessor agents with cross-correlated compound-exposure reporting. | [sjsyrek/compliance-panel](https://github.com/sjsyrek/compliance-panel) |
| [`context-cleaning-crew`](https://github.com/sjsyrek/context-cleaning-crew) | Audit your Claude Code setup (context cost, duplicates, stale memory, conflicting instructions, removed tools, version drift, plaintext secrets) and clean it up with per-item approval; a read-only report mode can run on a schedule. Needs `python3`. | [sjsyrek/context-cleaning-crew](https://github.com/sjsyrek/context-cleaning-crew) |

**Requirements:** design-council needs agent teams enabled (`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`, Claude Code 2.1.178+); context-cleaning-crew needs `python3` on `PATH`. red-team and compliance-panel need nothing extra.

## Adding a new plugin

1. Publish the plugin to its own GitHub repo (with `.claude-plugin/plugin.json` at root).
2. Follow the [card standard](docs/card-standard.md) for anything the plugin shows the user (plan cards, status lines, previews): copy its example card into your `SKILL.md`, and its Grader section into your plan-card evals.
3. Append an entry to `.claude-plugin/marketplace.json` with the repo URL as `source`, and a row to the table above.
4. Check both with `claude plugin validate .` (in this repo and in the plugin's).
5. Commit, push, and run `/plugin marketplace update sjsyrek` from any session that already has the marketplace added.

## License

MIT (this marketplace manifest only — each plugin carries its own license).
