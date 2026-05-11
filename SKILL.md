---
name: theme-skill
description: Edit Shopify theme JSON templates, settings data, and settings schema using the target theme's existing sections, blocks, nested block structures, and schema-defined settings. Use when updating, editing, or tweaking Shopify `templates/*.json`, JSON section data, `config/settings_data.json`, `config/settings_schema.json`, section settings, or block settings. Do not use for custom Liquid, new theme sections or blocks, snippets, assets, JavaScript, CSS, checkout extensions, or non-JSON theme-code work.
---

## Overview

Use this skill to update Shopify themes through **JSON-only** changes. Supported files include `templates/*.json`, JSON section data, `config/settings_data.json`, `config/settings_schema.json`, and schema-defined section or block settings inside JSON files.

Treat existing Liquid schemas as read-only references. Do not write, create, or modify custom Liquid, and do not add new files under `sections/`, `blocks/`, `snippets/`, `assets/`, or other theme-code paths.

Use only **existing** sections, blocks, nested block structures, and settings already provided by the target theme. If the requested design cannot be represented with the theme's existing JSON templates, section types, block types, and schema settings, state the gap and propose the closest JSON-only result.

## When to use

Use this skill when the task involves:

- Shopify **JSON templates** under `templates/`, such as `product.json`, `index.json`, or alternate templates like `page.contact.json`
- Section JSON that references existing theme blocks from `blocks/` in block-based themes
- Theme design settings that can be safely expressed through `config/settings_data.json`, `config/settings_schema.json`, section settings, or block settings
- Global setting schema updates in `config/settings_schema.json`, when the requested control belongs in Shopify's JSON settings schema
- Layout, content, color, typography, spacing, visibility, or behavior tweaks that are already exposed by the theme's JSON settings and schemas
- Translating a screenshot, design mockup, or written request into the closest JSON-only structure supported by the target theme

## When not to use

Do not use this skill for:

- Checkout branding or Checkout UI extensions
- Theme app extension code under `extensions/`
- Creating or editing **Liquid** `.liquid` files, including new sections, blocks, snippets, schemas, or theme assets
- Replacing **Liquid** `.liquid` templates where the theme has not adopted JSON for that route
- Creating new section types, block types, snippets, JavaScript, stylesheets, assets, or Liquid schema to make a design possible

If the theme uses a **build step** that generates JSON templates, settings data, or settings schema, read that theme’s README and edit the JSON/settings source of truth. If the source of truth is Liquid, JavaScript, CSS, or non-JSON theme code, report that the task is outside this skill.

## Quick start

1. **Understand requirements** — Parse prompts or images for layout, columns/rows, content types, and styling (see **Design images and mockups** when the input is visual).
2. **Stay JSON-only** — Treat existing Liquid schemas as read-only references. Edit Shopify JSON only, including `config/settings_schema.json` when global setting schema changes are requested.
3. **Discover allowed block types** — List `blocks/` and read parent `{% schema %}` so every `type` you use exists in this theme (required; see below).
4. **(Optional) Check bundled examples** — If `examples/` is present, follow **Choosing the best example to reference** in [examples/README.md](examples/README.md): compare the workspace theme to each indexed example and open at most one best-matched file. If nothing fits, copy patterns from existing `templates/*.json` in the workspace theme instead. Never emit `type` strings from a bundled example until they appear in the target theme’s allowed set.
5. **Map to real blocks** — Choose types only from that allowed set; resolve section `type` from existing `sections/*.liquid` for template-level JSON.
6. **Analyze schemas** — Read each existing block or section `{% schema %}` for allowed children, setting IDs, types, defaults, and presets.
7. **Build a settings ledger** — Before writing JSON, record the schema constraints for every setting you plan to emit.
8. **Apply theme design settings** — Map colors, typography, spacing, borders, and behavior to real global/section/block settings declared by the target theme.
9. **Compile JSON** — Build valid `sections`, nested `blocks`, and `order` / `block_order` arrays per Shopify’s template structure.
10. **Validate by strict upload** — Skip Theme Check for JSON template edits; use Shopify's strict upload validator before finishing.

## Workflow

### Step 1 — Understand user requirements

**From text:**

- Layout: grid, flex, slider, inline, etc.
- Column/row counts
- Content: images, buttons, text, product cards, etc.
- Styling: spacing, colors, responsive behavior

**From images:**

