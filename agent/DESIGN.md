# KubeSphere Console Design Guide

This document defines the visual target for frontend pages generated with `kube-design`.
It is for AI agents and engineers who need to build KubeSphere-style console pages from
`@kubed/components`, `@kubed/icons`, and local composition code.

Use this file together with `SKILL.md`:

- `SKILL.md` explains the workflow: what to inspect, how to generate, and how to verify.
- `DESIGN.md` explains the design target: what the UI should look and feel like.
- The consuming project's installed `@kubed/*` packages are the source of truth for
  component APIs and available icons. Public `kube-design` upstream references may be
  consulted when internet access is available, but a local `kube-design` checkout is not
  required.
- Private console repositories, if available to maintainers, are only reference material for
  extracting these rules. They are not required dependencies for generated pages.

## Design Goal

Generated pages should look like KubeSphere Enterprise console pages, not generic admin
templates. This document is the visual source of truth for downstream agents: it distills
the live console appearance and product implementation patterns into portable rules that
can be used with public `kube-design`.

The product UI is light, compact, operational, and data-dense. It should feel calm and
precise: white surfaces on pale blue-gray workspace chrome, small typography, compact
controls, status-rich tables, and quiet navigation.

Downstream agents should only need this document, the companion `SKILL.md`, and the
consuming project's installed `@kubed/*` packages to produce a good first version. Public
`kube-design` upstream references can help resolve API/icon uncertainty, but the generated
page must not depend on a local checkout. Use this document for colors, typography, shell
structure, list density, navigation active states, and common page composition.

Current design scope is Phase 1: **basic resource list pages**. Optimize for accurate
top navigation, management-view entries, left navigation, scoped selector, page title band,
list surface, toolbar, table, and pagination before expanding to detail pages or creation
flows.

The most important rule: the whole console frame must be recognizable before the resource
content is judged. Top navigation, left navigation, page header, content surface, toolbar,
table, and pagination are one coherent system.

## Shell Reality

`@kubed/components` does not currently provide a complete product shell component that
exactly matches the live KubeSphere top navigation and left resource navigation.

The real console implementation may contain private or business-specific components for
shell layout, navigation, data fetching, permissions, action menus, namespace selection,
extensions, and resource stores. Those components explain the product UI, but they should
not be treated as portable dependencies for agents that only receive this skill folder and
`kube-design`.

Agents should therefore:

- Use `@kubed/components` for primitives: buttons, inputs, selects, tables, cards, menus,
  tags, status components, dropdowns, and compact form controls.
- Use `@kubed/icons` for product, resource, and action icons.
- Build the product shell as local composition code when no real shell component exists.
- Reuse an existing `ConsoleShell`, `KubeSphereShell`, `AppLayout`, or product shell from the
  consuming project if one exists.
- Never replace the shell with a generic dark sidebar admin layout.

### Portability Contract

Generated UI must be portable across projects that have `kube-design` but do not have the
private console codebase. Design fidelity should come from the visual contract in this
document, not from importing private components.

When a real console component is unavailable, recreate its visual role with kube-design
primitives and local CSS:

| Console Role | Portable Construction |
|---|---|
| Global top navigation | Local flex header, logo area, Cluster/Workspace entries, Component Dock, profile menu |
| Left resource navigation | Local white sidebar, scoped selector, nested nav rows |
| Scoped cluster/workspace switcher | Bordered `Field`-like block with title, subtitle, chevron |
| Page title band | White flex header, `18px` bold title, no breadcrumb |
| List surface | One white surface containing toolbar, table, and pagination |
| Resource identity cell | `Field`-like two-line cell with resource icon and muted description |
| Status | `StatusDot` plus text, compact tag only when needed |
| Row actions | `Dropdown` + `Menu` + `MenuItem`, compact icon trigger |
| Primary create action | Compact dark pill `Button` in the list toolbar |

Do not let missing private components reduce the UI to a generic table demo. The generated
page still has to look like the live KubeSphere console.

### Golden Generic Page Blueprint

When an agent is asked to generate a generic KubeSphere console page, this is the default
visual blueprint. Resource type may change labels, icons, columns, and data, but not the
frame.

```text
ConsoleFrame
  ConsoleHeader       64px high, white
  ConsoleBody         height: calc(100vh - 64px)
    ResourceSidebar   220px wide, white
      ScopedSwitcher  196 x 70
      ResourceNav     36px rows
    MainArea          flex: 1, #eff4f9
      PageHeader      56px high, white, title only
      ContentArea     20px padding
        ListSurface   white, 4px radius
          Toolbar     52-56px
          Table       56px rows
          Footer      48-56px
```

