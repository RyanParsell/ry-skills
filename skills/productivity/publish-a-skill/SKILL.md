---
name: publish-a-skill
description: Wire a newly-authored skill into plugin.json, the top-level README, and the bucket README, then commit and push. Use when the user has a new skill directory under skills/ and wants to ship it, or says "publish this skill", "ship this skill", "wire up this skill". First-time publish only — not for renames or moves between buckets.
---

# Publishing a Skill

Companion to [write-a-skill](../write-a-skill/SKILL.md). That skill *authors and improves* the skill (vendored Anthropic `skill-creator`); this one *ships* it to the repo's three sync points.

## Scope

- **In scope**: first-time publish of a skill directory that already exists on disk.
- **Out of scope**: renames, moves between buckets, deletions, version bumps. Do those by hand.

## Process

1. **Identify the skill.** Take it from `$ARGUMENTS` if given (e.g. `engineering/my-skill`), otherwise run `git status` and pick the untracked `skills/<bucket>/<slug>/` directory. If multiple candidates, ask the user which one.

2. **Validate the skill file.** Read `skills/<bucket>/<slug>/SKILL.md`:
   - Frontmatter `name:` matches `<slug>` (the directory name).
   - Frontmatter `description:` exists and contains "Use when" (or equivalent trigger language).
   - File is under ~100 lines, or splits into sibling `.md` files. If not, suggest fixing via `write-a-skill` before publishing.

3. **Check `.out-of-scope/`.** Grep each file in `.out-of-scope/` for keywords from the skill's description. If anything matches, surface the rationale to the user and confirm they still want to publish.

4. **Determine the bucket.** From the path: `engineering`, `productivity`, `misc`, or `personal`.

5. **If `personal/`**: stop after step 7. Do not touch `plugin.json` or the top-level `README.md`. `personal/` skills are never shipped.

6. **Otherwise wire all three sync points** (per `CLAUDE.md`):

   - `.claude-plugin/plugin.json` — add `"./skills/<bucket>/<slug>"` to the `skills` array. Keep ordering grouped by bucket; alphabetical within a bucket is preferred but not required.
   - Top-level `README.md` — add a bullet under the matching `### <Bucket>` heading in the `## Reference` section:
     ```
     - **[<slug>](./skills/<bucket>/<slug>/SKILL.md)** — <one-line summary, ending with a period>.
     ```
   - `skills/<bucket>/README.md` — add a bullet with the same summary but a relative path:
     ```
     - **[<slug>](./<slug>/SKILL.md)** — <one-line summary>.
     ```

   Derive the one-line summary from the skill's `description:` field — strip the "Use when…" trigger sentence; keep only the capability description. If the result is awkward, ask the user for a one-liner.

7. **Sanity-check.** Run `scripts/list-skills.sh` (bash) and confirm the new skill appears. Diff `plugin.json` against the output mentally — every public skill on disk should be in the manifest.

8. **Stage and commit.** Stage `SKILL.md` (and any sibling files), `plugin.json`, both READMEs. Commit message convention from `git log`:
   ```
   Add <slug> skill
   ```
   or `Add <slug> skill under <bucket>` if disambiguation helps.

9. **Confirm before pushing.** Show the user the staged diff and the commit message. Ask explicitly: "Push to `origin main`?" Default is **no**. Only `git push` after explicit confirmation. If the user passed `--yes` / `-y` in `$ARGUMENTS`, skip the confirmation.

## Hard-dependency pointer (ADR 0001)

If the new skill is one of `to-issues`, `to-prd`, `triage`, or another skill that hard-depends on `/setup-ry-skills`, verify the `SKILL.md` includes the explicit setup pointer. See `docs/adr/0001-explicit-setup-pointer-only-for-hard-dependencies.md`. Don't add the pointer to soft-dependency skills.

## Failure modes to watch for

- **Bucket mismatch**: `name:` says one thing, directory says another. Stop and ask.
- **Already published**: skill is already in `plugin.json`. Stop — this skill is first-time-publish only.
- **No description trigger**: missing "Use when". Send back to `write-a-skill`.
- **Push rejected**: don't `--force`. Surface the error and let the user resolve.
