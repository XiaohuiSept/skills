# KubeSphere Console Visual Contract

Generated UI must look like a KubeSphere Enterprise console page, not a generic admin
template. The product style is light, compact, operational, and data-dense: white console
surfaces on pale blue-gray workspace chrome, quiet navigation, semantic duotone icons,
small typography, status-rich tables, and attached pagination.

## Critical Visual Contract

- Brand: top-left logo must use
  `https://ks-com-cn-staging.pek3b.qingstor.com/images/logo.svg`.
- Header: `64px` high, white, `224px` brand area, Cluster/Workspace icon entries,
  inactive light circular `36px` entry, active dark `36px` rounded-square entry,
  `28px x 4px` active indicator, Component Dock before profile.
- Component Dock: no-border light pill with label `组件坞` or `Component Dock`, icon
  `Grid2Duotone` with explicit default duotone channels `color="#324558"` and
  `fill="#b6c2cd"`; never a gear, bordered chip, or CSS-drawn grid.
- Inactive top Cluster/Workspace icons must explicitly set `color="#36435c"` and
  `fill="#b6c2cd"`; they must not become black silhouettes or single-channel glyphs.
- Profile menu is a compact transparent account trigger, preferably named
  `.ks-profile-trigger`: `32px` avatar + `12px` gap + username/role text stack + chevron,
  not a large bordered gray pill. Username and role text must be left-aligned inside the
  stack; do not inherit centered button text.
- Sidebar: `220px`, white, light scoped selector, semantic duotone icons, parent rows
  `36px`, child rows text-only by default. Sidebar menu labels use `500` font weight in
  both inactive and active states; active emphasis comes from brand color and duotone icon,
  not heavier text.
- Scope selector is a light field with title/subtitle and right chevron, without a leading
  scope icon in expanded state. Expanded parent nav rows stay light and quiet; no strong
  outlined/pill active container. The resource navigation starts `12px` below the scope
  selector.
- Active sidebar icons: set both `color="#00aa72"` and `fill="#90e0c5"`.
- Page header: compact white title band, `18px` bold title, no breadcrumb by default.
- List surface: one integrated surface containing toolbar, table, and pagination.
- Toolbar: left filters, growing FilterInput search, Refresh, Cogwheel/custom columns,
  dark text-only primary command. Refresh/Cogwheel are `32px` high text buttons with
  `0 20px` horizontal padding, not square icon buttons. The dark primary command must use
  kube-design `Button` with `shadow`; Refresh/Cogwheel must remain no-shadow text buttons.
  The toolbar must be three sibling zones: left filters, center search, and right actions.
  The search field grows only inside the center zone and must never overlap the Select
  filter or visually contain Refresh/Cogwheel/Create.
- Toolbar project Select: stable `240px x 32px` control. Apply the width to both the
  Select root and `.kubed-select-selector`; keep a `12px` gap before the search zone.
- Toolbar `Select` filters must show a visible kube-design dropdown chevron. Use
  `showArrow` and, when needed, explicit `suffixIcon={<ChevronDown size={16}
  color="#324558" fill="#b6c2cd" />}`; do not let padding or overlay styles hide the
  arrow.
- Toolbar Cogwheel is the column customization trigger. Clicking the Cogwheel opens and
  closes the compact DataTable settings menu: kube-design `Dropdown placement="bottom-end"
  maxWidth={160}` plus `Menu width={160} className="menu-setting"`. The menu is dark,
  `4px` radius, `padding: 4px 0`, shadowed, with `MenuLabel` title `定制内容` /
  `Custom Columns`; label padding is `8px 12px`. Each row is a `MenuItem` with
  `padding: 6px 12px`, regular `400` text, dark-hover background, and `Eye size={16}` for
  visible columns or `EyeClosed size={16}` for hidden columns. Use the stable structure:
  `.menu-setting` -> `.menu-label` -> row buttons with `.item-inner`, `.item-icon`,
  `.item-body`, and `.item-label`. Do not copy build-generated `sc-*` hash classes. Do
  not use checkboxes, blue selected backgrounds, or a modal for this basic list control.
  It toggles normal data columns only. The first data column (`Name` / `名称`) and the last
  operation column are fixed and must not appear in the customization menu or become hidden.
  The Cogwheel trigger itself must toggle the dropdown: click once to open, click the same
  Cogwheel again to close. Do not use `hideOnClick={false}` or custom popover state that
  breaks trigger close behavior.
- Table: inset body, source-like `border-collapse: collapse`, white header, `56px` rows,
  subtle hover cells, Object Identity Pattern in the first data column, borderless
  `More size={16}` row action with `0 20px` horizontal padding.
- Table checkbox column is fixed at `40px`. Use `colgroup` plus `.ks-select-cell` when
  composing a local table shell.
