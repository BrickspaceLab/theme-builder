# theme builder

Theme Builder is a skill for **designers and developers** who work in **Shopify Online Store 2.0** themes and want the agent to ship real structure—not invented block names or settings that do not exist in the theme you are editing.

It covers **JSON templates** (`templates/*.json`), **theme blocks** and section JSON, **Liquid `{% schema %}`** alignment, **discovery-first** workflows so types always match `blocks/` and `sections/` on disk, **design mockups** (what to infer vs what to verify in schema), and optional **bundled examples** for themes documented under [`examples/`](examples/).

Install:

```bash
npx skills add BrickspaceLab/theme-agent
```

Use it **on a case-by-case basis** rather than everywhere. Good moments: editing or adding a JSON template, wiring nested blocks to match a layout, reconciling a Figma or screenshot with what your theme actually exposes, or reviewing template JSON before commit.

This skill is the **focused layer** on top of Shopify’s platform. For filenames, limits, and official docs links, see [`reference/shopify-json.md`](reference/shopify-json.md) and the [Shopify theme docs](https://shopify.dev/docs/storefronts/themes).

Repository: [github.com/BrickspaceLab/theme-agent](https://github.com/BrickspaceLab/theme-agent). Folder name when cloning: `theme-agent`.

---

### Contents

| File | Purpose |
|------|---------|
| [SKILL.md](SKILL.md) | Instructions for AI agents (YAML frontmatter + workflow) |
| [reference/shopify-json.md](reference/shopify-json.md) | Concepts, template filenames, links to Shopify documentation |
| [examples/](examples/) | Theme-specific patterns and index ([examples/README.md](examples/README.md)) |

---

### Scope

- **In scope:** JSON templates under `templates/`, section `type` / block `type` strings that exist in **that** theme, settings keys defined in section and block schemas, blocks under `blocks/` when the theme uses them.
- **Out of scope:** **Writing or editing custom Liquid** (sections, blocks, snippets, layouts—anything outside assembling JSON and reading `{% schema %}` for types and settings). Also out of scope: store credentials, `config/settings_data.json` theme-setup flows (unless you extend the skill), Checkout UI extensions, Theme app extension bundles.
