# List Page Reference

This is an explanation file for list page details. For copyable structure, use
`assets/page-shells/list.md`.

## List Surface

The toolbar, table, and pagination are one integrated white surface. Do not render them as
separate cards. The pale workspace background should remain visible around the surface.

## Toolbar

- Background: `#f9fbfd`.
- Padding: `10px 20px`.
- Layout: three sibling zones: left filters, center growing search, and right actions.
  Recommended hooks are `.ks-toolbar-left`, `.ks-toolbar-search`, and
  `.ks-toolbar-actions`.
- Use a stable layout such as `grid-template-columns: auto minmax(0, 1fr) auto`. The search
  grows only inside the center zone; it must not overlap the Select filter or extend under
  Refresh/Cogwheel/Create.
- The project Select should occupy a stable `148px x 32px` slot. Set both the Select root
  and `.kubed-select-selector` to the expected width/height and use `box-sizing:
  border-box` so the search field does not visually press into the selector border.
- Filter/select controls should use kube-design `Select` visual output. Do not apply native
  `select` CSS such as `appearance: none`, `background-image` chevrons, or raw option
  styling to a kube-design `Select`.
- Toolbar select filters must show the kube-design dropdown arrow. Use `showArrow` when the
  installed API supports it, and leave right padding for `.kubed-select-arrow`.
- If the arrow is missing in the rendered page, add an explicit
  `suffixIcon={<ChevronDown size={16} color="#324558" fill="#b6c2cd" />}` and ensure
  `.kubed-select-arrow` remains visible and right-aligned. Do not let the select's internal
  search input or custom padding cover the arrow.
- Search uses public kube-design `FilterInput` when exported. For plain keyword search,
  use `simpleMode`. It should keep the component interaction: hover border, focused white
  background, subtle border, search icon at `left: 12px`, and `padding: 5px 40px`.
- The `FilterInput` wrapper should be `32px` high and confined to the center search zone.
  If it visually overlaps the Select filter or action buttons, the toolbar layout fails
  even if the input itself looks polished.
- Do not set a positive fixed `min-width` on `.ks-filter-input`; values such as `220px`
  can force overlap at narrower content widths. Use `min-width: 0`, `width: 100%`, and
  `max-width: none` on the FilterInput wrapper.
- For the list toolbar search, the internal `.filter-input` should be regular and muted:
  `font-weight: 400`, `color: #79879c`, placeholder `#79879c`.
- Kube-design component styles can win over low-specificity CSS. If the computed
  `FilterInput` text remains bold or primary-colored, scope the override to
  `.ks-filter-input input.filter-input` and use higher specificity or `!important` only on
  `font-weight`, `color`, and placeholder color.
- Do not confuse the `Select` internal `.kubed-select-selection-search-input` with the
  real toolbar search input. The real search input is identified by the search placeholder
  or `.filter-input`.
- Only hand-write a FilterInput-style wrapper when the installed package lacks
  `FilterInput`.
- Right actions are fixed: Refresh, Cogwheel/custom columns, dark text-only primary action.
- Refresh and Cogwheel are kube-design text buttons with icon only: `32px` high,
  `0 20px` horizontal padding, about `56px` effective width. They are not square buttons.
- Refresh and Cogwheel must be sibling buttons in the right actions zone. Do not implement
  them as search suffix icons, input adornments, absolutely positioned children inside the
  FilterInput, or controls visually embedded at the right edge of the search field.
- The dark primary action uses kube-design `Button` with `color="secondary"` and `shadow`.
  Do not rely on the default Button shadow value.

## Table

- Use `8-10` visible rows by default for generated mock data unless the prompt asks for a
  smaller sample.
- Table main inset is required: `0 12px 12px`.
- Header background is white, not blue or strong gray.
- Rows are compact and stable, about `56px`.
- First data column uses Object Identity Pattern: `40px` icon + `12px` gap + title and
  description stack.
- Object identity title is intentionally bold `700` with primary text color. Description
  text is regular `400` and secondary color.
- Status labels next to status dots are semibold `600` and primary text color.
- Table identity icons are usually default/inactive duotone, not active green.
- The `40px` identity icon area is for sizing and alignment only. Do not add a gray tile,
  card, border, or background behind the icon unless the actual icon asset includes it.
- Keep the row action column narrow and stable. The `More` trigger must stay centered
  inside the action cell and must not be clipped by table overflow, cell padding, or an
  absolutely positioned menu wrapper.
- Row `More` is a kube-design text button with icon only, `32px` high and `0 20px`
  horizontal padding. Do not force it into a square.
- The row action cell still uses normal table cell padding `0 12px`. Because Row `More` is
  about `56px` wide, set the action column to at least `80px`.
- Row operation menu items use normal menu text color, including Delete/删除. Reserve
  danger/error color for destructive confirmation buttons.
- Avoid inline width styles on every header cell. Prefer reusable column classes or a
  column definition so column sizing stays consistent across resources.

## Selection

Selected rows use pale cells and a thin green outline. Do not use thick side bars, blue
fills, mint fills, or oversized effects.

## Pagination

Use attached footer: page size, divider, total, previous icon, `1 / N`, next icon. Do not
use numbered buttons for the generic list pattern.

The page-size control and previous/next controls use `4px` radius, not pill radius.
Previous/next are kube-design text buttons with `32px` height and `5px 12px` padding. Keep
footer padding and `overflow: visible` so the rightmost next button is fully visible and
not clipped by the list surface edge.

For the page-size control, split label, value, and chevron into separate children. Align
the kube-design Button inner wrappers with inline-flex center and about `4px` gap so
`Rows per page: 10` / `每页行数: 10` and the chevron do not look glued together or
vertically offset.

## Residual Scaffold

Generated projects should not retain unused Vite/React starter CSS, hero assets, locale
switcher styles, or unrelated demo selectors. Remove unused scaffold files/styles before
final review.
