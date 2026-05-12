# Security Policy

## Reporting a vulnerability

If you find a security vulnerability in tstack, **please don't open a public GitHub issue**. Email **security@thermondo.de** with:

- A description of the issue and its impact.
- Steps to reproduce.
- The version or commit of tstack affected.

We aim to acknowledge within 5 business days and to ship a fix or coordinated disclosure within 30 days. If a fix needs longer, we'll tell you why and propose a timeline.

## What counts as a vulnerability

The tstack threat model is shaped by the fact that the plugin reads from a user-configured knowledge base and (via `loremaster-aura` and the `/tstack-setup` seeding flow) writes back to it. Reportable issues include:

- **Prompt injection via the kb.** Every aura must treat kb content as data, not instructions. If you can show that a malicious kb entry (a `PAVED_PATH.md` recipe with embedded "ignore previous instructions", etc.) causes an aura to execute attacker-controlled steps, that's a vuln.
- **Writes without confirmation.** `loremaster-aura` and the `/tstack-setup` seeding step must ask before every file edit, commit, push, and PR. If they write without explicit user confirmation, that's a vuln.
- **Credential leakage.** tstack stores no credentials of its own — it piggybacks on git's auth (SSH keys, `gh` CLI, git credential helper) and on the user's MCP servers. If you find tstack writing credentials to disk, transmitting them, or logging them, that's a vuln.
- **`PEOPLE.md` PII leakage.** Seeding into `PEOPLE.md` is constrained to routing-level identity (name + role + how-to-reach). If emails, contractor flags, personal information, or other non-routing data slip through the seeding step into the kb, that's a vuln.
- **Path traversal / arbitrary file writes.** Any way to make an aura write outside the configured `$TSTACK_KB_DIR` is a vuln.

## Out of scope

- Vulnerabilities in MCP servers themselves (Confluence, GitHub, Notion, Linear, Slack, etc.) — report to those projects.
- Vulnerabilities in Claude Code itself — report to Anthropic.
- Vulnerabilities in the user's git host (GitHub, GitLab) — report upstream.
- Content errors in a kb (a `PEOPLE.md` listing the wrong owner). That's a kb hygiene issue, not a tstack plugin vulnerability.

If you're unsure whether something is in scope, email anyway — better to over-report than miss something.