- Visual layout, columns and rows, elements and relationships, spacing and alignment
- Visual hierarchy (headline vs body vs CTAs), approximate alignment (left/center/right), and content categories (hero, cards, logos, footer, etc.)
- **What is ambiguous** — Exact breakpoints, font sizes, and pixel spacing are not reliably readable from a static image; call that out instead of guessing.

**Design images and mockups**

When the user provides a screenshot, Figma export, or design mockup:

- **Extract:** structure (sections, columns, stacking order), content types, and qualitative spacing rhythm (tight vs airy), not exact pixel values unless provided separately.
- **Ignore images in source files:** do not copy, crop, recreate, download, duplicate, generate, or invent image assets from screenshots, Figma exports, mockups, or other source files. Use those images only to infer where image/media blocks belong.
- **Use empty image blocks or sections:** when the design calls for hero, product, lifestyle, logo, or decorative imagery, add the valid image/media block or section with its image setting omitted or blank. Assume the user or merchant will provide final images in the theme editor.
- **Only wire explicit assets:** set image/media values only when the user provides existing theme asset filenames, Shopify media IDs/references, already-selected section settings, or files they explicitly ask to use as storefront assets.
- **Do not infer** concrete Shopify `type` strings, setting keys, or enum values from the image alone. **Always** reconcile with **Discover allowed block types** on the target theme.
- **Limits:** JSON templates do not express every visual detail. Arbitrary typography, one-off CSS, new markup, or new behavior may require Liquid/theme-code development outside this skill. Do not implement that code here; say so when the design cannot be matched with existing schema-defined JSON settings only.
- If the design **cannot** be built with the theme’s available blocks and settings, **state the gap** and offer the closest achievable structure.

### Step 2 — Discover allowed block types

Do this **before** naming or emitting any `type` in JSON. This is the operational way to satisfy the validation rule that every `type` exists in the theme.

1. **List** files in `blocks/` — For themes using theme blocks, the basename of `blocks/<name>.liquid` is typically a valid block `type` (confirm in schema).
2. **Read** `{% schema %}` on each **section** you add or edit in the template, and on each **block** file you nest. Collect every block `type` the parent allows (specific types, `@theme`, `@app`, etc., per Shopify rules for that parent).
   - Validate **each immediate parent/child edge**, not just whether a block exists somewhere in the theme. If a layout block only allows specific item-wrapper blocks, content blocks cannot be direct children even when they are valid theme blocks elsewhere. They must be nested under an allowed child if that child permits them.
   - Treat explicit `blocks` arrays as allow-lists. Only `@theme` permits general theme blocks; a specific list permits only those listed types plus any listed `@app`.
   > **`_`-prefix caveat:** Blocks whose filename starts with `_` (e.g. `_stat-bar.liquid`) may **not** be matched by `@theme` in a parent schema, even if they have `presets`. Shopify theme check treats `_`-prefixed blocks as private/static and may require them to be **explicitly listed** in the parent’s `blocks` array. Before nesting a `_`-prefixed block inside a parent that only declares `@theme`, verify by checking existing templates or presets for a precedent. If none exists, do not edit the parent schema; choose another existing allowed block or report that the request needs Liquid/theme-code work outside this JSON-only skill.
3. **Build the allowed set** — Union of: types from `blocks/` that the parent schema permits, types declared in the parent’s `blocks` array, and section `type` values from `sections/<type>.liquid` for template-level `sections`.
4. **Only use types in that set.** If the user asks for a layout that no existing block provides, propose the **closest real types**, copy a working template in the same theme, or note that adding a new block or section is theme development work outside JSON-only edits.

### Step 3 — Check bundled examples (optional)

If `examples/` is present in this skill package, follow **Choosing the best example to reference** in [examples/README.md](examples/README.md):

- Compare the workspace theme to each indexed example (theme name, README, overlap between `blocks/` basenames and the example’s described types).
- Open **at most one** best-matched file for patterns and sample JSON.
- If nothing fits, copy patterns from existing `templates/*.json` in the workspace theme instead.
- **Never** emit `type` strings from a bundled example until they appear in the target theme’s allowed set.

### Step 4 — Map requirements to real blocks

Within the **allowed type set** from Step 2:

- Search the theme’s `blocks/` directory and section schemas for blocks that match the layout and content needs.
- For **template-level** sections, list `sections/` and map JSON `type` to section files (usually basename of `sections/<type>.liquid`).

### Step 5 — Analyze block and section schemas

For each block or section, read `{% schema %}`:

