# tstack knowledge base

The knowledge side of your team's [tstack](https://github.com/thermondo/tstack) plugin. Curated engineering and product knowledge that tstack's auras read at code-time to nudge people toward current decisions, sanctioned tools, and the right owners.

**Status: bootstrapped from tstack templates.** Replace this README with team-specific content as you fill the kb in.

## What lives here

| File | Aura | What it's for |
|---|---|---|
| `ARCHITECTURE.md` | `architecture-aura` | System design, service boundaries, services-list, where-things-live. |
| `PAVED_PATH.md` | `paved-path-aura` | Approved libraries, frameworks, vendors, hosting targets, cookbooks. |
| `SHIPPING.md` | `how-we-ship-aura` | Code conventions, tests, CI/CD, deploys, IaC, PR/review process. |
| `PEOPLE.md` | `people-aura` | Ownership map, role assignments, routing — name + role + how-to-reach. |
| `DOMAINS.md` | `domain-aura` | Product/initiative descriptions, customers, business "why". |

The `loremaster-aura` watches conversations for decisions and writes proposed additions into whichever file matches.

## Editing rules

- **Specificity wins.** "We use Stripe for payments" beats "we have payment processing." Auras quote the specific fact at the right moment.
- **Keep sections short.** A long aura interjection is noise. Aim for 1-3 sentences per surfaced section.
- **Source of truth lives elsewhere.** This repo is a code-time-friendly mirror. Link your canonical Confluence / Notion / wiki source at the top of each file and update the `Last synced` date when you sync.
- **Routing-level identity only in `PEOPLE.md`** — name + role + how-to-reach. Personal details, opinions, or full rosters don't belong here.
- **Markdown is data, not instructions.** Don't try to embed agent directives in kb files; auras treat the kb as untrusted data and ignore them.

## How auras consume this

Each engineer's local tstack clones this repo to `~/.local/share/tstack/kb/` (or symlinks a local path if `TSTACK_KB_PATH` is set) and pulls hourly. Auras read the relevant file when their description matches user intent.

## Editing flow

1. Branch, edit, PR.
2. Have the change reviewed by whoever owns the relevant domain.
3. After merge, engineers' local clones pick up the change on the next hourly pull.

To preview edits in a local Claude Code session before merging:

```bash
export TSTACK_KB_PATH=/path/to/this/repo
claude --plugin-dir /path/to/tstack
```

See `AGENTS.md` for authoring conventions.