Minimum reusable parts for generated code:

- `ConsoleHeader`: logo area, Cluster/Workspace entries, Component Dock, right user dropdown.
- `ResourceSidebar`: scoped switcher plus nested resource nav.
- `PageHeader`: compact title band, no breadcrumb.
- `ListSurface`: one integrated wrapper for toolbar, table, and pagination.
- `ResourceIdentityCell`: icon, bold resource name, muted secondary line.
- `StatusCell`: status dot plus text.
- `RowActions`: overflow dropdown.

If no project shell exists, generate these parts locally. They should be small composition
components, not a new design system.

### CSS Contract For Generated Pages

Generated pages should define or preserve these values. Treat this block as the strict
numeric and color authority for generic generated pages; do not scatter competing pixel
values or alternate palettes elsewhere in the implementation.

```css
:root {
  --ks-logo-url: url("https://ks-com-cn-staging.pek3b.qingstor.com/images/logo.svg");
  --ks-header-height: 64px;
  --ks-sidebar-width: 220px;
  --ks-sidebar-collapsed-width: 60px;
  --ks-page-header-height: 56px;
  --ks-content-padding: 20px;
  --ks-control-height: 32px;
  --ks-nav-row-height: 36px;
  --ks-table-row-height: 56px;
  --ks-min-font-size: 12px;
  --ks-bg: #eff4f9;
  --ks-surface: #ffffff;
  --ks-subtle: #f9fbfd;
  --ks-hover: #eff4f9;
  --ks-border: #e3e9ef;
  --ks-border-strong: #ccd3db;
  --ks-text: #242e42;
  --ks-muted: #79879c;
  --ks-nav-text: #4a5974;
  --ks-active: #55bc8a;              /* active nav text, success/running dot */
  --ks-icon-active: #00aa72;         /* selected sidebar icon primary/color */
  --ks-icon-active-fill: #90e0c5;    /* selected sidebar icon secondary/fill */
  --ks-command-hover: #181d28;
  --ks-warning: #f5a623;
  --ks-error: #ca2621;
}
```

Use these as layout and color anchors even if the consuming project uses different class
names. Do not introduce a different palette for generic pages. If the consuming project
already defines equivalent theme tokens, map these semantics to those tokens instead of
duplicating both systems.

## Visual Foundations

### Color

| Role | Value | Use |
|---|---:|---|
| Workspace background | `#eff4f9` | Main app chrome behind cards and tables |
| Header/sidebar/card background | `#ffffff` | Global header, sidebar, cards, table surfaces |
| Subtle background | `#f9fbfd` | Header icon backgrounds, empty areas, light controls |
| Hover/subtle selected background | `#eff4f9` | Quiet row/nav hover or secondary-reference child active background |
| Divider/border | `#e3e9ef` / `#ccd3db` | Section borders, table lines, input borders |
| Strong text | `#242e42` | Titles, primary labels, table names |
| Muted text | `#79879c` | Descriptions, secondary labels, timestamps |
| Nav text | `#4a5974` | Inactive first-level sidebar labels |
| Active green | `#55bc8a` | Active nav text/icon, running status, success state |
| Dark command | `#242e42` | Primary create/confirm button in resource pages |
| Dark command hover | `#181d28` | Hover for dark command buttons |
| Warning | `#f5a623` | Updating, warning, pending |
| Error | `#ca2621` | Failed, unavailable, destructive status |

Keep the UI light by default. Dark surfaces are reserved for selected top management
entries, dark primary commands, and tooltips.

### Icon Variants

KubeSphere icons are usually duotone. The icon variant must match the background.

| Background | Icon variant | Primary/color | Secondary/fill |
|---|---|---:|---:|
| Light background | `dark` | `#242e42` | `#b6c2cd` |
| Dark background | `light` | `rgba(255,255,255,0.9)` | `#ffffff66` |
| Selected sidebar on light background | custom active | `#00aa72` | `#90e0c5` |

Use the light variant whenever an icon sits on a dark filled button or dark selected block.
Do not reuse the default dark duotone colors on a dark background.

### Object Identity Pattern

KubeSphere often represents a resource, application, cluster, workspace, user-facing object,
or selectable record with a compact identity block. This pattern appears in table name
columns, resource list cards, selector rows, detail headers, modal records, and quota/list
items. It is one of the strongest small-scale visual signatures of the console.

Structure:

