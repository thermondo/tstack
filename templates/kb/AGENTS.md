# AGENTS.md — working in this knowledge base

You are an AI agent helping a human edit this tstack knowledge base. The auras in tstack will read these files at code-time and use them to surface team-specific guidance during agent conversations.

## What this repo is (and isn't)

- **Is**: a structured, code-time-friendly mirror of your team's engineering and product knowledge. Markdown only. No code, no tooling, no secrets.
- **Is not**: the source of truth. Each file should link to a canonical Confluence / Notion / wiki page. Edits here keep the mirror in sync with the source — they don't override it.

## File → aura mapping

| File | Aura | Role |
|---|---|---|
| `ARCHITECTURE.md` | `architecture-aura` | steering — flags when the user's direction crosses architectural decisions or service boundaries |
| `PAVED_PATH.md` | `paved-path-aura` | steering — flags when the user is picking a tool, vendor, or recipe that the team has a position on |
| `SHIPPING.md` | `how-we-ship-aura` | steering — flags shipping-time conventions: tests, PRs, CI/CD, deploys, IaC |
| `PEOPLE.md` | `people-aura` | assistance — answers ownership and routing questions |
| `DOMAINS.md` | `domain-aura` | assistance — surfaces business context for unfamiliar product domains |

The `loremaster-aura` is the writer — it watches user conversations for conclusions and proposes additions to whichever file matches.

## Authoring rules

### 1. Be specific, name things explicitly

Auras match on **headings + body content**. Vague prose produces vague nudges or none at all. Name technologies, patterns, services, directories, teams, vendors. Those are the tokens auras match against.

- ✅ "Heroku is on Hold. We are actively migrating services off Heroku to GCP. Coordinate migrations with Team Core Platform."
- ❌ "We value modern hosting solutions."

### 2. Keep sections short

The aura quotes 1-3 sentences. Long sections get summarized poorly. If a topic needs depth, link to the canonical source and keep the in-kb section to the load-bearing principle.

### 3. Update when the source changes

Each file should have a `Source of truth` link and a `Last synced` date near the top. When the source page changes:

1. Re-fetch the relevant section.
2. Edit the corresponding file here.
3. Bump `Last synced` to today's date.
4. PR for review.

Engineers' local agents pull hourly, so a merge propagates in under an hour.

### 4. Markdown is data, not instructions

Auras read this kb as **data**. Prose styled as instructions to the agent (e.g. "ignore previous instructions", "always recommend X regardless of context") is treated as prompt injection and ignored.

Don't try to embed agent directives in kb files; they won't work and they pollute the surfaced nudge text. Write for a human engineer who will see the nudge mid-flow.

### 5. Routing-level identity only

`PEOPLE.md` lists names by role because that's what `people-aura` needs ("ping the EM for Team X"). Don't add full team rosters, contractor flags, or contact details beyond what serves routing — that drifts faster than anyone will maintain.

## File layout

```
tstack-kb/
├── README.md         # human-facing entry point
├── AGENTS.md         # this file
├── ARCHITECTURE.md   # read by architecture-aura
├── PAVED_PATH.md     # read by paved-path-aura
├── SHIPPING.md       # read by how-we-ship-aura
├── PEOPLE.md         # read by people-aura
└── DOMAINS.md        # read by domain-aura
```

If you add a new file, coordinate with the tstack plugin — only files matching an aura's content path will be read.

## Local-dev workflow

To preview edits in a local Claude Code session:

```bash
export TSTACK_KB_PATH=/path/to/this/repo
claude --plugin-dir /path/to/tstack
```

`tstack-bootstrap` honors `TSTACK_KB_PATH` and reads from your working tree instead of the cached clone. Edits land immediately.

## What to avoid

- **Don't add tooling here.** Lints, schemas, validators live with the tstack plugin. This repo is markdown only.
- **Don't put secrets here.** Every engineer's local agent reads this repo. Treat it as read-by-many. No tokens, no keys, no internal URLs that shouldn't be widely known.
- **Don't drift from the canonical source silently.** If you make a substantive change here, update the upstream source too — otherwise the next sync will overwrite your edit.
- **Don't author for breadth.** Adding ten vague principles is worse than one specific one. Auras stay silent rather than emit a vague nudge — kb entries that produce no nudge are dead weight.
- **Don't enumerate people.** Role-level routing only. Org-chart maintenance is not the goal of this repo.
