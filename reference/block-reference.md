# Block reference guide

Quick reference for common blocks under a theme’s `blocks/` directory and typical use cases. **Always** confirm `type` strings and settings against the **target theme**’s Liquid files and `{% schema %}`.

## Layout blocks

### layout__grid

**Purpose:** Grid container for arranging blocks in rows and columns.

**Children:** `_g__grid-item` only.

**Key settings:**

- `row_desktop` (1–12) — Number of columns on desktop
- `row_mobile` (1–4) — Number of columns on mobile
- `gap_size` — Spacing between items
- `enable_x_padding` — Horizontal padding

**Use when:** Equal-width columns, product grids, image galleries.

**Example:**

```json
{
  "type": "layout__grid",
  "settings": {
    "row_desktop": 3,
    "row_mobile": 1,
    "gap_size": "default"
  }
}
```

### layout__flex

**Purpose:** Flexbox container for flexible layouts.

**Children:** `_g__flex-item` only.

**Key settings:**

- `direction` — `"flex-row"` or `"flex-col"`
- `row_x_alignment` / `row_y_alignment` — Alignment for row direction
- `col_x_alignment` / `col_y_alignment` — Alignment for column direction
- `enable_block_wrap` — Allow items to wrap
- `gap_size` — Spacing between items

**Use when:** Flexible widths, variable column sizes, complex alignments.

**Example:**

```json
{
  "type": "layout__flex",
  "settings": {
    "direction": "flex-row",
    "gap_size": "default"
  }
}
```

### layout__slider

**Purpose:** Carousel/slider for horizontal scrolling content.

**Children:** `_g__slider-item` only.

**Key settings:**

- `show_arrows` — Navigation arrows
- `show_indicators` — Dot indicators
- `enable_auto_scroll` — Auto-advance slides
- `auto_scroll_delay` — Delay between slides (ms)

**Use when:** Image carousels, product sliders, testimonial carousels.

### layout__inline

**Purpose:** Inline horizontal layout.

**Children:** Any theme blocks (per schema).

**Use when:** Simple horizontal arrangement without flex complexity.

### g__container

**Purpose:** Generic container for grouping and wrapping blocks.

**Children:** Any theme blocks (`@theme`, `@app`) when the schema allows.

**Key settings:**

- `enable_background_image_or_video` — Background media
- `enable_x_padding` / `enable_y_padding` — Padding controls
- `color_scheme` — Background/text colors
- `minimum_height` — Minimum container height
- `y_alignment` — Vertical alignment (top, middle, bottom)

**Use when:** Grouping multiple blocks, backgrounds, section-like regions.

## Content blocks

### image

**Purpose:** Display a single image with styling options.

**Children:** None (unless schema says otherwise).

**Key settings:**

- `image` — Image picker
- `enable_aspect_ratio` — Force aspect ratio
- `aspect_ratio` — e.g. `"aspect-1/1"`, `"aspect-4/3"`, `"aspect-16/9"`
- `width` — Image width percentage
- `color_scheme` — Background for transparent images
- `radius` — Border radius

**Use when:** Product images, hero images, gallery images.

### g__button

**Purpose:** Button/link with customizable styling and actions.

**Children:** Per schema: e.g. `richtext`, `icon`, `image`, `localization-label`, `cart-count`, `cart-price`.

**Key settings:**

- `url` — Link destination
- `enable_custom_action` — JavaScript action instead of URL
- `overlay_preset` — Predefined overlay actions
- `color_button` — Button style class
- `button_size` — Size class
- `enable_full_width` — Full-width button
- `x_alignment` — Horizontal alignment

**Use when:** CTAs, navigation buttons, action buttons.

### richtext

**Purpose:** Rich text content with formatting.

**Children:** None (unless schema says otherwise).

**Key settings:**

- `text` — Rich text content
- `font_size` — Text size class
- `color_text` — Text color class
- `x_alignment` — Text alignment
- `enable_max_width` — Limit text width

**Use when:** Headings, paragraphs, descriptions.

### g__product-card

**Purpose:** Product card with image, title, price.

**Children:** None (unless schema says otherwise).

**Key settings:**

- `product` — Product picker or `{{ product }}`
- `show_vendor` — Vendor name
- `show_price` — Price
- `color_scheme` — Card styling

**Use when:** Product listings, featured products, recommendations.

### g__article-card

**Purpose:** Blog article card.

**Children:** None (unless schema says otherwise).

**Key settings:**

- `article` — Article picker
- `show_date` — Publish date
- `show_author` — Author
- `show_excerpt` — Excerpt

**Use when:** Blog listings, recent articles, featured posts.

### g__collection-card

**Purpose:** Collection card.

**Children:** None (unless schema says otherwise).

**Key settings:**

- `collection` — Collection picker
- `show_description` — Collection description

**Use when:** Collection listings, featured collections.

## Item wrapper blocks

### _g__grid-item

**Purpose:** Wrapper for content inside a grid layout.

**Children:** Any theme blocks (`@theme`, `@app`) when allowed by schema.

**Key settings:**

- `enable_padding` — Internal padding
- `gap_size` — Spacing between child blocks
- `col_span` / `row_span` — If supported by schema
- `y_alignment` — Vertical alignment

**Use when:** Inside `layout__grid`.

### _g__flex-item

**Purpose:** Wrapper for content inside a flex layout.

**Children:** Any theme blocks (`@theme`, `@app`) when allowed by schema.

**Key settings:**

- `width_mobile` — Width % on mobile (0–100)
- `width_desktop` — Width % on desktop (0–100)
- `enable_width_fill` — Fill available space
- `y_alignment` — Vertical alignment
- `enable_sticky_layout` — Sticky positioning
- `sticky_position` — `"top"` or `"bottom"`

**Use when:** Inside `layout__flex`.

### _g__slider-item

**Purpose:** Wrapper for content inside a slider.

**Children:** Any theme blocks (`@theme`, `@app`) when allowed by schema.

**Use when:** Inside `layout__slider`.

## Specialized blocks

### g__accordion

**Purpose:** Collapsible accordion.

**Children:** Often `g__container` pairs for label/content; follow schema.

**Key settings:**

- `enable_pre_opened` — Start open
- `enable_single_open` — One item open at a time

**Use when:** FAQs, collapsible content.

### g__video

**Purpose:** Video player with controls.

**Key settings:**

- `video` — Video picker
- `autoplay`, `loop`, `muted`

**Use when:** Hero videos, background videos, product videos.

### divider

**Purpose:** Horizontal divider/separator.

**Key settings:**

- `spacing_top` / `spacing_bottom`
- `color_border`

**Use when:** Section separators, content breaks.

## Block naming patterns

Themes often use conventions such as:

- `layout__*` — Layout containers
- `g__*` — Grouped blocks (may nest)
- `_g__*` — Private grouped blocks (tied to a parent)
- `_*` — Private blocks valid only in a specific parent
- No prefix — Standard content blocks

Exact rules depend on the theme; see its docs or `file-naming` rules if present.

## Block compatibility (typical)

**Often accept any theme block (when schema allows):**

- `g__container` — `@theme`, `@app`
- `_g__grid-item`, `_g__flex-item`, `_g__slider-item` — `@theme`, `@app`

**Specific children only:**

- `layout__grid` — `_g__grid-item`
- `layout__flex` — `_g__flex-item`
- `layout__slider` — `_g__slider-item`
- `g__button` — `richtext`, `icon`, `image`, etc. per schema

**Often no children:**

- `image`, `richtext`, `g__product-card`, `g__article-card`, and many content blocks

Verify every parent/child pair in the live `{% schema %}` for the theme you are editing.