- Left icon/image: `40px x 40px`.
- Gap between icon and text stack: `12px`.
- Text stack: two lines, vertically centered with the icon.
- Title line: `12px`, bold/`600-700`, `--ks-text` / text-primary (`#242e42`).
- Description line: `12px`, regular, `--ks-muted` / text-secondary (`#79879c`).
- Line height: about `20px` (`1.67`).
- Both title and description should ellipsize when space is limited.
- Use semantic `@kubed/icons` or real object images/assets. Do not use decorative
  placeholder icons.

Optional status:

- When an object needs a health/running marker, a small `8px` status indicator may be
  anchored to the lower-right of the `40px` icon.

Do not replace this pattern with a plain text-only name cell, oversized avatar, badge-only
cell, generic `32px` icon row, or loosely spaced media object.

### Typography

Use the KubeSphere/kube-design compact type system.

```css
font-family:
  PingFang SC,
  Lantinghei SC,
  Helvetica Neue,
  Helvetica,
  Arial,
  Microsoft YaHei,
  STHeitiSC-Light,
  simsun,
  WenQuanYi Zen Hei,
  WenQuanYi Micro Hei,
  sans-serif;
```

| Role | Size | Weight | Notes |
|---|---:|---:|---|
| Page title | `18px` | `700` | Live list page title |
| Card/section title | `14px-16px` | `600` | Panels, table sections, detail blocks |
| Sidebar scoped title | `14px` | `700` | Cluster/project selector title |
| Default UI text | `12px` | `400-600` | Tables, controls, nav, helper text |
| Muted text | `12px` | `400` | Secondary lines and descriptions |

The minimum visible UI font size is `12px`. Do not use `10px` or `11px` for badges,
pagination, table headers, helper text, nav labels, button labels, tags, or secondary
metadata. If a component default renders smaller than `12px`, override it to `12px`.

Do not scale font size with viewport width. Avoid hero typography on console pages.

### Language Consistency

Use one UI language per generated page. If the consuming product context is Chinese, render
navigation labels, selector subtitles, buttons, table headers, and pagination in Chinese. If
the context is English, keep them in English. If the locale is not explicitly known, follow
the main language of the user's prompt: English prompt -> English UI, Chinese prompt ->
Chinese UI.

Do not mix English parent menu labels with Chinese child labels or vice versa. Do not render
bilingual duplicate labels such as `Deployments / 部署`, `Cluster 集群`, or `Create 创建`.

For cloud-native resource names, use canonical KubeSphere/Kubernetes terminology:

| Concept | English UI | Chinese UI |
|---|---|---|
| Cluster | `Cluster` / `Clusters` | `集群` |
| Workspace | `Workspace` / `Workspaces` | `企业空间` |
| Project | `Project` / `Projects` | `项目` |
| Namespace | `Namespace` / `Namespaces` | `命名空间` when the Kubernetes namespace itself is meant; `项目` in KubeSphere project context |
| Workload | `Workload` / `Workloads` | `工作负载` |
| Deployment | `Deployment` / `Deployments` | `部署` |
| StatefulSet | `StatefulSet` / `StatefulSets` | `有状态副本集` |
| DaemonSet | `DaemonSet` / `DaemonSets` | `守护进程集` |
| Job | `Job` / `Jobs` | `任务` |
| CronJob | `CronJob` / `CronJobs` | `定时任务` |
| Pod | `Pod` / `Pods` | `容器组` |
| Service | `Service` / `Services` | `服务` |
| Ingress | `Ingress` / `Ingresses` | `路由` or `Ingress` if the product context keeps the Kubernetes kind |
| Gateway | `Gateway` / `Gateways` | `网关` |
| ConfigMap | `ConfigMap` / `ConfigMaps` | `配置字典` |
| Secret | `Secret` / `Secrets` | `保密字典` |
| ServiceAccount | `ServiceAccount` / `ServiceAccounts` | `服务账户` |
| Image | `Image` | `镜像` |
| Created Time | `Created Time` | `创建时间` |
| Updated Time | `Updated Time` | `更新时间` |
| Custom Columns | `Custom Columns` | `自定义列` |

If a technical term is uncertain, preserve the standard Kubernetes English kind instead of
inventing a translation.

### Spacing And Shape

| Token | Value | Use |
|---|---:|---|
| Page/content padding | `20px` | Main workspace padding |
| Compact gap | `8px-12px` | Icon-label, small control groups |
| Standard gap | `16px` | Form groups, toolbar groups |
| Large gap | `20px-24px` | Major content rhythm |
| Control height | `32px` | Inputs, selects, toolbar buttons |
| Top nav icon target | `36px` | Header product icons |
| Sidebar row height | `36px` | First-level and child nav rows |
| Table toolbar height | `52px-56px` | Search/filter/action band |
| Table row height | `56px-58px` | Resource rows |
| Radius | `3px-4px` | Inputs, cards, nav selector |
| Pill radius | `32px` | Dark create/confirm buttons |

