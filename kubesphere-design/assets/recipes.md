# Recipes

This is an execution table. Follow it directly before inventing UI choices.

Before applying any recipe, import primitives from public kube-design packages. Recipes do
not authorize local clones of kube-design components or icons.

Using `@kubed/icons` alone is insufficient. The visible controls in a list page should also
come from `@kubed/components`.

## Resource Recipes

Use these recipes before generating resource list pages.

| Resource | Chinese Title | Key | Icon | Active Menu | Default Columns | Identity Secondary |
|---|---|---|---|---|---|---|
| Deployments | 部署 | `deployments` | `Backup` | Workloads > Deployments | Status, Project, Image, Pods, Updated Time | Project |
| Pods | 容器组 | `pods` | `PodDuotone` | Workloads > Pods | Status, Project, Node, Pod IP, CPU/Memory, Restarts, Age | Pod IP or Node |
| Services | 服务 | `services` | `Appcenter` | Service And Network > Services | Status, Project, Type, Cluster IP, Ports, Age | Project |
| Ingresses | 应用路由 | `ingresses` | `Loadbalancer` | Service And Network > Ingresses | Status, Project, Host, Path, Service, Age | Project |
| Gateway | 网关 | `gateway` | `GatewayDuotone` | Service And Network > Gateway | Status, Project, Class, Address, Listeners, Age | Project |

Mock names should look like real Kubernetes resources. Examples:

- Deployments: `nginx-frontend`, `api-gateway`, `redis-master`.
- Pods: `nginx-frontend-7889cb9c7f-hjsd2`, `coredns-65c54cc984-plh52`.
- Services: `frontend-service`, `api-gateway`, `redis-master`.
- Gateway: `public-gateway`, `internal-gateway`, `edge-gateway`.

Generated basic list pages should normally show `8-10` visible rows so the table density,
row separators, selected-row state, and pagination can be judged. Shorter snippets in
examples are abbreviated; do not treat them as the desired page density unless the user asks
for a small data set. Basic demos start with no selected rows. Implement row checkbox
interaction so the reviewer can select and unselect rows and verify the selected-row
outline.

## Button Recipes

Use real kube-design `Button` when available. In a new project, add `@kubed/components`
instead of writing a local `Button` component.

| Scenario | Component | Icon | Text | Notes |
|---|---|---|---|---|
| Primary create/action | `Button variant="filled" color="secondary" radius="xl" size="sm" shadow` | No icon by default | Create / 创建 | Dark button, text only, shadow required |
| Secondary action | `Button variant="filled" color="default" radius="xl" size="sm"` | No icon | Cancel / 取消 | Used for cancel/back |
| Toolbar refresh | `Button variant="text"` | `Refresh size={16}` | aria-label only | `32px` high, icon-only, `0 20px` horizontal padding |
| Toolbar column settings | `Dropdown maxWidth={160}` + `Button variant="text"` + `Menu width={160}` rows | `Cogwheel size={16}` trigger, `Eye` / `EyeClosed size={16}` rows | aria-label only | Fixed after Refresh; opens DataTable settings menu; `32px` high, `0 20px` horizontal padding |
| Row more | `Button variant="text" radius="xl" size="sm"` | `More size={16}` | aria-label only | `32px` high, `0 20px` horizontal padding |
| Pagination previous/next | `Button variant="text" radius="sm"` | `Previous/Next size={20}` | aria-label only | Exact `32px x 32px`, `0` padding, `4px` radius, no numbered buttons |
| Dangerous confirm | `Button variant="filled" color="error" radius="xl" size="sm"` | No icon | Delete / 删除 | Destructive confirmation only |

Button controls are generally `32px` high. Text actions use about `0 20px` horizontal
padding. Toolbar Refresh, Cogwheel, and table Row More are still text buttons: keep the
button height at `32px`, use a `16px` icon, and preserve `0 20px` horizontal padding even
though there is no visible text. These icon-only text buttons have no shadow at rest.
Primary create/action buttons must explicitly pass `shadow`; kube-design `Button` does not
enable command-button shadow by default. Do not use green create/confirm buttons.

