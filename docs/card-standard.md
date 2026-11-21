# Card standard

`standard: v1` · 2026-11-21

Every plugin in this marketplace shows its user the same kinds of card: a **plan card** that waits for approval, **status lines** while it works, and a **preview** of what it is about to save. This standard makes them look and read the same in every plugin. It comes from a design council on 2026-11-21.

A plugin can't read this file at runtime, because installed plugins don't ship the marketplace repo. Each plugin therefore copies its own example card into its `SKILL.md`, and its evals copy the Grader section below. Both copies carry the `standard: v1` stamp, so `grep -r "standard: v1"` finds every copy when the standard changes.

## Rules

1. **No code fence around a card.** Claude Code renders unfenced markdown: tables get borders, and narrow terminals and screen readers get a stacked `Header: value` form. A fence shows a card as one tinted block of raw text.
2. **Title: one bold line**, `**<Display Name> plan** — <mode or scope>`. The display name is the README's H1, in Title Case, and a path scope goes in backticks. A bold line rather than a heading keeps cards from competing with the reply's own headings.
3. **Single values are a bulleted list with bold labels**, `- **Budget:** ~300k tokens, ~12 min`. Consecutive bare lines would run together as one paragraph.
4. **One label set:** Mode, Scope, Roster, Budget, Output, plus Opening question where a plugin has one. Each field means one thing in every plugin.
5. **A table is only for parallel records**, such as seats or findings: at most 4 columns, short cells, and a non-empty header in every column. Single values in a table would read as noise, and an empty header prints a stray label in the stacked layout.
6. **Long paths go in a bold-label line, not a table cell.** A long path squeezes the other columns.
7. **Reasons go in bullets under the table**, each naming its row (`- **Why llm-attacker:** …`). A wide "Why" column would crowd the table.
8. **More than ~8 rows gets a one-line count above the table** (`9 seats: 3 Opus, 6 Sonnet`), so a screen-reader user can judge the size first.
9. **A blank line goes before and after every list and table.** Without one, the next line can be swallowed into the list or become a table row.
10. **The reply line is the card's last line when the card asks for a reply.** It reads `Reply:` then each verb in its own backticks, separated by ` | `, after a blank line and never inside a table. Backticks keep `<seat>` from vanishing as an HTML tag. Pipes mark where one verb ends, which screen readers can't hear from backticks.
11. **No commas inside an example argument** on the reply line (`go except 3`, not `go except 3,7`). Read aloud, `3,7` blurs into the next verb. Users can still type commas.
12. **Every verb, placeholder and path goes in backticks.** Unfenced, `_` and `*` in a path turn into emphasis.
13. **No meaning carried by a glyph, an emoji, position or an empty cell.** Flags are words (`uninstall (reversible)`). A terminal has no ARIA, so the card has to make sense read as plain text.
14. **No hand-drawn boxes or rules.** That means no `┌─┐`, no `=====` underlines and no `───` separators of your own. The renderer draws the table borders, and hand-drawn ones break at narrow widths and are read glyph by glyph.
15. **Status lines are one whole-line code span, with their text unchanged and no bold**, e.g. `` `SPAWN: 4/4 ok | started=[…] | failed=[]` ``. Regex graders and users grep them. Call them "status line" in docs.
16. **Previews:** a line `**Draft <report|log>** — N words, not saved yet`, then the draft rendered as markdown (no fence, no frontmatter), then `---`, then `Reply: \`save\` | \`amend <note>\` | \`discard\``.
17. **Machine lines stay plain.** A line that a notifier or script reads, such as context-cleaning-crew's `compare.py` summary, keeps no markup, and stays last where it is defined to be last.

## Grader

Copy this section into each plugin's plan-card evals: the rules text into an LLM grader (`standard: v1` in its body), and the regexes into regex graders (`# standard: v1` as a front-matter comment). The regexes run on `last_message`, not on the whole trace, because an output style can print its own `───` rules.

**LLM grader (`card-format.md`), layout only (the content graders already check what the card says):**

> The final message's plan card follows the card standard v1. Pass only if ALL hold: (1) the card is not inside a code fence; (2) it starts with a bold title line naming the plugin, then an em dash and the mode or scope; (3) single values are a bulleted list with bold labels; (4) seats or findings, if listed, are a markdown table with a header row, and findings keep their numbers; (5) it contains no hand-drawn box characters and no `=====` underline; (6) when the card asks for a reply, its last line starts with `Reply:` and lists the plugin's verbs, each in backticks, separated by `|`.

**Regex graders:**

