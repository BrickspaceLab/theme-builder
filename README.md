# Theme Agent

Agent skill for authoring **Shopify Online Store 2.0** JSON templates (`templates/*.json`) and block-based theme JSON: discover section and block types from the theme on disk, align settings with Liquid `{% schema %}`, and emit valid template JSON.

Repository folder name: `theme-agent` (clone or copy this repo into a folder with that name locally if you prefer).

## Contents

| File | Purpose |
|------|---------|
| [SKILL.md](SKILL.md) | Instructions for AI agents (YAML frontmatter + workflow) |
| [reference/block-reference.md](reference/block-reference.md) | Block types under `blocks/` (layout, content, wrappers) |
| [reference/shopify-json.md](reference/shopify-json.md) | Concepts, template filenames, links to Shopify documentation |
| [examples/minimal-templates.md](examples/minimal-templates.md) | Generic JSON examples (illustrative section types) |

## Install

### Cursor

Copy or clone this repository into your project or global skills folder:

- Project: `<project>/.cursor/skills/theme-agent/`
- Or follow [Cursor Agent Skills](https://cursor.com/docs) for the path your setup expects.

Ensure `SKILL.md` lives at `.../theme-agent/SKILL.md`.

### Codex / other agents

Many tools expect `SKILL.md` under a named folder, for example:

- `./.agents/skills/theme-agent/`

### LobeHub marketplace

If this skill is published there, install with the marketplace CLI (see [LobeHub Skills](https://lobehub.com/skills)) and your agent’s `--agent` flag, for example:

```bash
npx -y @lobehub/market-cli skills install <owner-repo> --agent cursor
```

Replace `<owner-repo>` with the marketplace identifier once published.

## Scope

- **In scope:** JSON templates under `templates/`, section `type` / block `type` strings that exist in **that** theme, settings keys defined in section and block schemas, blocks under `blocks/` when the theme uses them.
- **Out of scope:** Store credentials, `config/settings_data.json` theme-setup flows (unless you extend the skill), Checkout UI extensions, Theme app extension bundles.

## Publishing

1. Create a new **public** repository on GitHub (e.g. `theme-agent`) and push this tree:

   ```bash
   git remote add origin https://github.com/<you>/<repo>.git
   git push -u origin main
   git push origin v1.0.0
   ```

2. **LobeHub** or other marketplaces: follow their contributor flow (often linking the GitHub repo and using `@lobehub/market-cli` or the site UI). This repo may be tagged at `v1.0.0` for versioned installs.

## License

[MIT](LICENSE)