Pagination previous/next controls are not pill buttons and should not be forced into custom
square raw buttons. Use kube-design `Button variant="text" radius="sm"` with exact
`32px x 32px`, `0` padding, `4px` radius, no shadow, and visible overflow so the rightmost
button does not clip.

## Toolbar Recipe

List toolbar order is fixed:

```text
[left filters/selects] [growing FilterInput] [right actions: Refresh] [Cogwheel] [dark primary action]
```

- Implement the toolbar as three sibling zones: left filters, center search, and right
  actions. Recommended class hooks are `.ks-toolbar-left`, `.ks-toolbar-search`, and
  `.ks-toolbar-actions`. Do not place Refresh, Cogwheel, or Create inside the FilterInput
  wrapper or search zone.
- The toolbar wrapper uses `padding: 10px 20px`, `background: #f9fbfd`, top radius `4px`,
  and a subtle bottom divider via `box-shadow: 0 1px 0 0 #e3e9ef`. Prefer this product
  divider over an extra table-like border when composing a local list surface.
- The center search grows only inside `.ks-toolbar-search`; it must not overlay the Select
  filter on the left or the action buttons on the right. Use `grid-template-columns: auto
  minmax(0, 1fr) auto`, `min-width: 0` on the search zone, and `flex: 0 0 auto` /
  `white-space: nowrap` on the actions zone.
- The project Select should reserve a stable `240px x 32px` slot. Apply the size to both
  `.ks-project-filter` and its `.kubed-select-selector`; keep the selector `box-sizing:
  border-box`, full width, and vertically centered.
- Search must use public kube-design `FilterInput` when exported. Use `simpleMode` for a
  plain keyword search field. Its product interaction is `32px` high, `18px` radius,
  `padding: 5px 40px`, search icon at `left: 12px`, hover border, and focused white
  background with subtle border.
- For toolbar search, override the internal `.filter-input` text to regular weight and
  secondary color: `font-weight: 400`, `color: #79879c`. Placeholder color should also be
  `#79879c`.
- Kube-design styles may override the toolbar search input. Scope the override to the real
  `FilterInput` input, for example `.ks-filter-input input.filter-input`, and increase
  selector specificity or use `!important` only for `font-weight`, `color`, and
  placeholder color when necessary.
- Do not inspect or style `.kubed-select-selection-search-input` as the toolbar search
  field. That input belongs to `Select`; the toolbar search is the `FilterInput` input with
  the search placeholder or `.filter-input` class.
- A local FilterInput-style wrapper is allowed only if the installed package does not export
  `FilterInput`; it must reproduce the hover/focus states above.
- Search must flex through available center-toolbar space. Do not cap it with a narrow
  fixed `max-width` such as `600px`, but also do not let it span underneath or behind
  Refresh, Cogwheel, or Create.
- Do not give `.ks-filter-input` a positive fixed `min-width` such as `220px`. The search
  zone already controls available space; the FilterInput itself should use `width: 100%`,
  `min-width: 0`, and `max-width: none` so it can shrink without pushing into the Select or
  action zones.
- Project/resource filters use kube-design `Select` styling. Do not apply native select CSS
  (`appearance`, custom data-URI chevrons, `option` styling) to kube-design `Select`.
- Toolbar `Select` filters should show the kube-design arrow. Pass `showArrow` when the
  API supports it, reserve right padding for the arrow, and never hide or replace
  `.kubed-select-arrow`.
- If the arrow is still invisible, pass an explicit suffix icon:
  `<ChevronDown size={16} color="#324558" fill="#b6c2cd" />`. Keep
  `.kubed-select-arrow` visible, right-aligned, and above the selector text; do not cover
  it with selection search input width or custom overflow styles.
