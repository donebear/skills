# Donebear Skills

Skill bundle for running the Done Bear CLI with discovery, auth, workspace, task, project, and import workflows.

## Install

Published bundle:

```bash
npx skills add donebear/skills -g --all -y
```

Manual copy:

```bash
cp -R skills/* ~/.claude/skills/
```

## Included Skills

This repo currently ships one skill.

| Skill      | What it does                                                                                                                              | Use when                                                                                                                                                                       |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `donebear` | Operates the Done Bear CLI for command discovery, auth and workspace preflight, task and project workflows, API keys, and Things imports. | The user asks to run `donebear`, inspect `donebear spec` or `donebear context`, or manage Done Bear tasks, projects, workspaces, auth, API keys, or imports from the terminal. |

## Maintenance

- Keep the README skill inventory in sync with `skills/*/SKILL.md`.
- When adding or removing a skill, update both `README.md` and `AGENTS.md`.

## License

[MIT](LICENSE.md)
