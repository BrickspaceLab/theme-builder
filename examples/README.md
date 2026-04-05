# Theme-specific examples

**For theme authors:** how to add a new bundled theme family is documented in [docs/adding-theme-support.md](../docs/adding-theme-support.md).

All **named-theme** vocabulary—concrete block `type` strings, nested layouts, and theme-specific setting keys—lives **only** in files under this folder. [SKILL.md](../SKILL.md) stays theme-agnostic; it tells you how to discover blocks on disk and **how to pick which bundled example to open**, not which theme you are using.

## Choosing the best example to reference

Do this **after** [Discover allowed block types](../SKILL.md) on the workspace theme so you know which `type` strings actually exist.

1. **Identify the theme** — Read `config/settings_schema.json` and check the `theme_info` object for `theme_name`. This is the most reliable way to identify the theme without asking the user.
2. **Open the best-matching file** only when the overlap is strong (same naming style and types). Use it for layout ideas and sample JSON **after** confirming every `type` still appears in the target theme’s schemas.
3. If **no** bundled example fits, **do not** use one—copy structure from existing `templates/*.json` in that theme instead.
4. If multiple could fit or the theme is unknown, **ask the user** which theme or example family to follow.

## Index


| File | Documents |
| --- | --- |
| [slab/README.md](slab/README.md) | Slab / Brickspace-style blocks: essential **block reference**, curated layout/content/cards reference, structure trees, and concrete `*.json` (including one full **section** example). Full inventory: theme `blocks/` or `scripts/generate_slab_block_reference.py` (stdout). |


Add new rows here when you add `examples/<theme>.md` files.