- Refresh and Cogwheel are icon-only text buttons, borderless/backgroundless at rest.
- Refresh and Cogwheel are always separate toolbar action buttons to the right of the
  FilterInput. They must not be rendered as suffix icons, adornments, absolute-positioned
  children, or visually embedded controls inside the search input.
- Cogwheel is not decorative. Wrap it with kube-design `Dropdown placement="bottom-end"
  maxWidth={160}` and render the DataTable settings menu using `Menu width={160}
  className="menu-setting"`, `MenuLabel`, and `MenuItem`. The menu is dark, `4px` radius,
  `padding: 4px 0`, shadowed, with title `定制内容` / `Custom Columns`; label padding is
  `8px 12px`. Rows use `padding: 6px 12px`, regular `400` text, dark-hover background,
  `Eye size={16}` for visible columns, and `EyeClosed size={16}` for hidden columns. Prefer
  the stable structure `.menu-setting` -> `.menu-label` -> row buttons with `.item-inner`,
  `.item-icon`, `.item-body`, and `.item-label`; do not copy generated `sc-*` hash class
  names. Do not use Checkbox controls, blue selected backgrounds, or a modal. The menu
  toggles ordinary data columns such as Status, Project, Image, Pods, and Updated Time. The
  Object Identity first data column (`Name` / `名称`) and final operation column are fixed
  anchors: never show them in the column menu and never hide them.
- The Cogwheel trigger must be a true toggle: first click opens the column menu, second
  click on the Cogwheel closes it. Do not pass `hideOnClick={false}` or similar props that
  prevent the trigger from closing its own dropdown. If a column row click hides the menu in
  a package version, prefer the default product Dropdown behavior over custom popover state.
- Create/action button is dark, text-only by default, and must pass `shadow`.

## Table Recipe

- Table main inset: `margin: 0 12px 12px`.
- Use a source-like table model: `border-collapse: collapse`, `border-spacing: 0`, full
  width, left-aligned text. Avoid `border-collapse: separate` unless the consuming table
  primitive forces it.
- Header: white background, `padding: 16px 12px`, `12px` muted `600` text,
  `line-height: 1.67`, and no strong tinted fill.
- Body cells: `height: 56px`, `padding: 8px 12px`, `border-top: 1px solid #eff4f9`,
  `12px` text, regular `400`, primary color `#242e42`, `line-height: 1.67`, and
  `box-sizing: border-box`.
- Checkbox/select column is fixed at `40px`. For local table shells, use a `colgroup` and
  `.ks-select-cell` so the checkbox column does not drift.
- The first data column and final operation column are fixed. Column customization may hide
  only ordinary data columns between them. When a column is hidden, remove both its header
  cell and body cells, and keep `colgroup` aligned with the visible columns.
- Normal single-line body cells use primary text color `#242e42` and regular `400` weight.
  Do not apply muted secondary color or bold weight to ordinary values such as project,
  image, replica count, or updated time. Use secondary color only for description/helper
  text inside intentional two-line stacks.
- Normal row hover uses subtle pale blue-gray cells, usually `#eff4f9` / `var(--ks-hover)`.
  Do not change text weight, add a side bar, or override the selected-row outline on hover.
- First data column: Object Identity Pattern, `40px` semantic icon + `12px` gap + text
  stack.
- Object identity primary text is bold `700`, `#242e42`, and ellipsized. Secondary text is
  regular `400`, `#79879c`, and ellipsized.
- Table identity icons use default/inactive duotone, not active green.
- Object identity icon containers provide only layout size. No gray background, tile, card,
  or border behind the icon.
- Row more action: borderless `More size={16}` inside text button with `0 20px` horizontal
  padding.
- Row more action cell keeps normal table cell horizontal padding: `12px` left and right.
  Since the button is about `56px` wide, the action column is fixed at `80px`. The trigger
  must not be clipped or overlap the table edge.
