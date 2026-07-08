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
for a small data set.

## Button Recipes

Use real kube-design `Button` when available. In a new project, add `@kubed/components`
instead of writing a local `Button` component.

| Scenario | Component | Icon | Text | Notes |
|---|---|---|---|---|
| Primary create/action | `Button variant="filled" color="secondary" radius="xl" size="sm" shadow` | No icon by default | Create / 创建 | Dark button, text only, shadow required |
| Secondary action | `Button variant="filled" color="default" radius="xl" size="sm"` | No icon | Cancel / 取消 | Used for cancel/back |
| Toolbar refresh | `Button variant="text"` | `Refresh size={16}` | aria-label only | `32px` high, icon-only, `0 20px` horizontal padding |
| Toolbar column settings | `Button variant="text"` | `Cogwheel size={16}` | aria-label only | Fixed after Refresh; `32px` high, `0 20px` horizontal padding |
| Row more | `Button variant="text" radius="xl" size="sm"` | `More size={16}` | aria-label only | `32px` high, `0 20px` horizontal padding |
| Pagination previous/next | `Button variant="text" radius="sm"` | `Previous/Next size={20}` | aria-label only | `32px` high, `5px 12px` padding, `4px` radius, no numbered buttons |
| Dangerous confirm | `Button variant="filled" color="error" radius="xl" size="sm"` | No icon | Delete / 删除 | Destructive confirmation only |

Button controls are generally `32px` high. Text actions use about `0 20px` horizontal
padding. Toolbar Refresh, Cogwheel, and table Row More are still text buttons: keep the
button height at `32px`, use a `16px` icon, and preserve `0 20px` horizontal padding even
though there is no visible text. These icon-only text buttons have no shadow at rest.
Primary create/action buttons must explicitly pass `shadow`; kube-design `Button` does not
enable command-button shadow by default. Do not use green create/confirm buttons.

Pagination previous/next controls are not pill buttons and should not be forced into custom
square raw buttons. Use kube-design `Button variant="text" radius="sm"` with `32px` height,
`5px 12px` padding, `4px` radius, no shadow, and visible overflow so the rightmost button
does not clip.

## Toolbar Recipe

List toolbar order is fixed:

```text
[left filters/selects] [growing FilterInput] [Refresh] [Cogwheel] [dark primary action]
```

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
- Search must flex through available toolbar space. Do not cap it with a narrow fixed
  `max-width` such as `600px`.
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
- Create/action button is dark, text-only by default, and must pass `shadow`.

## Table Recipe

- Table main inset: `margin: 0 12px 12px`.
- Header: white background, about `44px` high, `12px` muted bold text.
- Rows: about `56px` high.
- First data column: Object Identity Pattern, `40px` semantic icon + `12px` gap + text
  stack.
- Object identity primary text is bold `700`, `#242e42`, and ellipsized. Secondary text is
  regular `400`, `#79879c`, and ellipsized.
- Table identity icons use default/inactive duotone, not active green.
- Object identity icon containers provide only layout size. No gray background, tile, card,
  or border behind the icon.
- Row more action: borderless `More size={16}` inside text button with `0 20px` horizontal
  padding.
- Row more action cell keeps normal table cell horizontal padding: `padding: 0 12px`. Since
  the button is about `56px` wide, the action column should be at least `80px`. The trigger
  must not be clipped or overlap the table edge.
- Row action menu item text uses normal menu text color, including Delete/删除. Do not use
  semantic error color inside table row operation menus; reserve danger color for destructive
  confirmation buttons.
- Selected row: pale cells plus thin green outline; no left bar or mint fill.
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
[Rows per page / 每页行数] [divider] [Total]        [Previous] [1 / N] [Next]
```

- Footer background: `#f9fbfd`.
- Border top: `1px solid #e3e9ef`.
- Height: about `48px-56px`.
- Page-size control and previous/next buttons use `4px` radius, not pill radius.
- Page-size control uses kube-design Button inner structure. Keep the outer button `32px`
  high with `5px 12px` padding, then align `ButtonInner`, `ButtonLabel`, text spans, and
  `ChevronDown` with inline-flex center and about `4px` gap so text and icon do not look
  glued together.
- Previous/next buttons are `32px` high text buttons with `5px 12px` padding, but the footer
  must keep enough right padding and `overflow: visible` so the last button is not clipped
  or flush against the surface edge.
- No numbered page buttons.