Avoid large rounded cards, glass effects, gradients, decorative blobs, and oversized
spacing. Cards should feel utilitarian, not editorial.

## Console Frame

### Brand Logo Contract

The KubeSphere logo is mandatory in the global top navigation.

- Canonical logo URL:
  `https://ks-com-cn-staging.pek3b.qingstor.com/images/logo.svg`.
- Generated pages must render this as an image, for example
  `<img src="https://ks-com-cn-staging.pek3b.qingstor.com/images/logo.svg" alt="KubeSphere" />`.
- Do not generate a custom logo, draw an SVG mark, use an emoji/icon as the logo, or render
  a text-only logo.
- Do not use fake branding such as `LogoText Kube Sphere`, split words such as `Kube Sphere`,
  placeholder image icons, or stock-looking marks.
- A consuming project asset may replace the URL only when it is already the official
  KubeSphere brand logo or the user explicitly asks to use that asset.
- If the logo URL cannot load in an offline environment, keep the same image slot and
  mention the load limitation in the final response; do not silently replace the logo with
  invented branding.

### Baseline Measurements

Use the live console at `1440 x 900` as the baseline frame for extracting layout
proportions. Production layouts remain responsive, but should preserve density and
hierarchy.

```text
viewport baseline        1440 x 900
global header              64px
header logo area          224px
live sidebar              220px
live main area           1220px
live page header           56px
content padding            20px
```

At `1920px` and wider, do not simply stretch content until the table becomes sparse.
Main content can expand, but row density, toolbar height, typography, and navigation scale
should remain the same.

### Global Top Navigation

The top navigation is mandatory on full console pages.

- Height: `64px`.
- Background: white.
- Logo area: about `224px` wide on desktop.
- Logo source: use the canonical logo URL from the Brand Logo Contract. The private console
  resolves the logo from theme/config, but generated portable pages should use the canonical
  URL unless the consuming app already exposes the official KubeSphere logo.
- Logo layout: wrapper width about `224px`; linked logo box about `180px x 36px`; image uses
  `max-width: 100%`.
- Text-only logo fallback is not acceptable for normal generated pages.
- Product/module icon rail starts after the logo area.
- Product icon target: `36px x 36px`, vertically centered at `y=14`.
- Inactive product icons use pale `#f9fbfd` backgrounds.
- Active product icon can use a dark navy square (`#242e42`) with compact shadow.
- Use quiet vertical dividers between icon groups when needed.
- Right side contains component dock button, avatar, username, role/subtitle, and chevron.
- User block is compact: `32px-36px` high, `12px` text, bold username, muted role.

Do not turn top navigation into a marketing header, breadcrumb-only bar, or dark app bar.

Frame gate: a generated full console page is not visually acceptable if the top navigation
is missing the official logo image, Cluster/Workspace management entries, Component Dock,
or profile menu.

#### Management View Entries

KubeSphere's console has two primary management views in the persistent top navigation:

- **Cluster management**: route family `/clusters`, label `Cluster` / `集群`, icon `Cluster`.
- **Workspace management**: route family `/workspaces`, label `Workspaces` / `企业空间`,
  icon `Workspace`.

These are not decorative icons and should not be replaced by arbitrary product shortcuts.
They are the global view switcher for the console. A generated full console page should
show both entries when the current user/context has access to both.

Visual target for each top entry:

- Outer target: `36px` wide and full header height.
- Inner icon button: `36px x 36px`, vertically centered.
- Icon size: about `24px`.
- Inactive background: `#f9fbfd`, circular.
- Hover: `#eff4f9`, radius becomes about `8px`.
- Active background: `#242e42`, radius about `8px`, shadow
  `0 4px 8px rgba(36, 46, 66, 0.2)`.
- Active icon uses the light icon variant: primary `rgba(255,255,255,0.9)`, secondary
  `#ffffff66`. Do not render the active top icon with the default dark duotone colors.
- Active indicator: bottom bar `28px x 4px`, dark `#242e42`, radius `3px 3px 0 0`.
- Spacing: about `20px` margin between entries; a pale vertical divider may separate these
  primary entries from extension/custom entries.

Active state follows the route:

- Any `/clusters...` page highlights `Cluster`.
- Any `/workspaces...` page highlights `Workspace`.
- Dashboard or neutral home pages may show no selected management view.

