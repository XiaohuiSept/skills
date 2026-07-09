# List Page Reference

This is an explanation file for list page details. For copyable structure, use
`assets/page-shells/list.md`.

## List Surface

The toolbar, table, and pagination are one integrated white surface. Do not render them as
separate cards. The pale workspace background should remain visible around the surface.

## Toolbar

- Background: `#f9fbfd`.
- Padding: `10px 20px`.
- Top radius: `4px`.
- Divider: prefer `box-shadow: 0 1px 0 0 #e3e9ef` on the toolbar wrapper instead of
  adding a heavy table-like border.
- Layout: three sibling zones: left filters, center growing search, and right actions.
  Recommended hooks are `.ks-toolbar-left`, `.ks-toolbar-search`, and
  `.ks-toolbar-actions`.
- Use a stable layout such as `grid-template-columns: auto minmax(0, 1fr) auto`. The search
  grows only inside the center zone; it must not overlap the Select filter or extend under
  Refresh/Cogwheel/Create.
- The project Select should occupy a stable `240px x 32px` slot. Set both the Select root
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
- Cogwheel opens the compact DataTable settings menu. Use kube-design
  `Dropdown placement="bottom-end" maxWidth={160}` with `Menu width={160}
  className="menu-setting"`, `MenuLabel`, and `MenuItem` rows. The menu is dark, `4px`
  radius, `padding: 4px 0`, shadowed, with title `定制内容` / `Custom Columns`; label
  padding is `8px 12px`. Rows use `padding: 6px 12px`, regular `400` text, dark-hover
  background, `Eye size={16}` for visible columns, and `EyeClosed size={16}` for hidden
  columns. Preserve the product-like structure `.menu-setting`, `.menu-label`,
  `.item-inner`, `.item-icon`, `.item-body`, and `.item-label`; ignore generated `sc-*`
  hash classes. Do not use checkboxes, blue selected backgrounds, or a modal for this
  control. It toggles normal data columns only; the Object Identity `Name` / `名称` column
  and final operation column are fixed and should never appear in the settings menu.
- The dark primary action uses kube-design `Button` with `color="secondary"` and `shadow`.
  Do not rely on the default Button shadow value.

## Table

- Use `8-10` visible rows by default for generated mock data unless the prompt asks for a
  smaller sample.
- Basic demos start with no selected rows. Implement row checkbox interaction so users can
  select and unselect rows and verify the selected outline during review.
- Table main inset is required: `0 12px 12px`.
- Use a source-like table model: `border-collapse: collapse`, `border-spacing: 0`, full
  width, left-aligned text. Avoid `border-collapse: separate` for local table shells unless
  a consuming primitive forces it.
- Header background is white, not blue or strong gray.
- Header cells use `padding: 16px 12px`, `font-size: 12px`, `font-weight: 600`,
  `line-height: 1.67`, and muted text color `#79879c`.
- Rows are compact and stable, about `56px`.
- Body cells use `height: 56px`, `padding: 8px 12px`,
  `border-top: 1px solid #eff4f9`, primary text color `#242e42`, regular `400`, and
  `line-height: 1.67`.
- Checkbox/select column is fixed at `40px`; action column is fixed at `80px`. In local
  table shells, prefer `colgroup` plus `.ks-select-cell` / `.ks-action-cell` so browser
  table layout does not stretch these utility columns.
- Column visibility changes must keep table structure coherent: render matching `colgroup`,
  `th`, and `td` entries for visible data columns. Keep the checkbox column, first data
  Object Identity column, and final operation column visible at all times.
- Normal single-line body cells use primary text color `#242e42` and regular `400` weight.
  Do not mute or bold ordinary values such as updated time, project, image, or replica
  counts. Secondary text color is for helper/description lines inside intentional two-line
  stacks.
- Normal rows need a hover state: apply subtle pale blue-gray cell background
  (`#eff4f9` / `var(--ks-hover)`) to normal row cells only. Hover must not change text
  weight, create a side bar, or override the selected row outline.
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
  about `56px` wide, set the action column to exactly `80px`.
- The row action dropdown must use `placement="bottom-end"` so the menu panel's right edge
  aligns with the More button's right edge. Keep `maxWidth` around `160px` for compact row
  operation menus, and also constrain the inner `.ks-row-menu` / menu node to `160px`.
  If the inner menu remains at its default wider width, it will overflow the aligned panel
  and still look visually misaligned.
- Row operation menu items use normal menu text color, including Delete/删除. Reserve
  danger/error color for destructive confirmation buttons.
- Avoid inline width styles on every header cell. Prefer reusable column classes or a
  column definition so column sizing stays consistent across resources.

## Selection

Selected rows use pale cells and a thin green outline. Do not use thick side bars, blue
fills, mint fills, or oversized effects.

Selected-row styling must override hover/focus styling. Define both `tr.row-selected td`
and `tr.row-selected:hover td`, including the first and last cell edges, so a hovered
selected row still appears as one green rectangular outline.

Prefer the source-like border model with `border-collapse: collapse`: every selected cell
gets `border-top: 1px solid #55bc8a` and pale background, the first selected cell gets
`border-left: 1px solid #55bc8a`, and the last selected cell gets `border-right:
1px solid #55bc8a`. If the selected row is the last row, add `border-bottom:
1px solid #55bc8a`. For consecutive selected rows, make the later selected row's top
border transparent to avoid double green lines; for the next non-selected row after a
selected row, set its top border green so the frame closes visually.

Checkbox checked state and row selected state must be synchronized from the same data. Do
not render a green checked checkbox while leaving its row without `row-selected`. For static
or interactive demos, add a CSS fallback so `tr:has(input[type="checkbox"]:checked) td`
receives the same selected-cell frame.

## Pagination

Use attached footer: page size, divider, total, previous icon, `1 / N`, next icon. Do not
use numbered buttons for the generic list pattern. The footer wrapper uses `padding:
10px 20px`, `background: #f9fbfd`, bottom radius `4px`, and muted base color `#79879c`.

Page-size label text is Chinese `每页显示` or English `Rows per page`. Default value is
`10`, and the allowed values are `10`, `20`, `50`, and `100`. Total text is Chinese
`总数：9` or English `Total: 9`. Do not use `每页行数` or `共 9 条记录` in the generic list
footer.

The page-size dropdown visual panel outer width is fixed at `96px` with `0` popover content
padding. The menu is also `96px` wide, uses `8px` side padding, and contains `80px`
options with no extra option side margin. Do not allow kube-design's default menu width to
expand to the trigger width or a wide `200px+` panel for the four numeric options.

The page-size control and previous/next controls use `4px` radius, not pill radius.
Page-size is a lightweight trigger, not a prominent command button; it may be a styled
inline-flex trigger or a kube-design text Button when portability requires. Use `32px`
height, `5px 8px` padding, semibold `12px`, primary-ish text `#36435c`, and centered
children. Previous/next are exact `32px x 32px` kube-design text buttons with `0` padding
and centered icons. Keep footer padding and `overflow: visible` so the rightmost next
button is fully visible and not clipped by the list surface edge.

For the page-size control, split label, value, and chevron into separate children. Align
the kube-design Button inner wrappers with inline-flex center and about `4px` gap so
`Rows per page 10` / `每页显示 10` and the chevron do not look glued together or
vertically offset.

## Residual Scaffold

Generated projects should not retain unused Vite/React starter CSS, hero assets, locale
switcher styles, or unrelated demo selectors. Remove unused scaffold files/styles before
final review.
