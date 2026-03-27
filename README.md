# Theme Builder

Agent skill for authoring **Shopify Online Store 2.0** JSON templates (`templates/*.json`) and block-based theme JSON: discover section and block types from the theme on disk, align settings with Liquid `{% schema %}`, and emit valid template JSON.

Repository: [github.com/BrickspaceLab/theme-agent](https://github.com/BrickspaceLab/theme-agent). Folder name when cloning: `theme-agent`.

## Contents

| File | Purpose |
|------|---------|
| [SKILL.md](SKILL.md) | Instructions for AI agents (YAML frontmatter + workflow) |
| [reference/shopify-json.md](reference/shopify-json.md) | Concepts, template filenames, links to Shopify documentation |
| [examples/](examples/) | Theme-specific patterns and index ([examples/README.md](examples/README.md)) |

## Install

### Via `npx skills` ([skills.sh](https://skills.sh/))

This skill is a standard [Agent Skill](https://agentskills.io) with `SKILL.md` at the repo root, so it installs with the open [`skills` CLI](https://github.com/vercel-labs/skills):

```bash
npx skills add BrickspaceLab/theme-agent
```

**Examples:**

- List what will be installed without installing yet:

  ```bash
  npx skills add BrickspaceLab/theme-agent --list
  ```

- Install for **Cursor** only (non-interactive):

  ```bash
  npx skills add BrickspaceLab/theme-agent -a cursor -y
  ```

- Install **globally** (`-g`) so the skill is available in every project; the CLI prints the destination path (for Cursor, often under `~/.agents/skills/` or `~/.cursor/skills/` per your [skills CLI](https://github.com/vercel-labs/skills) version):

  ```bash
  npx skills add BrickspaceLab/theme-agent -g -a cursor -y
  ```

The CLI copies or symlinks the skill folder; `reference/` and `examples/` are included alongside `SKILL.md`.

Discoverability on the [skills.sh leaderboard](https://skills.sh/) comes from anonymous install telemetry from the CLI, not a separate signup ([docs](https://skills.sh/docs)). Opt out of telemetry: `DISABLE_TELEMETRY=1`.

### Manual install (Cursor)

Copy or clone this repository into your project or global skills folder:

- Project: `<project>/.cursor/skills/theme-agent/`
- Or follow [Cursor Agent Skills](https://cursor.com/docs) for the path your setup expects.

Ensure `SKILL.md` lives at `.../theme-agent/SKILL.md`.

### Codex / other agents

Many tools expect `SKILL.md` under a named folder, for example:

- `./.agents/skills/theme-agent/`

The `npx skills add` command can target [many agents](https://github.com/vercel-labs/skills#supported-agents); use `-a <agent>` to choose.

## Scope

- **In scope:** JSON templates under `templates/`, section `type` / block `type` strings that exist in **that** theme, settings keys defined in section and block schemas, blocks under `blocks/` when the theme uses them.
- **Out of scope:** Store credentials, `config/settings_data.json` theme-setup flows (unless you extend the skill), Checkout UI extensions, Theme app extension bundles.

## Publishing

1. **Clone / push** (for maintainers or forks):

   ```bash
   git clone https://github.com/BrickspaceLab/theme-agent.git
   # or
   git remote add origin https://github.com/BrickspaceLab/theme-agent.git
   git push -u origin main
   git tag v1.0.0
   git push origin v1.0.0
   ```

   Public install: `npx skills add BrickspaceLab/theme-agent`.

## License

[MIT](LICENSE)
