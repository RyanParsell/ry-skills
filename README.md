# ry-skills

My personal collection of Claude Code agent skills. Originally forked from [mattpocock/skills](https://github.com/mattpocock/skills); now diverged enough that I treat it as its own thing.

This is a personal repo — public so I can install from any machine via `npx`, not because I'm trying to promote it. If you're on my machine and find something useful here, take it.

## Install

The repo follows the `SKILL.md` + `.claude-plugin/plugin.json` convention used by the [Vercel Labs `skills` CLI](https://github.com/vercel-labs/skills), so there are two ways to consume it.

### On an ad-hoc machine (VM, fresh box, somebody else's computer)

```
npx skills@latest add RyanParsell/ry-skills
```

The CLI prompts you to pick which skills to install. Run it again later to add more.

To install a single skill non-interactively:

```
npx skills@latest add RyanParsell/ry-skills/engineering/tdd
```

### On a daily-driver workstation (where I edit skills too)

Clone the repo somewhere stable, then symlink every skill into `~/.claude/skills/`:

```
git clone https://github.com/RyanParsell/ry-skills.git
cd ry-skills
./scripts/link-skills.sh
```

The link script is bash. On Windows use Git Bash, or run it from WSL. After this, editing a skill in the repo is the same as editing the one Claude Code sees — no reinstall step.

Don't mix the two modes on the same machine; pick one and stick with it.

## Reference

### Engineering

Skills I use daily for code work.

- **[diagnose](./skills/engineering/diagnose/SKILL.md)** — Disciplined diagnosis loop for hard bugs and performance regressions: reproduce → minimise → hypothesise → instrument → fix → regression-test.
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)** — Grilling session that challenges your plan against the existing domain model, sharpens terminology, and updates `CONTEXT.md` and ADRs inline.
- **[triage](./skills/engineering/triage/SKILL.md)** — Triage issues through a state machine of triage roles.
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)** — Find deepening opportunities in a codebase, informed by the domain language in `CONTEXT.md` and the decisions in `docs/adr/`.
- **[setup-ry-skills](./skills/engineering/setup-ry-skills/SKILL.md)** — Scaffold the per-repo config (issue tracker, triage label vocabulary, domain doc layout) that the other engineering skills consume. Run once per repo before using `to-issues`, `to-prd`, `triage`, `diagnose`, `tdd`, `improve-codebase-architecture`, or `zoom-out`.
- **[tdd](./skills/engineering/tdd/SKILL.md)** — Test-driven development with a red-green-refactor loop. Builds features or fixes bugs one vertical slice at a time.
- **[to-issues](./skills/engineering/to-issues/SKILL.md)** — Break any plan, spec, or PRD into independently-grabbable issues using vertical slices.
- **[to-prd](./skills/engineering/to-prd/SKILL.md)** — Turn the current conversation context into a PRD and submit it as a tracker issue. No interview — just synthesizes what you've already discussed.
- **[zoom-out](./skills/engineering/zoom-out/SKILL.md)** — Tell the agent to zoom out and give broader context or a higher-level perspective on an unfamiliar section of code.

### Productivity

General workflow tools, not code-specific.

- **[caveman](./skills/productivity/caveman/SKILL.md)** — Ultra-compressed communication mode. Cuts token usage ~75% by dropping filler while keeping full technical accuracy.
- **[grill-me](./skills/productivity/grill-me/SKILL.md)** — Get relentlessly interviewed about a plan or design until every branch of the decision tree is resolved.
- **[write-a-skill](./skills/productivity/write-a-skill/SKILL.md)** — Create new skills with proper structure, progressive disclosure, and bundled resources.

### Misc

Tools I keep around but rarely use.

- **[git-guardrails-claude-code](./skills/misc/git-guardrails-claude-code/SKILL.md)** — Set up Claude Code hooks to block dangerous git commands (push, reset --hard, clean, etc.) before they execute.

## License

MIT. Built on top of MIT-licensed work by [Matt Pocock](https://github.com/mattpocock/skills) — see `LICENSE`.
