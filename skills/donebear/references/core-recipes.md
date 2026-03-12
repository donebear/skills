# Core Recipes

## Discovery And Orientation

```bash
donebear spec
donebear spec task add
donebear context --json
donebear context --markdown
```

- Use `spec` to discover commands and JSON shapes.
- Use `context` after auth and workspace are valid to see user, workspace, task counts, recent tasks, and active projects.

## Auth And Identity

```bash
donebear auth status --json
donebear whoami --json
donebear auth login
donebear auth login --no-open
donebear auth logout
```

- `auth` with no subcommand runs status.
- Use `whoami` after `auth status` to confirm the token really works.
- Announce the browser handoff before `auth login`.

## Workspace Selection

```bash
donebear workspace list --json
donebear workspace current --json
donebear workspace use personal
donebear workspace=personal task list --json
donebear workspace members personal --format csv
```

- Use the prefix form when the user wants a one-off command without changing the persisted default.
- `workspace invite` returns a reusable invite link, and `--copy` can place it on the clipboard.

## Task Triage

```bash
donebear task list --json
donebear task show 8f2c --json
donebear task read 8f2c
donebear search "invoice" --json
donebear today --json
```

- Use `today` instead of `task list --view today`.
- `task read` prints raw notes in text mode and is useful when the user wants just the description.

## Task Mutations

```bash
donebear task add "Ship donebear skill" --when today --project CLI --json
donebear task edit 8f2c --deadline 2026-03-20 --json
donebear task append 8f2c "Added rollout notes" --json
donebear task done 8f2c --json
donebear task checklist 8f2c
donebear task checklist add 8f2c "Write README updates" --json
```

- Prefer full ids or stable keys when available.
- `task archive` is confirmation-sensitive; do not hide it inside JSON mode without explicit user approval.

## Projects, Labels, Teams, History, And API Keys

```bash
donebear project list --json
donebear project add "CLI polish" --target-date 2026-03-31 --json
donebear label list --json
donebear team show eng --json
donebear history --model Task --limit 20 --json
donebear api-key create "CI"
donebear api-key list --json
```

- `api-key create` reveals the token once. Surface it carefully and assume it is sensitive.
- `api-key revoke` is destructive and should be treated like a red-tier action.

## Things Import

```bash
donebear import things --dry-run --json
donebear import things --database-url ~/Backups/Things\ Database.thingsdatabase --json
donebear import things --database-url file:///Users/me/Backups/main.sqlite --workspace personal
```

- Always run a dry run first.
- Use `--metadata-mode note|none` and `--skip-teams` only when the user asks for that behavior.
- The importer accepts a copied `.thingsdatabase` bundle, a direct `main.sqlite`, or a `ThingsData-*` directory.
