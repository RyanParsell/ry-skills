# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A Claude Code plugin that ships a collection of agent skills (slash commands + behaviors). Distributed via `npx skills@latest add RyanParsell/ry-skills` and consumed by per-repo config seeded by `/setup-ry-skills`. No build, no tests, no runtime — the deliverable is the skill files themselves and the plugin manifest. The repo also supports a clone + `scripts/link-skills.sh` workflow for daily-driver workstations where skills are edited in place.

## Skill layout and shipping rules

Skills live under `skills/` in five bucket folders:

- `engineering/` — daily code work
- `productivity/` — daily non-code workflow tools
- `misc/` — kept around but rarely used
- `personal/` — tied to my own setup, not promoted

Every skill in `engineering/`, `productivity/`, or `misc/` must appear in **all three** places, kept in sync:

1. `.claude-plugin/plugin.json` — the plugin manifest. A skill not listed here is not shipped to users, no matter what the READMEs say.
2. The top-level `README.md` reference section, with the skill name linked to its `SKILL.md`.
3. The bucket's own `README.md`, one-line description, linked to `SKILL.md`.

Skills in `personal/` must **not** appear in `plugin.json` or the top-level `README.md`. When adding, renaming, or moving a skill, update all three places in the same change.

## Skill file shape

Each skill is a directory containing a `SKILL.md` with YAML frontmatter:

```yaml
---
name: <kebab-case-slug>           # matches the directory name
description: <one-line trigger>   # tells the model when to invoke
---
```

Supporting `.md` files (e.g. `tdd/tests.md`, `tdd/mocking.md`) implement **progressive disclosure** — `SKILL.md` stays short and links out to them. The `write-a-skill` skill is the canonical reference for authoring conventions.

## Hard- vs soft-dependency on setup

See `docs/adr/0001-explicit-setup-pointer-only-for-hard-dependencies.md`. Engineering skills split into:

- **Hard dependency** on `/setup-ry-skills` (`to-issues`, `to-prd`, `triage`) — must include an explicit pointer telling the user to run setup if the per-repo config is missing.
- **Soft dependency** (`diagnose`, `tdd`, `improve-codebase-architecture`, `zoom-out`) — reference "the project's domain glossary" and ADRs in vague prose only; degrade gracefully without setup.

Don't cargo-cult the setup pointer into soft-dependency skills.

## CONTEXT.md is load-bearing

`CONTEXT.md` defines the domain vocabulary used across skills (**Issue tracker**, **Issue**, **Triage role**). When editing skill prompts, use these exact terms — they're what the skills themselves teach downstream repos to use. Changes to terminology in `CONTEXT.md` cascade into every skill prompt.

## `.out-of-scope/` — explicitly rejected features

Before designing anything that smells like "let me add a check mode / question limit / new issue-tracker backend," grep `.out-of-scope/` first. Each file documents a previously-rejected request, the reasoning, and prior issue numbers. If the user's ask matches one, surface the rationale instead of re-litigating it silently.

## Scripts

- `scripts/list-skills.sh` — prints every `SKILL.md` path in the repo. Useful for auditing manifest/README sync.
- `scripts/link-skills.sh` — symlinks every skill into `~/.claude/skills/` for local development. Has a guard against the destination being a symlink back into this repo (which would write per-skill symlinks into the working tree).

Both are bash, not PowerShell.
