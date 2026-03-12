# Repository Contract

## Scope

- This repo is a skill bundle. The local source of truth is `skills/*/SKILL.md`.
- The current local bundle contains only `skills/donebear/SKILL.md`.
- Keep edits scoped to the files requested. Do not revert unrelated work in the repo.

## Discover First

Run these before changing bundle docs:

```bash
git status --short README.md AGENTS.md
find skills -mindepth 2 -maxdepth 2 -name 'SKILL.md' | sort
sed -n '1,120p' skills/donebear/SKILL.md
```

## Editing Rules

- `README.md` is install-facing. Keep it short and accurate to the local bundle.
- `AGENTS.md` is an execution contract. Keep only rules that change agent behavior here.
- Do not document skills that are not present under `skills/*/SKILL.md`.
- README skill inventory must match `skills/*/SKILL.md`.
- If a skill uses reference files, link every reference from `SKILL.md`. Unlinked files will not load.

## Install Paths

Published bundle:

```bash
npx skills add mblode/agent-skills
```

Manual copy:

```bash
cp -R skills/* ~/.claude/skills/
```

## Validation

- There is no automated test suite in this repo.
- Smoke-check doc accuracy with:

```bash
find skills -mindepth 2 -maxdepth 2 -name 'SKILL.md' | sort
rg -n 'mblode/agent-skills|~/.claude/skills/|skills/\\*/SKILL.md|skills/donebear/SKILL.md' README.md AGENTS.md
```

## Repo Gotchas

- The published install slug is `mblode/agent-skills`. Do not reintroduce older slugs.
- If the skill list changes, update `README.md` in the same change as the skill addition or removal.
