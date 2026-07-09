# Component Reference

This is an explanation file. For direct execution tables, use `assets/recipes.md`.

## Buttons

Use kube-design `Button` roles from `assets/recipes.md` when available. If the installed
API differs, keep the visual role and adapt prop names. Primary commands are dark,
text-only by default, and must explicitly pass `shadow`; kube-design does not enable that
shadow by default. Dangerous confirmations use error color. Toolbar and row icon actions
are borderless/backgroundless and no-shadow at rest.

Do not treat all icon-only buttons the same. Toolbar Refresh/Cogwheel and table Row More
are kube-design text buttons with icon only, `32px` height, and `0 20px` horizontal
padding. Pagination arrows are kube-design text buttons with exact `32px x 32px`, `0`
padding, and `4px` radius, not pill buttons or wide text buttons. Pagination page-size
controls also use `4px` radius. Their dropdown visual panel is fixed at `96px` width with
`0` popover content padding; the menu is `96px` wide with `8px` side padding, and the
options are `80px` wide with no extra side margin.

Do not create a local `Button` abstraction that maps custom class names to visual variants.
That usually produces a page that looks close in code but diverges in focus, hover, sizing,
and disabled behavior. For a new React app, add `@kubed/components`; for an existing app,
reuse its installed kube-design import path.

Do not install `@kubed/components` and then ignore it. A list page that implements toolbar
buttons, command buttons, checkbox selection, dropdown menus, and pagination controls with
raw `button`, `input type="checkbox"`, `select`, and custom CSS still fails the component
contract.

Allowed raw HTML elements are structural wrappers: `header`, `aside`, `main`, `section`,
`nav`, `table`, `thead`, `tbody`, `tr`, `th`, and `td`. Interactive controls should use
kube-design components unless no public primitive exists for that exact control.

When a kube-design component is used, style only the outer layout hook unless a small size
correction is required. Do not overwrite its internal control behavior with native-control
CSS. Examples to avoid:

- applying `appearance: none`, data-URI chevrons, or raw `option` styling to `Select`;
- restyling `Button` into a custom bordered chip when the recipe already specifies a
  kube-design variant;
- recreating checkbox visuals with `.ks-checkmark` CSS after using `Checkbox`;
- positioning menu popovers manually after using `Dropdown`.

## Icons

Use `@kubed/icons` for navigation and resources. KubeSphere icons are duotone. When
overriding icon color, set both `color` and `fill`. Avoid lucide icons, emoji, CSS masks,
monochrome placeholders, and hand-drawn blocks.

Inactive duotone icons should keep both channels visible. Passing only a muted `color`,
setting `fill="transparent"`, or wrapping the icon in a gray tile often makes the icon look
like the wrong asset.

Do not create `icons.tsx` with hand-written SVG approximations of `Cluster`, `Workspace`,
`PodDuotone`, `Grid2Duotone`, `GatewayDuotone`, or other mapped icons. If the icon package
is unavailable, install or request access to public `@kubed/icons`; otherwise clearly mark
the output as an approximation.

## Object Identity

Use for table name columns, list cards, selector rows, modal record rows, and detail
headers:

```text
[40px semantic duotone icon] 12px gap [primary line + secondary line]
```

Primary line is bold `700` and `#242e42`. Secondary line is regular `400` and `#79879c`.
Both should ellipsis rather than wrap.

The icon slot is a `40px` alignment box, not a card. Do not add a gray background, border,
or mini-tile behind the resource icon.

For ordinary table cells outside the Object Identity pattern, use primary text color and
regular `400` weight. Do not apply secondary color to normal values such as updated time.

## Dropdowns And Menus

Use compact overflow menus. The row action trigger is `More size={16}` inside a text
button. Table row operation menu items should use normal menu text color, including
Delete/删除. Use error color for destructive confirmation buttons, not for ordinary table
row menu text.

The trigger must remain visible and centered in the fixed `80px` action cell. Avoid
clipping caused by `overflow: hidden`, too-narrow action columns, or dropdown wrappers that
exceed the cell.

Use `Dropdown placement="bottom-end"` for row action menus so the panel's right edge aligns
with the More trigger's right edge. Keep compact row menus around `160px` max width, and
constrain the inner `.ks-row-menu` / menu node to the same width so it cannot overflow the
aligned tippy panel.

## Form Controls

For now, only basic list filters are covered. Future create/edit flows should add specific
form layout rules before broad form generation is considered stable.

Use public kube-design `FilterInput` for toolbar search when exported. Use `simpleMode`
for a plain keyword field. Do not replace it with a local `label + input` unless the
installed package truly lacks `FilterInput`; the local fallback must reproduce the
component's hover/focus behavior, `padding: 5px 40px`, and icon placement.

For list toolbar search, override only the text layer if needed: `.filter-input` should use
regular weight and secondary text color (`#79879c`) so it matches description text rather
than table title text.

When checking or styling toolbar search, target the real `FilterInput` input, such as
`.ks-filter-input input.filter-input`, or the input with the search placeholder. Do not
target `.kubed-select-selection-search-input`; that belongs to kube-design `Select`. If the
installed component's CSS keeps the search input bold or primary-colored, use higher
specificity or `!important` only for `font-weight`, `color`, and placeholder color.

For toolbar `Select`, keep the kube-design arrow visible. Use `showArrow`; if the installed
version still renders no visible arrow, pass an explicit `suffixIcon` with
`ChevronDown size={16}` and default duotone colors. Do not cover `.kubed-select-arrow` with
custom selector padding or search-input width.

Column-management dialogs, delete confirmation modals, toast notifications, and runtime
locale switches are outside the basic list-page recipe unless explicitly requested. For
basic list pages, use only the toolbar Cogwheel's compact column visibility dropdown:
toggle normal data columns, keep the first Object Identity column and final operation
column fixed, and avoid adding a larger settings modal. The column visibility dropdown
uses the DataTable settings menu shape: `Dropdown placement="bottom-end" maxWidth={160}`,
`Menu width={160} className="menu-setting"`, `MenuLabel`, and `MenuItem` rows. The menu is
dark, `4px` radius, `padding: 4px 0`, shadowed, with title `定制内容` / `Custom Columns`;
label padding is `8px 12px`. Rows use `padding: 6px 12px`, regular `400` text,
dark-hover background, `Eye size={16}` for visible columns, and `EyeClosed size={16}` for
hidden columns. Preserve the stable semantic structure `.menu-setting`, `.menu-label`,
`.item-inner`, `.item-icon`, `.item-body`, and `.item-label`; do not copy generated
`sc-*` hash classes. Do not render checkbox rows, blue selected panels, or a modal for this
control.

Keep the trigger interaction native to kube-design Dropdown: clicking the Cogwheel opens the
menu, and clicking the same Cogwheel again closes it. Do not set `hideOnClick={false}` on
the column-settings Dropdown and do not replace this with custom popover state unless the
installed package lacks a working Dropdown.

## Typography

Use the kube-design/KubeSphere font stack from `assets/tokens.md` for custom shell text,
tables, object identity cells, and local wrappers. Do not use `Inter`, `Geist`, or
generic `system-ui` as the leading font family; those make generated pages look like a
generic AI admin template and visually diverge from kube-design controls.