When generating a basic list page, choose the active top entry from the requested scope. A
Deployment list inside cluster management should highlight `Cluster`; a Deployment list
inside enterprise-space/project management should highlight `Workspace`.

#### Right Header Actions

The right side of the header is also part of the live console identity. It is not only a
user profile dropdown.

Order:

```text
right divider
optional AI assistant
Component Dock
profile menu
```

Component Dock (`组件坞`) is a persistent action before the user profile menu.

- Runtime component: `ExtensionsEntry`.
- Text key: `ComponentDock`.
- Icon asset: `/assets/grid.svg`, backed by `packages/bootstrap/assets/grid.svg`.
- If `/assets/grid.svg` is unavailable in the consuming project, use a semantic
  `@kubed/icons` app/grid/dashboard-style fallback. Keep the same button size, placement,
  label, and hover behavior. Do not replace Component Dock with a gear/settings action.
- Button display: inline-flex, centered, `36px` high.
- Minimum width: about `84px`; horizontal padding `0 16px`.
- Radius: `18px` by default.
- Background: `#f9fbfd`.
- Icon: `20px x 20px`.
- Label: `12px`, `600`, with about `6px` gap after the icon.
- Hover: background `#eff4f9`, radius about `8px`.
- It opens a full-page component dock/custom-entry panel, not a small dropdown.

Do not omit this button on full console pages, do not merge it into the user dropdown, and
do not replace it with a plain gear/settings icon.

### Left Resource Navigation

The left navigation is one of the strongest KubeSphere identity signals.

- Width: `220px` in the source-derived console rules. Generated pages should default to
  `220px` unless the consuming shell already uses a different product sidebar width.
- Position: starts below the `64px` global header.
- Background: white.
- Right edge: subtle border/separation only; no heavy shadow.
- It is a resource tree, not a generic admin sidebar.
- It must remain light. Dark sidebars are not acceptable unless the user explicitly asks
  for a dark theme.

#### Scoped Selector

Resource pages usually begin with a scoped context selector for cluster/project/workspace.

- Live baseline: `196px x 68px-70px`, inside the `220px` sidebar.
- Position: `12px` horizontal margin, about `16px` from top of sidebar, with `12px`
  margin below it.
- Fill: white to very subtle `#f9fbfd` gradient. The live reference uses a quiet
  `linear-gradient(359.02deg, #f9fbfd 0.81%, #ffffff 99.13%)`.
- Border: subtle `#e3e9ef` / `#ccd3db`.
- Radius: `4px`.
- Internal padding: `12px`.
- Content: bold scope name at `14px`, muted scope type at `12px`.
- Scope name must stay on one line with ellipsis.
- Right side: small chevron.
- Do not add a large left scope icon in expanded state unless the consuming product shell
  already has one. The switcher should read like a compact `Field`, not a card with a
  decorative avatar.

The selector is linked to the active top management view:

| Active top view | Selector title | Selector subtitle | Selector action |
|---|---|---|---|
| Cluster management list | `All clusters` / `全部集群` | `Cluster` / `集群` | Opens cluster selector |
| Cluster detail/resources | Current cluster display name | `Cluster` / `集群` | Opens cluster selector |
| Workspace management list | `All workspaces` / `全部企业空间` | `Workspace` / `企业空间` | Opens workspace selector |
| Workspace detail/resources | Current workspace display name | `Workspace` / `企业空间` | Opens workspace selector |

Do not show a cluster selector when the active top view is enterprise-space management, and
do not show a workspace selector when the active top view is cluster management. In
workspace project resource pages, the sidebar switcher still represents the workspace
context; project/cluster filters belong in the page or toolbar when needed.

#### Navigation Rows

- Menu begins about `12px` below the scoped selector.
- First-level row: `196px x 36px`, `x=12`.
- First-level content: icon, label, optional chevron.
- Label start: about `x=48`.
- Row text: `12px`, weight `500`.
- First-level labels must remain a single line: use `white-space: nowrap`,
  `overflow: hidden`, `text-overflow: ellipsis`, and `min-width: 0`.
- Do not let long English labels such as `Application Workloads` wrap to two lines or
  increase row height. Ellipsis or a shorter product label is preferable to a wrapped row.
- Inactive first-level color: `#4a5974`.
- Row rhythm: `36px` height with small vertical gaps. Do not add large section blocks.
- Expanded child row: text-only, `x=48`, width about `160px`, height `36px`.
- Inactive child color: `#242e42`, weight `400`.
- Child rows default to no icons. Only add child-row icons when the user explicitly requests
  them or when the consuming product shell already renders that exact submenu with icons.

