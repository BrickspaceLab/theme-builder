# Slab

This file holds bundled knowledge for the Slab Shopify theme.

Use [checklist.md](checklist.md) when building, reviewing, or testing Slab theme pages.

## Block reference

Curated Slab blocks for typical JSON templates: **layout** (full set), **content**, and **cards**. The JSON `type` is the basename of `blocks/<type>.liquid`. Always confirm against the **workspace** theme’s `{% schema %}`.

When the theme repo is open, listing `blocks/` is the source of truth for every block type; this README lists only common building blocks.

### Layout


| Role                  | JSON `type`       | Theme file                      |
| --------------------- | ----------------- | ------------------------------- |
| Grid                  | `layout__grid`    | `blocks/layout__grid.liquid`    |
| Grid cell             | `_g__grid-item`   | `blocks/_g__grid-item.liquid`   |
| Flex row/column       | `layout__flex`    | `blocks/layout__flex.liquid`    |
| Flex cell             | `_g__flex-item`   | `blocks/_g__flex-item.liquid`   |
| Slider                | `layout__slider`  | `blocks/layout__slider.liquid`  |
| Slide                 | `_g__slider-item` | `blocks/_g__slider-item.liquid` |
| Inline cluster        | `layout__inline`  | `blocks/layout__inline.liquid`  |
| Float                 | `layout__float`   | `blocks/layout__float.liquid`   |
| Marquee               | `layout__marquee` | `blocks/layout__marquee.liquid` |
| Tabs                  | `layout__tab`     | `blocks/layout__tab.liquid`     |
| Tab panel             | `_g__tab-item`    | `blocks/_g__tab-item.liquid`    |
| Table (layout)        | `layout__table`   | `blocks/layout__table.liquid`   |
| Table row             | `_g__table-row`   | `blocks/_g__table-row.liquid`   |
| Table cell            | `_g__table-cell`  | `blocks/_g__table-cell.liquid`  |
| Overlay / fixed layer | `layout__overlay` | `blocks/layout__overlay.liquid` |
| Container / frame     | `g__container`    | `blocks/g__container.liquid`    |
| Divider               | `divider`         | `blocks/divider.liquid`         |


### Content


| Role             | JSON `type`        | Theme file                       |
| ---------------- | ------------------ | -------------------------------- |
| Rich text        | `richtext`         | `blocks/richtext.liquid`         |
| Image            | `image`            | `blocks/image.liquid`            |
| Image comparison | `image-comparison` | `blocks/image-comparison.liquid` |
| Icon             | `icon`             | `blocks/icon.liquid`             |
| Button           | `g__button`        | `blocks/g__button.liquid`        |
| Video            | `g__video`         | `blocks/g__video.liquid`         |


### Cards


| Role            | JSON `type`          | Theme file                         |
| --------------- | -------------------- | ---------------------------------- |
| Product card    | `g__product-card`    | `blocks/g__product-card.liquid`    |
| Article card    | `g__article-card`    | `blocks/g__article-card.liquid`    |
| Collection card | `g__collection-card` | `blocks/g__collection-card.liquid` |




## Design controls

Slab exposes most visual changes through schema settings. Prefer these existing settings over arbitrary `custom_css`. Read the relevant schema first and only emit setting keys and values that exist there. Do not create or edit Liquid, blocks, sections, snippets, stylesheets, or other theme code when using this skill.

### Global design settings (`config/settings_data.json`)

- Use global settings for broad storefront-wide changes: body foreground/background, color schemes, theme fonts, heading/body/button typography systems, default margins/gaps, button/input/element radii, and global border widths.
- Before editing global values, read `config/settings_schema.json` and `config/settings_data.json`. Preserve the existing `current` object shape and only update keys that exist in the schema.
- Key global families in Slab include:
  - Colors: `color_schemes`, `color_body_background`, `color_body_foreground`, `color_error`, `color_success`, `color_mobile_bar`, `color_overlay_background`, `color_links_type`, `color_links_custom`.
  - Typography: `type_font_body`, `type_font_heading`, `type_font_subheading`, `type_font_accent`, plus heading/button type controls such as `type_button_font`, `type_button_line_height`, `type_button_letter_spacing`, `type_button_case`, and `type_heading_*`.
  - Shape and borders: `border_button_radius`, `border_input_radius`, `border_element_radius`, `border_button_width`.
- Do not create arbitrary color scheme IDs. If `color_type: "custom"` uses `color_scheme_custom`, pick an existing scheme ID from `settings_data.json` / the theme editor data. Otherwise use `color_type: "base"` with schema-listed utility class values.
- Use global changes only when the requested design should affect the whole storefront. For one homepage or one section, prefer section/block settings.

### Section settings (`sections/section.liquid`)

- Use section settings for broad bands of layout and styling. Common real settings are:
  - Spacing: `spacing_top`, `spacing_bottom`, `minimum_height`, `gap_size`.
  - Width/margins: `margin`, `mobile_margin`, `enable_mobile_margin`.
  - Colors/borders: `color_type`, `color_scheme_custom`, `color_scheme`, `color_border`, `border_position`.
  - Layout/behavior: `y_alignment`, `enable_header_overlap`, `enable_hide_on_scroll`, `show_above`, `scroll_snap_align`, `visibility`.
  - Background media: `enable_background_image_or_video`, `image_background_desktop`, `image_background_mobile`, `video_background`, `show_video_on_mobile`, `show_entire_image`.
