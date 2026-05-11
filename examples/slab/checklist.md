# Slab Page-Building Checklist

Use this checklist when building, reviewing, or testing Slab theme pages. Not every page needs every feature, but skipped items should be intentional.

## Header

- [ ] Header works on mobile; use visibility settings on duplicate header blocks such as `blocks/logo.liquid`, `blocks/menu.liquid`, `blocks/g__menu-dropdown.liquid`, `blocks/cart-count.liquid`, or `blocks/g__account.liquid` when a custom mobile layout is needed.
- [ ] Logo renders correctly with `blocks/logo.liquid`.
- [ ] Dropdown navigation works with `blocks/g__menu-dropdown.liquid`.
- [ ] Primary links render through `blocks/menu.liquid` or `blocks/_g__menu-item.liquid`.
- [ ] Cart button and cart count work with `blocks/cart-count.liquid`, using `blocks/g__button.liquid` for custom cart action buttons where needed.
- [ ] Account entry works with `blocks/g__account.liquid`, using `blocks/overlay__drawer.liquid`, `blocks/overlay__sidebar.liquid`, or `blocks/overlay__popup.liquid` where an account sheet or overlay is appropriate.

## Menu

- [ ] Menu works on mobile; use visibility settings on duplicate menu blocks such as `blocks/menu.liquid`, `blocks/_g__menu-item.liquid`, `blocks/g__menu-dropdown.liquid`, or `blocks/g__menu-page.liquid` when a custom mobile layout is needed.
- [ ] Collapsible navigation works with `blocks/g__accordion.liquid` where used.
- [ ] Search entry works with `blocks/search-input.liquid` or `blocks/_layout__search.liquid`.
- [ ] Extra menu content renders through `blocks/_g__menu-item.liquid`, `blocks/g__menu-page.liquid`, or nested layout blocks such as `blocks/layout__flex.liquid`, `blocks/_g__flex-item.liquid`, `blocks/layout__grid.liquid`, and `blocks/_g__grid-item.liquid`.
- [ ] Drawer/sidebar menu behavior works when using `blocks/overlay__drawer.liquid` or `blocks/overlay__sidebar.liquid`.
- [ ] Consider adding visual hierarchy to the menu to highlight the purchase funnel.

## Footer

- [ ] Footer works on mobile; use visibility settings on duplicate footer blocks such as `blocks/logo.liquid`, `blocks/menu.liquid`, `blocks/social-icons.liquid`, `blocks/picker-language.liquid`, or `blocks/picker-country.liquid` when a custom mobile layout is needed.
- [ ] Consider showing navigation in collapsible containers such as `blocks/g__accordion.liquid`, `blocks/layout__tab.liquid`, `blocks/_g__tab-item.liquid`, `blocks/g__menu-dropdown.liquid`, `blocks/overlay__drawer.liquid`, `blocks/overlay__sidebar.liquid`, or `blocks/g__menu-page.liquid`.
- [ ] Navigation links render through `blocks/menu.liquid`.
- [ ] Language picker works with `blocks/picker-language.liquid`.
- [ ] Country/currency picker works with `blocks/picker-country.liquid`.
- [ ] Payment icons render with `blocks/payment-icons.liquid`.
- [ ] Social links render correctly with `blocks/social-icons.liquid` when present.
- [ ] Consider showing the logo in the footer with `blocks/logo.liquid`.
- [ ] Consider showing a newsletter signup in the footer with `blocks/newsletter.liquid` where available.

## Collection Page

- [ ] Product grid renders with `blocks/_layout__collection.liquid`.
- [ ] Product cards render with `blocks/g__product-card.liquid`.
- [ ] Promo/content blocks can appear in the grid through `blocks/_g__grid-item.liquid` with nested layout/content blocks such as `blocks/layout__flex.liquid`, `blocks/_g__flex-item.liquid`, `blocks/g__container.liquid`, `blocks/richtext.liquid`, `blocks/image.liquid`, or `blocks/g__button.liquid` where configured.
- [ ] Filter bar renders with `blocks/filter.liquid`.
- [ ] Filters work on mobile, including drawer behavior through `blocks/overlay__drawer.liquid`.
- [ ] Tag filters work with `blocks/filter-tags.liquid` where used.
- [ ] Pagination works with `blocks/_pagination.liquid` where used.
- [ ] Collection title renders on the page with `blocks/richtext.liquid`.
- [ ] Collection description renders near the bottom of the page for SEO content with `blocks/richtext.liquid` where appropriate.

## Product Page

- [ ] Gallery works with `blocks/product-media.liquid`, `blocks/_grid-gallery.liquid`, or `blocks/_slider-gallery.liquid`.
- [ ] Product details render with `blocks/richtext.liquid`, `blocks/product-price.liquid`, `blocks/product-badges.liquid`, `blocks/product-rating.liquid`, `blocks/product-buy.liquid`, `blocks/product-options.liquid`, and any relevant target-theme `blocks/product-*.liquid` blocks.
- [ ] Product options work with `blocks/product-options.liquid`.
- [ ] Swatches work with `blocks/product-swatches.liquid` where used.
- [ ] Sibling options work with `blocks/product-sibling-options.liquid` where used.
- [ ] Product buy/add-to-cart works with `blocks/product-buy.liquid`.
- [ ] Sticky product summary works through sticky layout settings on `blocks/_g__flex-item.liquid` or `blocks/g__container.liquid`.
- [ ] Size chart popup works through `blocks/overlay__popup.liquid`, `blocks/overlay__drawer.liquid`, or `blocks/overlay__sidebar.liquid` when configured.
- [ ] Accordions work with `blocks/g__accordion.liquid`.
- [ ] Tabs work with `blocks/layout__tab.liquid` and `blocks/_g__tab-item.liquid`.
- [ ] Related products render with `blocks/_grid-recommendations.liquid` or `blocks/_slider-recommendations.liquid`.
- [ ] Recently viewed products render with `blocks/_grid-recent.liquid` or `blocks/_slider-recent.liquid`.
- [ ] Upsells or bundles render with `blocks/product-bundles.liquid`, `blocks/checkbox-upsell.liquid`, `blocks/_grid-recommendations.liquid`, or `blocks/_slider-recommendations.liquid` where used.
- [ ] Pickup and subscription blocks work with `blocks/product-pickup.liquid` and `blocks/product-subscription.liquid` where present.

## Cart Page

- [ ] Cart results render with `blocks/cart-results.liquid`.
- [ ] Cart count renders with `blocks/cart-count.liquid`.
- [ ] Cart totals/summary render with `blocks/cart-summary.liquid` and `blocks/cart-fulltotals.liquid` where used.
- [ ] Cart progress bar works with `blocks/g__cart-progress.liquid` and `blocks/_g__cart-progress-tier.liquid`.
- [ ] Checkout action works with `blocks/g__checkout.liquid`.
- [ ] Checkout approval works with `blocks/checkout-approval.liquid` where used.
- [ ] Cart notes work with `blocks/cart-notes.liquid` where used.
- [ ] Discounts render with `blocks/cart-discount.liquid` or `blocks/shared-discount.liquid` where used.
- [ ] Cart share works with `blocks/cart-share.liquid` where used.
- [ ] Upsells render with `snippets/c__product-stack.liquid`, `blocks/checkbox-upsell.liquid`, `blocks/_grid-recommendations.liquid`, or `blocks/_slider-recommendations.liquid` where used.