#### Active State

Live KubeSphere uses a quiet active state:

- Active parent and active child use green text/icon `#55bc8a`.
- Weight is medium, not oversized.
- No dark selected rows.
- No thick left border.
- No high-saturation background.
- No gradient or large pill.

Some secondary references show a subtle light blue-gray selected child background. If used,
it must stay quiet and inside the sidebar gutter. It should never visually dominate the
content. The live-console green text/icon active state is the default.

#### Sidebar Icons

- Use `@kubed/icons`.
- Choose icons by menu English name, Kubernetes kind, and aliases. Use the semantic
  `@kubed/icons` component available in the consuming project.
- Examples: `Pod`, `Cluster`, `Gateway`, `Job`, `CronJob`, `StatefulSet`.
- SVG names map to PascalCase component names: `cron-job.svg` -> `CronJob`,
  `stateful-set.svg` -> `StatefulSet`.
- For a normal deployment menu, use `Deployment` if available; do not use
  `BlueGreenDeployment` unless the menu is specifically for blue-green deployment.
- Do not use emoji, lucide icons, hand-written SVG, or random abstract placeholders for
  resource menus.

## Page Layout

### Page Header

Resource list pages use a compact white page title band.

- Live baseline: starts at `x=220/221`, `y=65`, width fills the main area, height about
  `56px`.
- Some secondary references may use `64px`, but the live list-page baseline is `56px`.
- Page title: `18px`, `700`, `#242e42`.
- Do not show breadcrumb navigation unless the user explicitly requests it.
- Keep it quiet: no hero section, no marketing description, no oversized action area.
- Local tabs, when present, live in or directly under this band.

### Content Area

- Starts below the page header.
- Standard padding: `20px`.
- Live 1440 list baseline: content starts around `x=241`, `y=141`, width about `1179px`.
- Use pale blue-gray workspace background behind the content.
- Prefer one dominant white surface for the primary workflow.

## Resource List Page

List pages are the most common KubeSphere page type. They must be dense, scannable, and
operation-oriented.

### Structure

```text
global-header
sidebar
main
  page-header
    title
  content
    list-surface
      toolbar
      table-header
      table-rows
      pagination
```

### List Surface

- One white surface contains toolbar, table, and pagination.
- Do not split toolbar, table, and pagination into separate floating cards.
- Surface radius is small (`3px-4px`) with subtle shadow or border.
- The surface should align to the `20px` content grid.
- The list surface starts immediately inside the content padding. Do not insert a marketing
  banner, breadcrumb, hero, or explanatory panel above it for a basic list page.
- The toolbar, table, and footer have distinct but connected backgrounds: toolbar uses
  `#f9fbfd`, table header/body use `#ffffff`, footer uses `#f9fbfd`.

### Toolbar

- Height: `52px-56px`.
- Internal padding: `10px 20px`.
- Background: `#f9fbfd`.
- Bottom separator: `1px` shadow/border in `#eff4f9`.
- Layout: left optional filters/selectors, center search channel, right fixed action group.
- The center search channel must grow to fill available width.
- Left side: namespace/project selector when needed.
- Search input is compact, pale, and `32px` high.
- Right side order is fixed: refresh icon button, column settings icon button, then the
  primary create/action button.
- Refresh icon: use `Refresh` from `@kubed/icons`.
- Column settings icon: use `Cogwheel` from `@kubed/icons`; it opens custom column
  visibility controls. This is table settings, not Component Dock.
- Icon buttons use text/ghost button styling, keep intrinsic `32px` control rhythm, and
  have `12px` left spacing between the right-side actions.
- Create button is dark, pill-like, `32px` high, at far right.
- Avoid large multi-filter forms above the table unless the task requires advanced filtering.
- Search and select controls should remain `32px` high. Do not use `40px+` dashboard-sized
  inputs in a basic list toolbar.

#### Filter Input

The live table search uses kube-design `FilterInput`, not a plain rectangular input.

- Height: `32px`.
- Width: `100%` of the toolbar center channel.
- Toolbar layout must let the search channel grow: the center `.toolbar-item` or equivalent
  wrapper should use `flex: 1 1 auto` / `flex-grow: 1`, while the right action group keeps
  its intrinsic width.
- Do not cap the search to a small fixed width in basic list pages. It should stretch from
  the left controls to the right action icons/create button.
- Horizontal padding: `5px 40px`.
- Border radius: `18px`.
- Default background: `#eff4f9`.
- Default border: transparent.
- Hover border: `#e3e9ef`.
- Focused or has value: white background and `#79879c` border.
- Search icon is positioned `left: 12px`; clear icon is `right: 12px`.
- Inner input height: `20px`, no border, transparent background, `12px`, weight `600`.

