# sjsyrek/claude-plugins

Personal Claude Code marketplace by [Steven Syrek](https://github.com/sjsyrek).

A marketplace is a Claude Code concept: a single repo that lists one or more plugins, so they can all be installed under one `marketplace add` step. Each plugin lives in its own source repo; this repo is just the pointer.

## Installation

```
/plugin marketplace add sjsyrek/claude-plugins
/plugin list
```

Then install any plugin from the list:

```
/plugin install <plugin-name>@sjsyrek
```

## Plugins

| Plugin | Description | Source |
|---|---|---|
| [`design-council`](https://github.com/sjsyrek/design-council) | Convene parallel role-specialized peer agents to debate a cross-domain decision or audit a codebase. Invoking Claude acts as CEO. | [sjsyrek/design-council](https://github.com/sjsyrek/design-council) |
| [`red-team`](https://github.com/sjsyrek/red-team) | Adversarial security review by independent specialist attacker agents. The Incident Commander cross-correlates findings to surface compound risks. | [sjsyrek/red-team](https://github.com/sjsyrek/red-team) |
| [`compliance-panel`](https://github.com/sjsyrek/compliance-panel) | EU regulatory compliance review (GDPR, e-Privacy, NIS2, AI Act, Data Act) by independent specialist assessor agents with cross-correlated compound-exposure reporting. | [sjsyrek/compliance-panel](https://github.com/sjsyrek/compliance-panel) |

## Adding a new plugin

1. Publish the plugin to its own GitHub repo (with `.claude-plugin/plugin.json` at root).
2. Append an entry to `.claude-plugin/marketplace.json` with the repo URL as `source`.
3. Commit, push, and run `/plugin marketplace update sjsyrek` from any session that already has the marketplace added.

## License

MIT (this marketplace manifest only — each plugin carries its own license).