1. **Nested blocks** — `"blocks": [{ "type": "@theme" }]` vs specific types vs `"blocks": []`. Build a small parent-to-child map for every nested level you emit, and check each proposed child against its immediate parent's schema before writing JSON.
2. **Settings** — Required vs optional, defaults, types. Pay attention to:
   - **`range`** — Values must land exactly on `min + (N * step)`, where `N` is a whole number, even when the setting is hidden by `visible_if`. A range with `min: 10, max: 48, step: 2` rejects `13` (use `14`). A range with `min: 100, step: 5` rejects `158` (use `160` or `155`). When copying or inventing settings, calculate the nearest valid step before writing JSON.
   - **`select`** — Values must exactly match one of the `options[].value` strings. Do not invent values even if they look like valid CSS—only the listed options pass validation.
   - **`visible_if`** — Settings gated by `visible_if` may still be validated during upload even when hidden. Prefer omitting hidden custom-only settings entirely unless the controlling setting makes them active. If you keep hidden settings as editor defaults, they must still satisfy the destination schema's range and select constraints.
3. **Presets** — Recommended configurations.

### Build a schema settings ledger before writing JSON

For every section or block type you will emit, collect the schema constraints for every setting you plan to set. Do this before drafting or modifying the JSON so invalid values do not make it into the template.

- **`range`** — Record `min`, `max`, and `step`; normalize every value to `min + (N * step)`.
- **`select`** — Record exact `options[].value` strings and use only those values.
- **Typed settings** — Record expected JSON value types for `checkbox`, `number`, `text`, `richtext`, `image_picker`, `url`, `collection`, `product`, `blog`, and `page`.
- **`visible_if` settings** — Do not skip validation. Shopify may validate hidden settings during upload, so omit hidden custom-only settings or normalize them too.

Common upload-only range traps vary by theme. After selecting a bundled example reference, consult that reference for theme-specific setting families and common validation failures. Regardless of theme, every emitted range value must match the target schema's `min`, `max`, and `step`.

### Step 6 — Apply theme design controls

Prefer schema-defined design settings over arbitrary `custom_css`. Read the relevant schema first and only emit setting keys and values that exist in the target theme. Use `custom_css` only when it is already exposed as a JSON setting in the target schema and the user explicitly asks for a CSS-level tweak; otherwise treat CSS needs as outside the JSON-only scope.

**Global design settings (`config/settings_schema.json` and `config/settings_data.json`):**

- Use global settings for broad storefront-wide changes, such as site-wide colors, typography systems, spacing defaults, radii, and border defaults.
- Use `config/settings_schema.json` for global setting definitions exposed in the theme editor, and `config/settings_data.json` for the active values of those settings.
- Before editing global values, read both files. Preserve the existing `current` object shape and keep setting IDs, value types, defaults, and options aligned between schema and data.
- When adding or changing a global schema setting, follow the surrounding schema group structure and Shopify setting types. Do not use settings schema changes as a back door for custom Liquid, JavaScript, CSS, new sections, or new blocks.
- Do not create arbitrary color scheme IDs, utility class values, or token names. Use existing settings data and schema-listed options.
- Use global changes only when the requested design should affect the whole storefront. For one page, one template, or one section, prefer section/block settings.

**Section settings (`sections/*.liquid`):**

- Use section settings for broad bands of layout and styling: spacing, width, margins, colors, borders, alignment, visibility, behavior, and background media when the section schema supports them.
- Select values must be exact schema options. Do not invent pixel strings, CSS utility strings, animation names, or class names.
- If a section exposes `custom_css` and the user explicitly asks for a CSS-level tweak, entries must be complete CSS rules with selectors, e.g. `.rte { text-transform: uppercase; }`. Do not use standalone declarations or selectorless rules, and do not add or edit stylesheet files.

**Block settings (`blocks/*.liquid`):**

- Use block settings for local visual adjustments inside a section: spacing, colors, borders, shape, typography, layout, visibility, inheritance, and animation when the block schema supports them.
- Respect inheritance settings when a block should use parent section/container styles. Only override local colors, typography, or borders when the destination block schema declares those settings and the design requires it.
- Typography, layout, and animation settings vary by block. Validate every setting ID, range step, select option, and enum-like string against the destination block schema.
- Layout container blocks often have restricted child rules. Follow each parent block's schema for allowed children, and copy nesting patterns only from the same target theme or a verified matching bundled example.
- Do not copy a setting from one block type into another unless that destination block schema declares the same setting ID and accepts the same value.

### Step 7 — Compile JSON structure

Follow Shopify’s JSON template shape. If the theme ships **Cursor rules** (e.g. `.cursor/rules/templates.mdc`, `blocks.mdc`, `schemas.mdc`), follow those for the project you are editing.

