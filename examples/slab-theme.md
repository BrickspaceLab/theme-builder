# Slab / Brickspace-style blocks (example)

This file holds **Slab-related** bundled knowledge for this skill. Other themes are documented in separate files under `examples/` as they are added.

These patterns match themes that use **Slab-style** block filenames and settings (e.g. `layout__grid`, `_g__grid-item`). **Do not** use these `type` values in another theme without checking its `blocks/` and `{% schema %}`.

## Block type reference (illustrative)

| Role | Typical block types |
|------|---------------------|
| **Layout** | `layout__grid` (children: `_g__grid-item`), `layout__flex` (`_g__flex-item`), `layout__slider` (`_g__slider-item`), `layout__inline`, `g__container` |
| **Content** | `image`, `g__button`, `richtext`, `g__product-card`, `g__article-card`, `g__collection-card` |
| **Item wrappers** | `_g__grid-item`, `_g__flex-item`, `_g__slider-item` |

## 3-column grid with image and button per column

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

## Flex row with multiple items

`layout__flex` with `direction: flex-row` and several `_g__flex-item` blocks; set `width_desktop` / `width_mobile` per schema.

## Nested containers

```
_g__grid-item
  └── g__container (optional)
      └── image
      └── richtext
      └── g__button
```

## Settings vocabulary (typical for this theme family)

Slab-style schemas often use keys like the following; **always** confirm names and enums in the target `{% schema %}`.

**Layout**

- `row_desktop` / `row_mobile` — Grid columns (often 1–12 / 1–4).
- `gap_size` — e.g. `"none"`, `"xs"`, `"sm"`, `"default"`, `"md"`, `"lg"`, `"xl"`.
- `enable_x_padding` — Often `false` for full-bleed rows.
- `spacing_top` / `spacing_bottom` — Vertical spacing if present.
- `visibility` — Responsive visibility if the theme defines it.

**Content**

- `enable_x_padding`, `enable_t_padding`, `enable_b_padding` — Per-block padding.
- `color_scheme` — Theme schemes.
- `aspect_ratio` — For images.
- `x_alignment` — e.g. `text-left`, `text-center`, `text-right`.