- Normal single-line body cells use primary text color `#242e42` and regular `400` weight.
  Do not make ordinary cells muted or bold. The exception is an intentional two-line text
  stack: title is bold primary text; description is regular secondary text.
- Object identity primary text is bold `700` and `#242e42`; secondary text is regular
  `400` and `#79879c`. Icons have a `40px` alignment slot but no added gray
  tile/background.
- Row action `More` trigger must not be clipped or overlap the table edge.
- Row action cell is fixed at `80px` and keeps normal table cell horizontal padding:
  `12px` left and right; use `colgroup` plus `.ks-action-cell` when composing a local
  table shell.
- Row action dropdown uses kube-design `Dropdown` with `placement="bottom-end"` so the menu
  panel's right edge aligns with the More button's right edge; keep `maxWidth` around
  `160px`. Also constrain the inner `.ks-row-menu` / menu node to `160px`; otherwise the
  kube-design Menu can overflow the aligned tippy panel and visually drift right.
- Row operation menu text uses normal menu text color, including Delete/删除. Use danger
  color for destructive confirmation buttons, not ordinary table menu items.
- Normal row hover: apply a subtle pale blue-gray cell background, without changing text
  weight, adding side bars, or overriding selected-row outline. Selected row: pale cells
  plus thin green rectangular outline that visually frames the whole row; no thick side
  bar, mint fill, or blue fill. Prefer real cell borders in a collapsed table: selected
  cells get green top borders, the first cell gets a green left border, the last cell gets
  a green right border, and the last selected row closes with a green bottom border. Basic
  demos start with no selected rows, then support selecting and unselecting rows through
  table checkboxes. Checkbox checked state and row selected state must be synchronized; a
  checked row without a selected outline is a failure.
- Status text next to the status dot is semibold `600` and primary text color.
- Pagination: attached footer with page size, total, previous icon, `1 / N`, next icon; no
  numbered page buttons. Chinese page-size label is `每页显示`; English label is
  `Rows per page`. Default value is `10`, with options `10`, `20`, `50`, `100`. Chinese
  total uses `总数：9`; English total uses `Total: 9`. Page-size control uses `4px` radius
  and `8px` horizontal padding. Previous/next controls are exactly `32px x 32px` with
  `4px` radius, and the right-side controls must never touch or clip against the surface
  edge. Page-size dropdown visual panel outer width is fixed at `96px` with `0` popover
  content padding; the menu itself uses `8px` side padding, options are `80px`
  wide, and options have no extra side margin. Page-size text, value, and chevron must be
  vertically centered with about `4px` inner gap.
- Language/type: one locale per page, no bilingual duplicates, kube-design font stack,
  minimum visible font size `12px`.
- Typography must not use AI-template font stacks such as `Inter`, `Geist`, `system-ui` as
  the first family. Use the kube-design/KubeSphere stack in `assets/tokens.md`.
- Public primitives: controls and mapped icons come from `@kubed/components` and
  `@kubed/icons`; do not hand-write local primitive, component, or icon replacements.
- Visible controls: toolbar actions, command buttons, table checkboxes, row menus, and
  pagination icon actions must use kube-design components, not raw HTML controls styled to
  look similar.
- Basic list scope: do not add dashboard/stat cards, language switchers, toast systems,
  delete modals, animated loading flows, or column-management dialogs. Use only the compact
  Cogwheel dropdown for list column visibility unless the user explicitly asks for a larger
  settings flow.

## Source Of Truth

| Need | Read |
|---|---|
| Exact tokens and CSS variables | `assets/tokens.md` |
| Terms, resource keys, and icons | `assets/icon-map.md` |
| Resource/button/status/table decisions | `assets/recipes.md` |
| Copyable list page structure | `assets/page-shells/list.md` |
| Header/sidebar/page frame rules | `references/console-frame.md` |
| List page details | `references/list-pages.md` |
| Component behavior | `references/components.md` |
| Locale and terminology policy | `references/language.md` |
| Known failure examples | `references/case-studies/*` |

## High-Frequency Anti-Patterns