**Template structure:**

```json
{
  "sections": {
    "<sectionId>": {
      "type": "<sectionType>",
      "settings": {},
      "blocks": {
        "<blockId>": {
          "type": "<blockType>",
          "name": "t:blocks.block_name",
          "settings": {},
          "blocks": {},
          "block_order": []
        }
      },
      "block_order": ["<blockId>"]
    }
  },
  "order": ["<sectionId>"]
}
```

**Block instance IDs:**

- Use descriptive prefixes that reflect role (e.g. layout, container, item, image, text)—match conventions you see in the theme’s existing JSON.
- Do **not** start block instance IDs with `_`. Private block **types** may start with `_` (for example, `"type": "_grid-products"`), but the JSON object key for that block must be a valid instance ID such as `"grid_products_best"`.
- Append a short unique suffix if needed.
- Keep IDs unique within the template (and within each nested `blocks` scope per schema rules).

**Static blocks and `block_order`:**

- Static blocks (`"static": true`) are declared in the parent `blocks` object but must **not** be listed in that parent's `block_order`.
- `block_order` is only for dynamic/reorderable sibling blocks. If a parent contains only static child blocks, use `"block_order": []` or omit it if the existing template pattern allows omission.
- Before finishing, compare every sibling `block_order` against sibling `blocks`: each listed ID must exist and must not point to a block object with `"static": true`.

**Settings (block-based themes):**

- Layout blocks often expose settings for column counts, rows, gaps, padding, and responsive behavior. Use only the setting IDs and values declared by the target schema.
- Prefer translation keys for block `name` when the theme does: e.g. `"t:blocks.image"`.
- Align color and spacing settings with the theme’s design system.

**Whole-template replacement guard:**

After replacing an entire JSON template, parse the file with Shopify's generated-file comment header stripped and verify there is exactly one top-level JSON object. Do not leave an appended copy of the previous template after the new closing brace.

```sh
node -e "const fs=require('fs'); const s=fs.readFileSync('templates/index.json','utf8').replace(/^\/\*[\s\S]*?\*\/\s*/, ''); JSON.parse(s);"
```

**Richtext content rules:**

Shopify sanitizes HTML in `richtext` and `text` (rich) settings. Only a limited set of tags and attributes are allowed:

- **Allowed tags:** `p`, `h1`–`h6`, `ul`, `ol`, `li`, `a`, `br`, `strong`, `em`, `span`
- **No `style` attributes** — `<span style="color: red">` will be stripped. Use schema-defined rich text or typography settings where available; otherwise note the gap instead of adding inline styles, stylesheet rules, custom Liquid, or new assets.
- **No `class` attributes** on inline elements in richtext.
- If the design requires colored text within a single text block and no existing JSON setting supports it, this is a **theme-code customization gap**. Note it instead of adding inline styles, stylesheet rules, custom Liquid, or new assets.

### Step 8 — Validate

Run the **Validation checklist** below before finishing.

For JSON template edits, skip `shopify theme check` and validate with Shopify's upload validator instead:

```sh
shopify theme push --strict --only templates/<name>.json
```

For the homepage, use:

```sh
shopify theme push --strict --only templates/index.json
```

This is not a dry run; it uploads the specified file if validation passes. Use it only when uploading that template is acceptable.

For non-interactive agents, avoid the theme-selection prompt. If `npm run dev` or `shopify theme dev` is already running, inspect its output for `preview_theme_id=<theme_id>` and validate against that same development theme:

```sh
shopify theme push --strict --only templates/<name>.json --theme <theme_id>
```

If no theme ID is available and the command would prompt for a theme, ask the user which theme to target before pushing.

Treat any `templates/<name>.json` error printed after `Theme upload complete` as authoritative, even when the command exits `0`. If it fails, copy the exact console error into the validation checklist and add a local guard for that class of error before retrying.

When `npm run dev` or `shopify theme dev` is already running, use its upload log as the practical validation loop for JSON template edits:

1. Save or touch the edited template so the dev server attempts to sync it.
2. Inspect the dev terminal for `Failed to upload file "templates/<name>.json" to remote theme`.
3. Treat the message below that line as authoritative Shopify validation.
4. Fix the specific class of error and add it to this skill/checklist if it is not already covered.
5. Repeat until the dev terminal reports a successful sync or no new upload error for the template.

Do not start a duplicate dev server if one is already running. Prefer the existing terminal output. Known upload-only JSON template errors to guard locally include:

