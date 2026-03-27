# Theme-specific examples (bundled knowledge)

All **named-theme** vocabulary—concrete block `type` strings, nested layouts, and theme-specific setting keys—lives **only** in files under this folder. [SKILL.md](../SKILL.md) stays theme-agnostic; it tells you how to discover blocks on disk and **how to pick which bundled example to open**, not which theme you are using.

## Choosing the best example to reference

Do this **after** [Discover allowed block types](../SKILL.md) on the workspace theme so you know which `type` strings actually exist.

1. **Read the index** below and each file’s title/intro to see which theme or theme family it documents.
2. **Match the workspace theme** using any of: theme `README`, package or vendor name in `config/`, the user’s prompt, or **overlap** between basenames in `blocks/*.liquid` and the block types described in an example file.
3. **Open the best-matching file** only when the overlap is strong (same naming style and types). Use it for layout ideas and sample JSON **after** confirming every `type` still appears in the target theme’s schemas.
4. If **no** bundled example fits, **do not** use one—copy structure from existing `templates/*.json` in that theme instead.
5. If multiple could fit or the theme is unknown, **ask the user** which theme or example family to follow.

## Index

| File | Documents |
|------|-----------|
| [slab-theme.md](slab-theme.md) | Slab / Brickspace-style blocks (`layout__grid`, `_g__grid-item`, etc.) |

Add new rows here when you add `examples/<theme>.md` files.
