# tstack

Coding-agent-native engineering governance. tstack ships **auras** — passive skills that sit in your coding agent and fire on intent, without slash commands — pointed at a configurable knowledge base (e.g. `thermondo/tstack-kb`) holding your team's architecture decisions, tech-radar choices, and ownership facts.

**Status: prototype.** Do not install in production.

## Quick install

```bash
git clone https://github.com/thermondo/tstack.git
cd tstack
claude --plugin-dir .
```

Then inside the agent:

```
/tstack-setup
```

`/tstack-setup` walks you through pointing at your knowledge base (a git URL or local path) — or creating one for you from scratch and optionally seeding it from your existing wiki, repos, Notion, Linear, or Slack via whatever source MCPs are installed in the session — validates access, clones the kb, and runs a health check. After that, auras auto-activate based on what you ask the agent to do.

## Auras

The skills tstack ships are **auras** — borrowed from RPGs like Diablo 2, where a paladin's aura emits a constant effect on everyone in radius without being cast. Same idea here: auras are always loaded, never invoked by command, and intervene only when something in their domain shows up in what you're doing. The user keeps working casually; the aura fires when it sees a reason.

Properties of an aura:

- **Passive activation.** No `/slash-command`. The aura watches user intent and triggers when the topic matches its domain.
- **Quiet by default.** When an aura activates with nothing specific to flag, it stays silent. Most invocations should produce no output — false positives erode trust faster than missed activations.
- **Stackable.** Multiple auras can be loaded at once. They don't conflict; each owns its own domain.
- **Advisory, not blocking** (for steering/assistance auras). Surface a short note and yield control back. The user remains in charge.

Three flavors:

- **Steering auras** — check whether your direction aligns with current company standards and surface a short note if it doesn't. Advisory, never block, never ask questions.
- **Assistance auras** — answer with context the agent wouldn't otherwise have: who maintains this, where the runbook lives, what the customer is for this domain.
- **Side-effect aura** (currently just `loremaster-aura`) — captures decisions from your conversation and writes them into the right kb file. Because it takes destructive actions (edits, commits, PRs), it asks before every write.

## What it does

You message the agent casually. If what you're doing touches a topic an aura covers — architecture, technology choices, code design, product UX, ownership and routing, troubleshooting — the matching aura activates. Triggering is by intent, not by slash command; "help me design a new payments service" and "who owns the billing service?" both fire auras without you typing `/anything`.

The plugin is generic; the kb is yours. tstack ships the protocol that turns your existing governance and team docs into context-aware auras inside the agent. It ships no opinions of its own.

## Local development

```bash
cd /path/to/tstack
claude --plugin-dir .
```

Inside Claude Code, then run:

```
/tstack-setup
```

It will ask for the kb source (a git URL or local path), clone it, write your config, and run a health check.

After editing `skills/` or `bin/` files, run `/reload-plugins` inside Claude Code to pick up changes without restarting.

## Auras shipped

- `/tstack-setup` — explicit setup skill (not an aura — runs once after install or to reconfigure).
- `architecture-aura` *(steering)* — fires on architecture-shaped work: service design, where-does-this-live, extend-vs-build, services-list. Reads `ARCHITECTURE.md`.
- `paved-path-aura` *(steering)* — fires when you're picking a library, framework, vendor, hosting target, or starting a task with a sanctioned recipe. Reads `PAVED_PATH.md`.
- `how-we-ship-aura` *(steering)* — fires on shipping-shaped work: code conventions, tests, PR/review, CI/CD, deploys, IaC, repo settings, DNS/CDN. Reads `SHIPPING.md`.
- `people-aura` *(assistance)* — fires on ownership/routing questions: who owns X, who's the [role], who do I ping. Reads `PEOPLE.md`.
- `domain-aura` *(assistance)* — fires when you're starting work in an unfamiliar product domain: what is X, who's the customer, why are we building this. Reads `DOMAINS.md`.
- `loremaster-aura` *(side-effect)* — watches your conversation for decisions and identifications, and offers to add them to the right kb file. Detects whether the kb source is a local dir, a git repo, or a GitHub remote, and uses the matching workflow (edit / commit / push / PR via `gh`). Asks before every write.

## Configuration

After running `/tstack-setup`, your config lives at:

- `~/.config/tstack/config.json` — kb source URL
- `~/.local/share/tstack/kb/` — cloned knowledge base
- `~/.cache/tstack/` — derived state (last-pull timestamps, skip-state)

Override during dev:

- `TSTACK_KB_PATH=/path/to/local/kb` — point at a local kb directory instead of a remote.
- `TSTACK_KB_BRANCH=branch-name` — test a draft kb branch.

Auth uses git's standard mechanisms (SSH keys, `gh` CLI credentials, git credential helper). No tstack-specific tokens.

## License

Apache-2.0 — see [LICENSE](LICENSE).
