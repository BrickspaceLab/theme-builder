# Adding support for a new theme

This page is for **theme partners** who want this skill to include your theme’s real block `type` strings and layout patterns. Put everything theme-specific under `[examples/](../examples/)` only—**do not** add concrete `type` strings or recipes to `[SKILL.md](../SKILL.md)`; that file stays theme-agnostic. Merchants’ agents still follow [Discover allowed block types](../SKILL.md) on the **workspace** theme and must match every `type` and setting to that theme’s `blocks/`, `sections/`, and `{% schema %}`.

## Steps

1. Create a folder `examples/<theme-key>/` using a short kebab-case name (for example `examples/acme-theme/`).
2. Add `examples/<theme-key>/README.md`. Include: how to recognize your theme in a repo (for example `theme_info` / `theme_name` in `config/settings_schema.json`); curated tables mapping **role → JSON `type` → `blocks/<type>.liquid`**; a few ASCII **structure trees** for common layouts; and a note that any bundled JSON is **illustrative**—keys and values must match real schemas in the target theme.
3. Optionally add illustrative `*.json` and image files in the **same** `examples/<theme-key>/` folder; link them from the README.
4. Use `[examples/slab/README.md](../examples/slab/README.md)` as a full example of layout and depth.
5. Add a row for your theme to the **Index** table in `[examples/README.md](../examples/README.md)`.
6. Open a pull request with a **scoped** change: your `examples/<theme-key>/` files and the index update. Use only block types that exist in your theme’s real Liquid/schema surface.

**See also:** [examples/README.md](../examples/README.md) (how agents pick a bundled example), `[reference/shopify.md](../reference/shopify.md)` (filenames and platform limits).