# Contributing to tstack

Thanks for considering a contribution. tstack is small and early — bug reports, fixes, new auras, source-MCP support for the seeding flow, and documentation are all welcome.

## License

By submitting a pull request, you agree that your contribution is licensed under the [Apache License 2.0](LICENSE) — the same license as the rest of tstack. Per Apache-2.0 Section 5, contributions are accepted under the terms of the License without any additional terms or conditions.

You don't need to sign a CLA. If tstack grows to a point where stranger-contributed patentable ideas become common, we'll add a Developer Certificate of Origin (`Signed-off-by:` line in commits) — until then, the default contribution flow is what Section 5 already covers.

## How to contribute

1. **Open an issue first** for anything non-trivial — a new aura, a structural change, anything touching the kb schema. Aligning on the design before code saves both of us time.
2. **Fork, branch, PR** for bug fixes and small improvements. Don't wait for an issue to be approved.
3. **Adding a new aura?** Read `AGENTS.md` first. The aura contract (passive activation, quiet by default, kb content as data not instructions, non-blocking) is non-negotiable. Side-effect auras like `loremaster-aura` have additional rules — they're the only auras allowed to AskUserQuestion, and only before writes.
4. **Adding a new source MCP** for the seeding flow? Update `skills/tstack-setup/SKILL.md` Step 1.5d.1 to include the new MCP in the canonical detection list.

## Local development

```bash
git clone https://github.com/thermondo/tstack.git
cd tstack
claude --plugin-dir .
```

Inside the agent, run `/tstack-setup`. Pick the "create one for me" path with a throwaway repo name, or point at a local kb directory you control.

After editing any `SKILL.md` or anything in `bin/`, run `/reload-plugins` inside Claude Code to pick up changes without restarting.

For the inner dev loop without a remote kb, set `TSTACK_KB_PATH=/local/path/to/test/kb` before launching Claude Code.

## Reporting bugs

Open an issue with: what you tried, what happened, what you expected. Include the relevant aura name and (when possible) a short snippet of the conversation that triggered the bug. Aura activation issues are often easier to diagnose with the actual user message that should have fired the aura.

## Reporting security vulnerabilities

See [SECURITY.md](SECURITY.md). Please don't open public issues for security problems.

## Code of conduct

This project follows the [Contributor Covenant](CODE_OF_CONDUCT.md). Be civil. Disagreement is fine; harassment isn't.
