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
  not a large bordered gray pill.
- Sidebar: `220px`, white, light scoped selector, semantic duotone icons, parent rows
  `36px`, child rows text-only by default.
- Scope selector is a light field with title/subtitle and right chevron, without a leading
  scope icon in expanded state. Expanded parent nav rows stay light and quiet; no strong
  outlined/pill active container.
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
- Toolbar `Select` filters must show a visible kube-design dropdown chevron. Use
  `showArrow` and, when needed, explicit `suffixIcon={<ChevronDown size={16}
  color="#324558" fill="#b6c2cd" />}`; do not let padding or overlay styles hide the
  arrow.
- Table: inset body, white header, `56px` rows, Object Identity Pattern in the first data
  column, borderless `More size={16}` row action with `0 20px` horizontal padding.
- Object identity primary text is bold `700` and `#242e42`; secondary text is regular
  `400` and `#79879c`. Icons have a `40px` alignment slot but no added gray
  tile/background.
- Row action `More` trigger must not be clipped or overlap the table edge.
- Row action cell keeps normal table cell horizontal padding: `0 12px`; with a `56px`
  More button, the action column is at least `80px`.
- Row operation menu text uses normal menu text color, including Delete/删除. Use danger
  color for destructive confirmation buttons, not ordinary table menu items.
- Selected row: pale cells plus thin green outline; no thick side bar, mint fill, or blue
  fill.
- Status text next to the status dot is semibold `600` and primary text color.
- Pagination: attached footer with page size, total, previous icon, `1 / N`, next icon; no
  numbered page buttons. Page-size and previous/next controls use `4px` radius, and the
  right-side controls must never touch or clip against the surface edge. Page-size text,
  value, and chevron must be vertically centered with about `4px` inner gap.
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
  delete modals, animated loading flows, or column-management dialogs unless explicitly
  requested.

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
| Scope selector has leading resource icon or active-nav styling | Light title/subtitle field with right chevron only |
| Generic square/search label input | Real kube-design `FilterInput` when exported |
| Treating `.kubed-select-selection-search-input` as the toolbar search input | Verify the real `FilterInput` input by placeholder or `.filter-input` |
| Kube-design `Select` with native select CSS/data-URI chevron | Let kube-design `Select` render its control |
| Toolbar `Select` has no visible arrow | `showArrow` plus explicit `ChevronDown` suffix when needed |
| Select and search visually press into each other | Stable `148px x 32px` Select plus `12px` gap and `min-width: 0` search |
| Search capped by narrow `max-width` | Search grows through the available toolbar space |
| FilterInput has positive fixed `min-width` such as `220px` | `width: 100%; min-width: 0; max-width: none` inside search zone |
| Search input overlaps Select or action buttons | Three sibling zones: filters, search, actions |
| Refresh/Cogwheel rendered inside the search input | Separate right action buttons after FilterInput |
| Table header with strong tinted fill | White table header with subtle divider |
| Resource row without `40px` icon + text stack | Object Identity Pattern |
| Gray tile/background behind table resource icon | Plain `40px` icon alignment slot |
| Square/clipped row more button | Stable action column with centered text-button `More` trigger |
| Action cell removes horizontal padding | Keep `padding: 0 12px`, at least `80px` action column |
| Delete row menu item in red | Normal menu text color; danger color only in confirmation dialogs |
| Numbered pagination | KubeSphere attached page-size/total/previous/current/next footer |
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