| Wrong | Correct |
|---|---|
| Fake logo or text-only brand | Official KubeSphere logo URL |
| Dark sidebar | White `220px` sidebar |
| Lucide/custom icons | Semantic `@kubed/icons` duotone icons |
| Local `icons.tsx` with hand-written SVG approximations | Real `@kubed/icons` imports |
| Local cloned `Button` / `Checkbox` / `Dropdown` primitives | Real `@kubed/components` imports |
| Installed `@kubed/components` but raw `button/select/checkbox/dropdown` controls | Actual kube-design control components |
| Runtime locale switcher in a basic list | Single locale chosen from the prompt |
| Green create button | Dark secondary filled button, text-only by default |
| Dark create button without `shadow` | `Button variant="filled" color="secondary" radius="xl" size="sm" shadow` |
| Boxed or square refresh/settings buttons | Borderless text buttons, `32px` high with `0 20px` padding |
| Text icon buttons with command-button shadow | Refresh/Cogwheel/Row More stay no-shadow `variant="text"` buttons |
| Inactive Workspace/Cluster icon as black silhouette | Explicit top duotone channels: `#36435c` + `#b6c2cd` |
| Component Dock restyled as custom bordered chip | No-border light pill with explicit-color `Grid2Duotone` |
| User profile wrapped in large gray bordered pill | Compact transparent `.ks-profile-trigger`: avatar + text + chevron |
| Profile username and role are centered or staggered | Left-align `.ks-profile-text`, `strong`, and subtitle |
| Scope selector has leading resource icon or active-nav styling | Light title/subtitle field with right chevron only |
| Sidebar labels become `600` or bolder | Keep sidebar menu labels `500`; use color/icon for active state |
| Generic square/search label input | Real kube-design `FilterInput` when exported |
| Treating `.kubed-select-selection-search-input` as the toolbar search input | Verify the real `FilterInput` input by placeholder or `.filter-input` |
| Kube-design `Select` with native select CSS/data-URI chevron | Let kube-design `Select` render its control |
| Toolbar `Select` has no visible arrow | `showArrow` plus explicit `ChevronDown` suffix when needed |
| Select and search visually press into each other | Stable `240px x 32px` Select plus `12px` gap and `min-width: 0` search |
| Search capped by narrow `max-width` | Search grows through the available toolbar space |
| FilterInput has positive fixed `min-width` such as `220px` | `width: 100%; min-width: 0; max-width: none` inside search zone |
| Search input overlaps Select or action buttons | Three sibling zones: filters, search, actions |
| Refresh/Cogwheel rendered inside the search input | Separate right action buttons after FilterInput |
| Cogwheel is a decorative settings icon only | Cogwheel opens the `160px` DataTable settings menu for non-fixed data columns |
| Cogwheel opens but cannot close on second trigger click | Use default kube-design Dropdown trigger behavior; do not set `hideOnClick={false}` |
| Checkbox/modal/blue column settings UI | Dark `.menu-setting` dropdown with `MenuLabel` and `Eye` / `EyeClosed` rows |
| Name or operation columns can be hidden | Keep first data column and last operation column fixed |
| Table header with strong tinted fill | White table header with subtle divider |
| Table rows have no hover state or heavy hover effects | Subtle pale blue-gray hover on normal row cells only |
| Demo opens with preselected table rows | Default no selected rows; support checkbox select/unselect |
| Checked checkbox but row has no selected outline | Synchronize checked state with `row-selected` and use `:has(input:checked)` fallback |
| Checkbox column wider/narrower than product table | Fixed `40px` selection column |
| Ordinary single-line cells are muted or bold | Primary text color `#242e42`, regular `400` |
| Resource row without `40px` icon + text stack | Object Identity Pattern |
| Gray tile/background behind table resource icon | Plain `40px` icon alignment slot |
| Square/clipped row more button | Stable action column with centered text-button `More` trigger |
| Action cell removes horizontal padding or drifts in width | Fixed `80px` action column with `12px` left/right padding |
| Row action menu opens left-aligned, centered, or inner menu overflows right | `Dropdown placement="bottom-end" maxWidth={160}` plus `160px` inner `.ks-row-menu` |
| Delete row menu item in red | Normal menu text color; danger color only in confirmation dialogs |
| Numbered pagination | KubeSphere attached page-size/total/previous/current/next footer |
| Chinese pagination says `每页行数` / `共 9 条记录` | `每页显示` and `总数：9` |
| Page-size control uses `20px` horizontal padding | `8px` horizontal padding |
| Page-size dropdown has overly wide or double side spacing | Fixed `96px` outer width, `0` popover padding, `8px` menu padding, `80px` options, no extra option margin |
| Pagination previous/next are text-width pills | Exact `32px x 32px`, radius `4px` |
| Pagination controls clipped against the right edge or rounded as pills | `4px` radius controls with footer padding and no clipping |
| Page-size text and chevron glued together or vertically off | Split label/value/icon children and center Button inner nodes with `4px` gap |
| `容器组 (Pods)` or `Deployments / 部署` | Single-locale labels only |
| `Inter` / `Geist` / generic `system-ui` first font family | Kube-design/KubeSphere font stack from tokens |
| Stats cards above a basic list | Compact page header plus integrated list surface |

## Visual Priority

The console frame must be recognizable before resource content is judged. Header, sidebar,
page header, toolbar, table, and pagination are one coherent system. When in doubt, preserve
the KubeSphere frame and choose the conservative compact operational layout.

Verify in this order: official logo, Header quick-nav states, Component Dock, profile
trigger, Sidebar scope selector, Sidebar menu hierarchy, Page Header, Toolbar, Table,
Pagination, language consistency. Do not accept a page with a polished table but incorrect
Header or Sidebar.
