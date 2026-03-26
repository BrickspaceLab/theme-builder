---
name: theme-agent
description: >-
  Helps create and edit Shopify Online Store 2.0 JSON templates (templates/*.json) and
  theme JSON that uses blocks from /blocks: understand requirements, find blocks, analyze
  {% schema %}, and compile valid sections/order/block_order structures. Use when working
  with Shopify templates, JSON templates, the theme editor, or when adding layouts, grids,
  sections, or blocks to a Shopify theme.
---

# Theme Agent — Shopify JSON templates (Online Store 2.0)

## When to use this skill

Use when the task involves **JSON templates** under `templates/` (e.g. `product.json`, `index.json`, alternates like `page.contact.json`, or customer JSON under `templates/customers/` if present), or **section JSON** that references **theme blocks** from `blocks/` in block-based themes.

**This skill alone does not fully cover:**

- `config/settings_schema.json` or `config/settings_data.json` (global theme settings)
- Checkout branding or Checkout UI extensions
- Theme app extension code under `extensions/`
- Replacing **Liquid** `.liquid` templates where the theme has not adopted JSON for that route

If the theme uses a **build step** that generates `sections/` or `blocks/`, read that theme’s README and follow its source-of-truth paths before editing compiled output.

## Quick start

1. **Understand requirements** — Parse prompts or images for layout (grid, flex, slider, inline), columns/rows, content types, and styling.
2. **Find blocks** — Search the theme’s `blocks/` directory for appropriate block types; resolve section `type` from `sections/*.liquid` when editing template-level JSON.
3. **Analyze schemas** — Read each block or section `{% schema %}` for allowed children, setting IDs, types, defaults, and presets.
4. **Compile JSON** — Build valid `sections`, nested `blocks`, and `order` / `block_order` arrays per Shopify’s template structure.
5. **Validate** — Run the checklist below before finishing.

## Workflow

### Step 1 — Understand user requirements

**From text:**

- Layout: grid, flex, slider, inline, etc.
- Column/row counts
- Content: images, buttons, text, product cards, etc.
- Styling: spacing, colors, responsive behavior

**From images:**

- Visual layout, columns and rows, elements and relationships, spacing and alignment

### Step 2 — Find appropriate blocks

Search the theme’s **`blocks/`** directory. Common patterns (names vary by theme):

| Role | Typical block types |
|------|---------------------|
| **Layout** | `layout__grid` (children: `_g__grid-item`), `layout__flex` (`_g__flex-item`), `layout__slider` (`_g__slider-item`), `layout__inline`, `g__container` |
| **Content** | `image`, `g__button`, `richtext`, `g__product-card`, `g__article-card`, `g__collection-card` |
| **Item wrappers** | `_g__grid-item`, `_g__flex-item`, `_g__slider-item` |

For **template-level** sections, list `sections/` and map JSON `type` to section files (usually basename of `sections/<type>.liquid`).

### Step 3 — Analyze block and section schemas

For each block or section, read `{% schema %}`:

1. **Nested blocks** — `"blocks": [{ "type": "@theme" }]` vs specific types vs `"blocks": []`.
2. **Settings** — Required vs optional, defaults, types (range, select, checkbox, etc.).
3. **Presets** — Recommended configurations.

### Step 4 — Compile JSON structure

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

- Use descriptive prefixes: `layout_grid_`, `g_container_`, `image_`, `g_button_`.
- Append a short unique suffix if needed: e.g. `layout_grid_abc123`.
- Keep IDs unique within the template (and within each nested `blocks` scope per schema rules).

**Settings (themes with layout blocks):**

- Grids: often `row_desktop`, `row_mobile`, `gap_size`, `enable_x_padding`.
- Prefer translation keys for block `name` when the theme does: e.g. `"t:blocks.image"`.
- Align `color_scheme` and spacing with the theme’s design system.

## Common patterns

### 3-column grid with image and button per column

**Structure:**

```
layout__grid (row_desktop: 3)
  └── _g__grid-item (×3)
      └── image
      └── g__button
```

**Illustrative nested block JSON** (verify types/settings in the target theme):

```json
{
  "type": "layout__grid",
  "settings": {
    "row_desktop": 3,
    "row_mobile": 1,
    "gap_size": "default",
    "enable_x_padding": false
  },
  "blocks": {
    "grid_item_1": {
      "type": "_g__grid-item",
      "blocks": {
        "image_1": {
          "type": "image",
          "settings": {
            "enable_x_padding": false,
            "enable_t_padding": false,
            "enable_b_padding": false
          }
        },
        "button_1": {
          "type": "g__button",
          "settings": {
            "url": "/collections/all",
            "enable_x_padding": false
          }
        }
      },
      "block_order": ["image_1", "button_1"]
    }
  },
  "block_order": ["grid_item_1", "grid_item_2", "grid_item_3"]
}
```

### Flex row with multiple items

`layout__flex` with `direction: flex-row` and several `_g__flex-item` blocks; set `width_desktop` / `width_mobile` per schema.

### Nested containers

```
_g__grid-item
  └── g__container (optional)
      └── image
      └── richtext
      └── g__button
```

## Settings best practices (block-based themes)

**Layout:**

- `row_desktop` / `row_mobile` — Grid columns (often 1–12 / 1–4).
- `gap_size` — e.g. `"none"`, `"xs"`, `"sm"`, `"default"`, `"md"`, `"lg"`, `"xl"`.
- `enable_x_padding` — Often `false` for full-bleed rows.
- `spacing_top` / `spacing_bottom` — Vertical spacing if present in schema.
- `visibility` — Responsive visibility classes if the theme defines them.

**Content:**

- `enable_x_padding`, `enable_t_padding`, `enable_b_padding` — Per-block padding.
- `color_scheme` — Theme schemes.
- `aspect_ratio` — For images.
- `x_alignment` — e.g. `text-left`, `text-center`, `text-right`.

## Validation checklist

- [ ] JSON is valid
- [ ] Section and block **instance ids** are unique in scope
- [ ] Every `block_order` matches the keys in the sibling `blocks` object at that level
- [ ] Top-level `order` lists every section key to render
- [ ] Each `type` exists in the theme’s `sections/` or `blocks/` (and is allowed by the parent schema)
- [ ] Nested block types are valid for the parent
- [ ] Setting keys and value types match `{% schema %}`

## Error handling

| Problem | What to do |
|--------|------------|
| Block/section not found | Search `blocks/` and `sections/`; suggest close matches |
| Invalid nesting | Re-read parent `{% schema %}` `blocks` array |
| Schema too large | Copy patterns from a working template in the same theme |
| Ambiguous request | Ask which template file and which section/block instances change |
| Generated theme output | Edit source per theme docs, then build |

## Further reading in this package

- [reference/block-reference.md](reference/block-reference.md) — Layout and content blocks, wrappers, compatibility
- [reference/shopify-json.md](reference/shopify-json.md) — Official docs links, filenames, platform limits
- [examples/minimal-templates.md](examples/minimal-templates.md) — Generic section-level examples (verify types against the target theme)

In a **theme repository**, also use project rules such as `templates.mdc`, `blocks.mdc`, and `schemas.mdc` when present under `.cursor/rules/`.