- Row action dropdown is right-aligned to the More trigger: use kube-design
  `Dropdown placement="bottom-end"` and keep `maxWidth` around `160px`, so the menu panel's
  right edge aligns with the More button's right edge. Constrain `.ks-row-menu` and its
  inner menu element to the same `160px` width; do not let the Menu overflow outside the
  aligned tippy panel.
- Row action menu item text uses normal menu text color, including Delete/删除. Do not use
  semantic error color inside table row operation menus; reserve danger color for destructive
  confirmation buttons.
- Selected row: pale cells plus thin green outline; no left bar or mint fill.
- Prefer source-like real cell borders for selected rows. Apply `border-top:
  1px solid #55bc8a` and `background: #eff4f9` to every selected cell, add
  `border-left: 1px solid #55bc8a` on the first cell, and add `border-right:
  1px solid #55bc8a` on the last cell. If the selected row is the last row, add
  `border-bottom: 1px solid #55bc8a`.
- Consecutive selected rows should not draw double green lines. For `tr.row-selected +
  .row-selected`, make the later row's `td` top border transparent. For `tr.row-selected +
  tr`, make the next row's top border green so the selected frame remains closed.
- Selected row CSS must win over hover/focus. Apply selected styles to `tr.row-selected td`
  and `tr.row-selected:hover td`, including first/last cell edges, so a hovered selected row
  still shows one continuous green rectangular outline.
- Checkbox checked state and row selected state must be synchronized from the same selected
  data. Do not render a checked `Checkbox` while leaving the row without `row-selected`.
  Add a CSS fallback for interactive/static demos:
  `tr:has(input[type="checkbox"]:checked) td` should receive the same selected-cell frame.
- Basic demos should start with no selected rows. Do not preselect rows just to show the
  selected state; support checkbox select/unselect instead.
- Do not add status summary cards above the list for the basic resource list pattern.
- Do not add CPU/memory progress bars unless the resource recipe or user prompt explicitly
  asks for metric visualization; plain compact text is safer for generic list fidelity.

## Status Recipe

| Status | Dot | English | Chinese |
|---|---:|---|---|
| Running | `#55bc8a` | Running | 运行中 |
| Updating | `#f5a623` | Updating | 更新中 |
| Error | `#ca2621` | Error | 异常 |
| Waiting | `#b6c2cd` | Waiting | 等待中 |

Running must never use gray or blue-gray.

Status text next to the dot is semibold `600` and primary text color `#242e42`. Do not
render status labels as muted helper text.

## Pagination Recipe

Use attached KubeSphere pagination:

```text
[Rows per page / 每页显示] [10] [divider] [Total: N / 总数：N]        [Previous] [1 / N] [Next]
```

- Footer wrapper: `padding: 10px 20px`, `background: #f9fbfd`, bottom radius `4px`, and
  muted base color `#79879c`. Prefer this source-like attached footer over a fixed-height
  toolbar clone.
- Page-size label text: Chinese `每页显示`, English `Rows per page`. Do not use
  `每页行数`.
- Page-size default value is `10`; available values are `10`, `20`, `50`, and `100`.
- Page-size dropdown visual panel outer width is fixed at `96px`. Use `0` popover content
  padding, a `96px` menu, `8px` menu side padding, and `80px` menu options. Options should
  not add extra left/right margin on top of the menu gutter.
- Total text: Chinese `总数：9`, English `Total: 9`. Do not use `共 9 条记录` for the
  generic list footer.
- Page-size control is a light trigger, not a prominent command button. It may be a
  styled inline-flex trigger or a kube-design text Button when needed for portability. Keep
  it `32px` high with `5px 8px` padding, `4px` radius, semibold `12px`, primary-ish text
  `#36435c`, and about `4px` gap between label, value, and chevron.
- Previous/next buttons are exact `32px x 32px` kube-design text buttons with `0` padding,
  `4px` radius, no shadow, and centered `Previous`/`Next` icons. The footer must keep enough
  right padding and `overflow: visible` so the last button is not clipped or flush against
  the surface edge.
- No numbered page buttons.