- `Invalid CSS value`: custom CSS array entries must be complete CSS rules with selectors, not standalone declarations.
- `Setting '<id>' must be a step in the range`: range settings must use exact `min + (N * step)` values, including settings hidden by `visible_if`.
- `Setting '<id>' can't be less than <min>` or `can't be greater than <max>`: range settings must stay inside schema bounds, including hidden settings.
- `'<id>' is not a valid block id`: block instance IDs must not use invalid characters or leading `_`.
- `static block with id '<id>' must not be present in 'block_order'`: remove static block IDs from sibling `block_order`.

- JSON is valid
- Whole-template replacements contain exactly one top-level JSON object after stripping Shopify's generated-file comment header; do not leave appended duplicate template content after the closing brace.
- Section and block instance IDs are unique in scope
- Block instance IDs do not start with `_`; only block `type` strings may use private `_` prefixes
- Every `block_order` matches the keys in its sibling `blocks` object
- No block with `"static": true` appears in its sibling `block_order`
- Top-level `order` lists every section key to render
- Each `type` exists in `sections/` or `blocks/` and is allowed by the parent schema
- Nested block types are valid for the parent
- Setting keys and value types match `{% schema %}`
- Range values land on valid steps (`min + N*step`) for every range setting present in JSON, including values hidden by `visible_if`
- Select values match an `options[].value` exactly
- Images from source files are ignored; use empty image/media blocks or sections unless explicit storefront asset values were provided
- No `style` or `class` attributes in richtext content strings
- `_`-prefixed blocks are explicitly listed (not just `@theme`) in every parent they nest inside

## Error handling


| Problem                 | What to do                                                       |
| ----------------------- | ---------------------------------------------------------------- |
| Block/section not found | Search `blocks/` and `sections/`; suggest close matches          |
| Invalid nesting         | Re-read parent `{% schema %}` `blocks` array                     |
| `_` block not allowed   | Block has `_` prefix; choose an existing allowed block or report that schema/Liquid work is outside this JSON-only skill |
| `style` attr stripped   | Shopify sanitizes richtext; use schema-defined text settings where available; otherwise note the theme-code customization gap |
| Range step violation    | Read the schema `step` value; round your value to the nearest valid step       |
| Hidden range violation  | Omit hidden custom-only settings or validate them anyway; hidden settings can still fail upload validation |
| Range min/max violation | Keep values within the target schema bounds, even when copying settings from another template or example |
| Invalid select value    | Only use values from the schema `options` array; don't invent CSS expressions  |
| Mockup image copied     | Remove screenshot-derived image values; preserve the layout with valid empty image/media blocks |
| Schema too large        | Copy patterns from a working template in the same theme          |
| Ambiguous request       | Ask which template file and which section/block instances change |
| Generated theme output  | Edit the source only when the source-of-truth path is JSON/settings data; otherwise report that theme-code work is outside this skill |


## Further reading


| File                                                   | Purpose                                             |
| ------------------------------------------------------ | --------------------------------------------------- |
| [reference/shopify.md](reference/shopify.md) | Official docs links, filenames, platform limits     |
| [examples/README.md](examples/README.md)               | How to pick a bundled theme example; index of files |


In a **theme repository**, also use project rules such as `templates.mdc`, `blocks.mdc`, and `schemas.mdc` when present under `.agent/rules/`.

## Sources

These are the Shopify documentation pages this skill's content is derived from. When Shopify updates these pages, review the corresponding sections above (indicated by `<!-- source: ... -->` comments).


| Shopify doc                                                                                         | Sections in this file                                  |
| --------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| [Theme architecture](https://shopify.dev/docs/storefronts/themes/architecture)                      | When to use this skill                                 |
| [JSON templates](https://shopify.dev/docs/storefronts/themes/architecture/templates/json-templates) | Step 4 — Compile JSON structure, Validation checklist  |
| [Sections](https://shopify.dev/docs/storefronts/themes/architecture/sections)                       | Discover allowed block types, Step 3 — Analyze schemas |
| [Section schema](https://shopify.dev/docs/storefronts/themes/architecture/sections/section-schema)  | Discover allowed block types, Step 3 — Analyze schemas |
| [Blocks](https://shopify.dev/docs/storefronts/themes/architecture/blocks)                           | Discover allowed block types                           |
| [Theme blocks](https://shopify.dev/docs/storefronts/themes/architecture/blocks/theme-blocks)        | Discover allowed block types, Step 3 — Analyze schemas |