Do not use a square white input with a strong border for the resource list search box.

### Table

- The table main wrapper is inset from the card: `margin: 0 12px 12px`.
- Toolbar and footer remain attached to the same list surface; the table header/body sit
  inside this `12px` left/right/bottom inset.
- Header row: quiet, compact, muted labels.
- Row height: about `56px-58px`.
- A full resource list should show about 10 rows at `1440 x 900` when data exists.
- Columns should prioritize:
  - selection checkbox
  - identity/name
  - status
  - namespace/project/scope
  - 1-3 resource-specific attributes
  - created/updated time
  - row actions
- Identity/name cells must follow the Object Identity Pattern: `40px` semantic icon,
  `12px` gap, bold primary title, muted regular description, and ellipsis on both lines.
- Status uses `StatusDot` or equivalent dot plus text.
- Tags are compact and neutral. Avoid colorful tag soup.
- Row hover can use a very pale background. Avoid colored row bands.
- Row actions sit at the far right, usually as vertical overflow.

Table CSS target:

| Element | Target |
|---|---|
| Table main wrapper | `margin: 0 12px 12px` inside the list surface |
| Table header background | `#ffffff`; do not use gray/blue filled header bands |
| Header cell | `12px`, `600`, `#79879c`, padding `16px 12px`, mono/sans per kube-design table |
| Body cell | `12px`, `400`, `#242e42`, padding `8px 12px` |
| Row height | `56px` |
| Row divider | `1px solid #eff4f9` |
| Row hover | `#eff4f9` |
| Selected row | pale background with `#55bc8a` border |

The first column should be visually rich and recognizable as an object identity block. A
plain text-only first column makes the page look unlike the live console.

### Pagination

- Attached to the list surface.
- Height: about `48px-56px`.
- Background: `#f9fbfd`.
- Padding: `10px 20px`.
- Left side: page-size dropdown, `12px` vertical divider, total count.
- Page-size text: `12px`, `600`, `#36435c`, with `CaretDown`.
- Total count text: `12px`, muted `#79879c`.
- Right side: previous button, page index dropdown such as `1 / 5`, next button.
- Previous/next are text buttons with `Previous` and `Next` icons at `20px`, not plain text.
- Page index text: `12px`, `600`, `#36435c`, margin `0 12px`.
- Keep the footer as real pagination controls. It should include page-size selection,
  total count, and previous/next navigation even when mock data has only one page; disabled
  buttons are acceptable.
- Keep it small and quiet.
- Footer background may use `#f9fbfd`; it should still feel attached to the same list
  surface.

Do not render pagination as only `10 / page 1-10 of 10`, `1-10 of 10`, or any plain text
summary. That pattern does not match the live console table footer.

### Empty List

Empty state lives inside the same content surface.

- Height: about `220px-260px`.
- One muted icon.
- One bold title.
- One short explanation.
- One dark primary action.
- No marketing illustration, no multi-card empty layout.

## Component Usage

Use kube-design primitives wherever possible:

| Need | Prefer |
|---|---|
| Buttons | `Button` |
| Search | `FilterInput` if exported; otherwise a local FilterInput-style wrapper |
| Plain inputs | `Input` |
| Select/filter | `Select` |
| Tables | `Table` / local table pattern backed by kube-design styles |
| Cards/surfaces | `Card` or small local surface wrapper |
| Status | `StatusDot`, compact tags/badges |
| Menus | `Dropdown` + `Menu` + `MenuItem` |
| Resource/object identity | `Field`, `Entity`, or local Object Identity Pattern composition |
| Icons | `@kubed/icons` only |

If a component API is uncertain, rely on the consuming project's existing imports,
TypeScript/build feedback, installed package documentation, or public `kube-design`
upstream references when internet access is available.

Use the consuming project's existing CSS approach first: styled/emotion patterns, CSS
modules, project utility classes, or colocated stylesheets. Avoid large inline `<style>`
blocks in React components unless the project is only a single-file demo.

If a high-level component cannot match the live-console layout, compose the surface
manually. For example, a resource table can be a local white wrapper with a toolbar,
plain table, hover styles, row selection styles, and footer pagination using kube-design
buttons, inputs, dropdowns, status dots, and icons. Matching the live visual system is more
important than forcing every piece through a single high-level component.

### Component Decision Rule

Use the most faithful option available:

1. Existing consuming-app shell/list component, if available and already matches this guide.
2. Public `kube-design` component that matches the required size, density, and states.
3. Local composition around kube-design primitives and plain CSS.

Do not choose option 2 when it visibly breaks the live-console frame. A hand-composed
toolbar/table surface is better than an incorrect high-level component.

Avoid importing or naming private business components in generated output unless the user
is explicitly working inside that private repository. This guide is intended to work with
public `kube-design` plus local composition code.

## Responsive Behavior

Desktop is the primary enterprise console target.

- At `1440px`, match the baseline proportions.
- At `1920px`, allow the main area to expand, but preserve compact density.
- Do not scale type up on wide screens.
- Do not turn tables into sparse full-bleed dashboards.
- On narrower screens, sidebar can collapse or become overlay if the product shell supports it.
- Tables may scroll horizontally on small screens.
- Toolbar controls should wrap cleanly without overlapping text.

## Anti-Patterns

Reject and revise if any of these appear:

- Dark left sidebar or dark global nav by default.
- Generic admin template shell.
- Missing official logo image.
- Fake branding such as `LogoText Kube Sphere`, generated SVG marks, random app icons, or
  text-only `KubeSphere` branding.
- Top nav missing entirely on a full console page.
- Missing persistent Cluster/Workspace management entries in the top nav.
- Missing Component Dock before the profile menu.
- Component Dock replaced by a gear/settings icon.
- Sidebar missing scoped selector on scoped resource pages.
- Scoped selector rendered as an oversized card with a large left icon, instead of a compact
  field-like switcher.
- Sidebar using section-caption blocks instead of the resource tree pattern.
- Parent sidebar labels wrapping to two lines or increasing the `36px` row height.
- Child sidebar rows rendered with icons by default; child rows should be text-only unless
  explicitly requested.
- Sidebar active state as dark filled row, thick left border, or large colored pill.
- Sidebar icons from lucide, emoji, custom placeholder SVG, or random abstract icons.
- Mixed Chinese and English labels in the same generated page.
- Bilingual duplicate labels such as `Deployments / 部署`.
- Fonts smaller than `12px` anywhere in the console UI.
- Page title rendered as a hero.
- Breadcrumb navigation on generated console pages unless the user explicitly requests it.
- Toolbar/table/pagination split into separate cards.
- Toolbar missing the fixed refresh action.
- Toolbar missing the fixed Cogwheel/custom-column action.
- Resource search rendered as a narrow fixed-width pill instead of filling the toolbar
  center channel.
- Resource search rendered as a plain square white input instead of the FilterInput-style
  pale rounded control.
- Table header/body touching the list surface edge instead of using `margin: 0 12px 12px`.
- Pagination rendered as a plain text summary instead of an attached footer control.
- Table stretched so wide that columns become sparse and hard to scan.
- Only 4-5 rows shown on a full list page with available mock data.
- Oversized filters and large form controls in the table toolbar.
- Card-heavy dashboard layout for a resource list page.
- Gradients, bokeh/orbs, glassmorphism, decorative illustrations, or landing-page composition.

## Implementation Checklist

Before considering a generated page visually acceptable:

- The implementation does not require private console repositories unless the current user
  explicitly provided and requested them.
- UI language is consistent across navigation, selectors, buttons, table headers, and
  pagination.
- All visible UI text is at least `12px`.
- CSS follows the consuming project's style approach and preserves the strict values in the
  CSS contract.
- Global top nav is present and `64px` high.
- The exact official logo URL is used unless an equivalent official project asset is
  explicitly available.
- Logo area, Cluster/Workspace management entries, and Component Dock match KubeSphere proportions.
- Sidebar is light, `220px` by default, with scoped selector and resource tree.
- Sidebar icons are from `@kubed/icons` and match resource semantics.
- Child sidebar rows are text-only by default.
- Active sidebar state is quiet green text/icon by default.
- Page header is compact, white, `56px` by default, with `18px` title.
- Generated console pages do not include breadcrumb navigation unless the user explicitly requests it.
- Content starts with `20px` padding on pale blue-gray workspace.
- List page uses one dominant white list surface.
- Toolbar is compact and integrated with the table.
- Toolbar right side contains Refresh, Cogwheel/custom columns, then the dark create/action
  button.
- FilterInput-style search fills the center toolbar channel.
- Table main wrapper is inset with `margin: 0 12px 12px`.
- Table rows are dense, about `56px`, with realistic resource data.
- Pagination is attached to the table surface.
- Empty state follows the same list surface and density.
- The first viewport looks like KubeSphere Enterprise, not a generic admin UI.