- Section `margin` controls the outer content width (`default`, `narrow`, `standard`, `wide`, `full`, `none`). Use `margin: "none"` only for full-bleed bands; use `default` for normal content.
- `gap_size` options are schema values (`none`, `default`, `xs`, `sm`, `md`, `lg`, `xl`). Do not invent pixel strings.
- `color_scheme` values must be exact schema utility strings such as `color-bg__body-bg color-tx__text-default`, `color-bg__overlay-1 color-tx__text-default`, `color-bg__shade-1 color-tx__text-default`, or `bg-transparent`.
- `color_border` and `border_position` must use exact schema class strings. Use `border-0!` for no border when that option exists; otherwise use a listed divider/border class.
- Use `custom_css` only when it is already exposed as a JSON setting and the user explicitly asks for a CSS-level tweak. Entries must be complete CSS rules with selectors, e.g. `.rte { text-transform: uppercase; }`. Do not use standalone declarations like `--color__scheme-bg: transparent;` or selectorless rules like `{ background: ... }`.

### Block settings (`blocks/*.liquid`)

- Use block settings for local visual adjustments inside a section. Common setting families across Slab blocks are:
  - Spacing: `enable_x_padding`, `enable_y_padding`, `enable_t_padding`, `enable_b_padding`, `spacing_top`, `spacing_bottom`, `gap_size`.
  - Colors: `color_type`, `color_scheme_custom`, `color_scheme`, `color_background`, `color_text`, `color_button`, `color_button_custom`, `enable_inheritance`.
  - Borders/shape: `color_border`, `border_position`, `radius`, `enable_shadow`, `enable_block_elevation`.
  - Typography: `font_family`, `font_size`, `font_size_custom`, `line_height_custom`, `letter_spacing_custom`.
  - Layout: `x_alignment`, `y_alignment`, `width`, `width_mobile`, `width_desktop`, `minimum_height`, `enable_height_fill`, `sticky_position`, `visibility`.
  - Animation: `load_animation`, `scroll_animation`.
- Respect inheritance. If a block should use its parent section/container colors, set `enable_inheritance: true` and avoid extra color settings unless the schema requires defaults. If it should override, set `enable_inheritance: false` and provide valid `color_type`/scheme/text/border values from that block schema.
- Typography settings vary by block. For `richtext`, schema values include font families like `""`, `*:type__heading`, `*:type__subheading`, `*:type__accent`; font sizes like `*:type--`, `*:type--small`, or heading presets. Custom ranges must obey steps (`font_size_custom` min `4`, step `2`; `line_height_custom` min `100`, step `5`; `letter_spacing_custom` min `-3`, step `0.1`).
- Layout container blocks have restricted child rules and layout settings:
  - `layout__flex` directly accepts only `_g__flex-item`; put content blocks inside `_g__flex-item`.
  - `layout__grid` accepts grid/feed blocks and theme blocks; use `row_desktop`, `row_mobile`, and `gap_size` from schema.
  - Slider/list/grid private blocks often render repeated resource cards; follow existing presets for static `product-card` / card child blocks.
- Animation values must be exact schema options. Common load values include `""`, `load-fadein`, and `load-fadein-offset`; common scroll values include `""`, `scroll-fullblurinout-10-90`, `scroll-slidedownup-10-90`, `scroll-slideupdown-10-90`, `scroll-slideleftright-10-90`, `scroll-sliderightleft-10-90`, and `scroll-zoominout-10-90` when listed by that block.
- Do not copy a setting from one block type into another unless that destination block schema declares the same setting ID and accepts the same value.

## Examples

### 3-column grid with image and button per column

Use this when you want equal-width columns driven by `layout__grid` with `row_desktop: 3`, repeating the same block pattern in each `_g__grid-item`.

**Structure:**

```
layout__grid (row_desktop: 3)
  └── _g__grid-item (×3)
      └── image
      └── g__button
```

**Illustrative nested block JSON** — [three-column-grid-image-button.json](three-column-grid-image-button.json) (verify types/settings in the target theme).

### Flex row with multiple items

Use this for row-based layouts where each column’s width is controlled on `_g__flex-item` (for example fractional or custom widths) instead of an implicit grid.

**Structure:**

```
layout__flex (direction: flex-row)
  └── _g__flex-item (×3)
      └── image
      └── richtext
```

**Illustrative nested block JSON** — [flex-row-multiple-items.json](flex-row-multiple-items.json) (verify types/settings in the target theme).

### Nested containers

Use this when a single grid or flex cell should wrap a shared inner frame (`g__container`) so spacing and grouping apply to several blocks together.

**Structure:**

```
_g__grid-item
  └── g__container (optional)
      └── image
      └── richtext
      └── g__button
```

**Illustrative nested block JSON** — [nested-containers.json](nested-containers.json) (verify types/settings in the target theme).

### 3-column flex, nested image grid, footer row

![Theme editor preview: three-column flex layout, nested two-cell image grid, and split footer row](three-column-flex-nested-image-grid-footer-row.png)


Two stacked flex layouts under one `section`—a main row with custom column widths (navigation column, bio column, wide column with stacked richtext and a nested two-cell image grid) and a footer row with a top border and split left/right richtext.

**Structure:**

```
section (type: section)
  ├── layout__flex (layout_flex_main_3col)
  │   ├── _g__flex-item (flex_item_nav_left)
  │   │   └── richtext
  │   ├── _g__flex-item (flex_item_bio_center)
  │   │   └── richtext
  │   └── _g__flex-item (flex_item_right_content)
  │       ├── richtext
  │       └── layout__grid (layout_grid_images)
  │           ├── _g__grid-item (grid_item_img_left) → image
  │           └── _g__grid-item (grid_item_img_right) → image
  └── layout__flex (layout_flex_footer_row)
      ├── _g__flex-item (flex_item_footer_left) → richtext
      └── _g__flex-item (flex_item_footer_right) → richtext
```

**Full section JSON** — [three-column-flex-nested-image-grid-footer-row.json](three-column-flex-nested-image-grid-footer-row.json) (verify types/settings in the target theme)
