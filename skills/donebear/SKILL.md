---
name: donebear
description: Operates the Done Bear CLI for command discovery, auth and workspace preflight, task and project workflows, and automation-friendly JSON execution. Validates flags against live help when `spec` drifts and uses safe patterns for state-changing commands. Use when the user asks to run `donebear`, manage Done Bear tasks, projects, workspaces, API keys, or Things imports, inspect `donebear spec` or `donebear context`, or automate Done Bear from the terminal.
---

# Done Bear

Operate `donebear` with a discovery -> preflight -> execute -> validate workflow.

## Reference Files

| File | Read when |
|------|-----------|
| `references/preflight-and-safety.md` | Before the first command, when auth or workspace state is unclear, or before destructive writes |
| `references/core-recipes.md` | Running common auth, workspace, task, project, search, context, API key, or import flows |
| `references/live-validation.md` | `spec`, `--help`, and runtime behavior disagree, or a command fails unexpectedly |

## Intent Map

| Intent | Commands | Notes |
|--------|----------|-------|
| Discover the CLI | `donebear spec`, `donebear spec <command> [subcommand]` | No auth required |
| Orient the current account | `donebear auth status`, `donebear whoami`, `donebear workspace list`, `donebear context` | `context --markdown` is prompt-ready |
| Read and triage work | `task list`, `task show`, `task read`, `today`, `search`, `history` | Prefer `--json` for parsing |
| Change state | `task add/edit/done`, `project add/edit/done`, `workspace use`, `checklist ...` | Validate ids, workspace, and post-state |
| Risky operations | `auth logout`, `task archive`, `project archive`, `api-key revoke`, `import things` without `--dry-run` | Require explicit user intent |

## Safety Tiers

- `GREEN — execute directly:` `spec`, `auth status`, `whoami`, `workspace list/current`, `context`, `task list/show/read/random`, `today`, `search`, `project list/show`, `label list/show`, `team list/show`, `history`, `api-key list`.
- `YELLOW — announce then execute:` `auth login`, `workspace use/create/join/invite`, `task add/edit/append/prepend/done/reopen/checklist`, `project add/edit/done/unarchive`, `api-key create`, `import things --dry-run`.
- `RED — explicit confirmation required:` `auth logout`, `task archive`, `project archive`, `api-key revoke`, `import things` without `--dry-run`.
- Do not rely on CLI prompts for safety in agent mode. `--json`, `--dry-run`, `--yes`, and non-TTY execution can bypass confirmations.

## Workflow

Copy this checklist to track progress:

```text
Donebear progress:
- [ ] Step 1: Discover the exact command surface
- [ ] Step 2: Preflight auth, token source, and workspace scope
- [ ] Step 3: Execute the smallest safe command
- [ ] Step 4: Validate the resulting state
```

### Step 1: Discover the exact command surface

- Start with `donebear spec` or `donebear spec <command> [subcommand]`.
- For flag-sensitive or less common paths, also run `donebear <command> --help` before execution.
- Prefer explicit subcommands over the interactive prompt. `donebear` with no args on a TTY enters interactive mode, which is harder to script and validate.
- Prefer `--json` for agent parsing. Add `--jq`, command-local `--limit`, `--format yaml|csv|tsv`, or `--copy` only when needed.
- If `donebear` is not on `PATH` but the monorepo is present, use `npm exec --workspace=packages/cli donebear -- <args>` from the repo root.

### Step 2: Preflight auth, token source, and workspace scope

- Load `references/preflight-and-safety.md`.
- Check `donebear auth status --json`, then `donebear whoami --json` before anything stateful.
- Resolve workspace state with `donebear workspace current --json` or `donebear workspace list --json`.
- If multiple workspaces exist, either set one with `donebear workspace use <id-or-slug>` or prefix the command as `donebear workspace=<slug> ...`.
- Use `donebear context --json` or `donebear context --markdown` before multi-step task or project work.

### Step 3: Execute the smallest safe command

- Read-only requests can execute directly once auth and workspace are known.
- For writes, use the narrowest mutation that satisfies the request, and prefer `--dry-run` when the command supports it.
- Use full ids or stable keys whenever possible. Task prefixes must be unique and at least 4 characters long.
- `task list` does not currently support a working `--view` flag. Use `donebear today` for today's tasks and `search` or `state` filters for other triage paths.
- Load `references/core-recipes.md` for concrete command flows.

### Step 4: Validate the resulting state

- After auth changes, rerun `donebear auth status --json` and `donebear whoami --json`.
- After workspace changes, rerun `donebear workspace current --json` or `donebear context --json`.
- After task or project mutations, rerun `show`, `list`, `today`, or `context --json`.
- If `spec`, `--help`, and runtime disagree, trust runtime behavior and then load `references/live-validation.md`.

## Anti-patterns

- Treating `donebear spec` as the only source of truth for flags and examples.
- Assuming a default workspace exists when multiple workspaces are available.
- Using `--json` on destructive commands without explicit user approval.
- Running `import things` without a dry run first.
- Relying on ambiguous id prefixes when a full id or key is available.
- Launching interactive mode when an explicit command is easier to verify.
