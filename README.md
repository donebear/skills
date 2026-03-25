# Donebear Skills

Skill bundle for Done Bear CLI workflows and automated npm releases via changesets.

## Quick Start

```bash
npx skills add donebear/skills -g --all -y
```

Supports OpenCode, Claude Code, Codex, and Cursor.

## Skills

| Skill      | What it does                                                                                                                              | Use when                                                                                                                                                                       |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `donebear` | Operates the Done Bear CLI for command discovery, auth and workspace preflight, task and project workflows, API keys, and Things imports. | The user asks to run `donebear`, inspect `donebear spec` or `donebear context`, or manage Done Bear tasks, projects, workspaces, auth, API keys, or imports from the terminal. |
| `autoship` | Automates npm release workflows: creates changesets, writes changelogs, fixes quality issues, pushes, monitors CI via scheduled tasks, merges the Version Packages PR, and watches npm-publish to completion. | The user asks to ship, release, publish, or autoship an npm package using a changesets workflow. |

## License

[MIT](LICENSE.md)