| File | `match` | Pattern |
|---|---|---|
| `reply-line.md` | `contains` | `Reply: .*\bgo\b` (plan cards) or `Reply: .*\bsave\b` (previews) |
| `no-boxes.md` | `not_contains` | `[┌┐└┘├┤┬┴┼│║╔╗╚╝]\|(^\|\n)={5,}(\n\|$)` |

Keep the existing status-line graders (`target: trace`, `match: contains`, e.g. `SPAWN: 3/3`) as they are.

## Canonical examples

These use invented projects. Copy the shape, not the content.

### design-council

**Design Council plan** — debate

- **Mode:** debate
- **Roster:** 5 seats
- **Budget:** ~900k tokens, ~25 min
- **Output:** `~/.claude/councils/2026-03-02-event-bus/log.md`
- **Opening question:** Should the order service publish events through an outbox table or straight to the broker?

| Seat | Model | Effort |
|---|---|---|
| principal-engineer | Opus | high |
| platform-engineer | Sonnet | medium |
| integration-engineer | Opus | medium |
| test-engineer | Sonnet | medium |
| sre-engineer | Sonnet | medium |

- **Why sre-engineer:** opt-in; the broker runs on shared production infrastructure.

Reply: `go` | `swap X for Y` | `drop X` | `add X` | `abort`

Status line: `HANDSHAKE: 5/5 ok | silent-spawn=[] | verdict=PROCEED`

### red-team

**Red Team plan** — `services/billing`

- **Scope:** `services/billing`
- **Budget:** ~350k tokens, ~14 min
- **Output:** `~/.claude/red-team/2026-03-02-billing/report.md`

| Seat | Model | Effort |
|---|---|---|
| input-attacker | Sonnet | medium |
| auth-attacker | Opus | high |
| logic-attacker | Opus | high |
| api-attacker | Sonnet | medium |

- **Why api-attacker:** opt-in; the service exposes a public webhook endpoint.

Reply: `go` | `add <seat>` | `drop <seat>` | `narrow scope to <path>` | `abort`

Status line: `SPAWN: 4/4 ok | started=[input-attacker, auth-attacker, logic-attacker, api-attacker] | failed=[]`

### compliance-panel

**Compliance Panel plan** — repo root

- **Scope:** repo root
- **Budget:** ~280k tokens, ~11 min
- **Output:** `~/.claude/compliance-panel/2026-03-02-clinic-app/report.md`

| Seat | Model | Effort |
|---|---|---|
| gdpr-assessor | Opus | high |
| eprivacy-assessor | Sonnet | medium |
| ai-act-assessor | Opus | medium |
| dpia-assessor | Opus | high |

- **Why dpia-assessor:** opt-in; appointment notes store health data.

Reply: `go` | `add <seat>` | `drop <seat>` | `narrow scope to <path>` | `abort`

### context-cleaning-crew

**Context Cleaning Crew plan** — quick

- **Always-on now:** ~6.1k tokens
- **After plan:** ~4.8k tokens
- **Findings:** 3 (1 fix, 1 cut, 1 move)

**Fix**

| # | Item | Evidence | Action |
|---|---|---|---|
| 1 | todo-helper skill | calls `TodoWrite`, unavailable on this model | rewrite for the task tools (reversible) |

**Cut**

| # | Item | Evidence | Action |
|---|---|---|---|
| 2 | lint-buddy plugin | ~900 tok, 0 uses in 90 days | uninstall (reversible) |

**Move**

| # | Item | Evidence | Action |
|---|---|---|---|
| 3 | "Deploy steps" section | 7 ordered steps, ~310 tok every session | move to a `deploy` skill (confirm) |

- **Item 2, keep?** "It runs in CI on Fridays" (advocate); CI doesn't load plugins, so the cut stands.

Reply: `go` | `go except 3` | `keep 2` | `explain 1` | `abort`

In report mode the title is `**Context Cleaning Crew plan** — report mode: proposed, nothing applied`, the card has no reply line, and the `compare.py` summary stays the last line of the reply.

### Preview (any plugin)

**Draft report** — 640 words, not saved yet

*(the report, rendered as markdown)*

---

Reply: `save` | `amend <note>` | `discard`

## Changing the standard

1. Edit this file and bump the stamp (`standard: v2`), with the date.
2. In the same release cycle, update every copy: each plugin's `SKILL.md` card and its grader files. `grep -r "standard: v1"` across the plugin repos finds them.
3. Record the change in this repo's `CHANGELOG.md` and in each plugin's.
