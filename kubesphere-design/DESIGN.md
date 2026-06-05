# KubeSphere Console Design Guide

This file is the visual source of truth for generated KubeSphere Enterprise console-style
pages. Use it with:

- `SKILL.md` for workflow, dependencies, fallbacks, and verification.
- `IMPLEMENTATION.md` for the portable React/CSS shell and list-page blueprint.
- The consuming project's installed `@kubed/components` and `@kubed/icons` for actual
  component APIs.

The rules here are self-contained. Generated pages must not require private repositories,
private packages, or local source checkouts.

## Design Goal

Generated UI should look like a KubeSphere Enterprise console page, not a generic admin
template. The style is light, compact, operational, and data-dense: white console surfaces
on pale blue-gray workspace chrome, precise typography, quiet navigation, semantic duotone
icons, status-rich tables, and attached pagination.

Phase 1 focuses on reusable console management pages. The resource list pattern is the
strongest reference, but the same header, sidebar, page header, locale, typography, icon,
and object identity rules apply to other console pages.

The most important rule: the whole console frame must be recognizable before resource
content is judged.

## Critical Visual Contract

Reject and revise generated UI if any rule below is broken:

- Brand: use
  `https://ks-com-cn-staging.pek3b.qingstor.com/images/logo.svg` for the top-left logo.
  Do not use generated artwork, text-only branding, random icons, or `LogoText`.
- Header: `64px` high, white, `224px` logo area, Cluster/Workspace icon entries, active
  dark `36px` icon button with `28px x 4px` bottom indicator, Component Dock before
  profile, and profile with avatar, username, role/subtitle, chevron.
- Component Dock: label is `组件坞` in Chinese and `Component Dock` in English. Use
  `Grid2Duotone`. Never replace it with a gear/settings action or a hand-drawn CSS grid.
- Sidebar: `220px`, white, `16px 12px 12px` padding, `196px x 68px-70px` scoped selector,
  parent rows `36px`, child rows text-only by default, no dark sidebar.
- Icons: use semantic `@kubed/icons` duotone icons. Active sidebar icons must set
  `color="#00aa72"` and `fill="#90e0c5"`. Active top icons on dark buttons use light
  values: `color="rgba(255,255,255,0.9)"` and `fill="#ffffff66"`.
- Page header: white `56px` title band, title only by default, `18px` bold title, no
  breadcrumb unless explicitly requested.
- List surface: one integrated white surface containing toolbar, table, and pagination.
  Do not split them into independent cards.
- Toolbar: `#f9fbfd`, `10px 20px`, left filters, growing center FilterInput search, right
  Refresh, Cogwheel/custom columns, then dark text-only primary command.
- Table: table main inset `0 12px 12px`, white header, `56px` rows, first data column uses
  the Object Identity Pattern, row action is borderless `More size={16}`.
- Selected row: pale `#eff4f9`/`accents_1` cells plus thin `1px` green outline. No thick
  left bar, mint fill, or blue fill.
- Pagination: attached `#f9fbfd` footer with page size, divider, total, previous icon,
  `1 / N`, next icon. No numbered page buttons.
- Language/type: one locale per page, no bilingual duplicates, no font below `12px`.
- Buttons: follow the button roles in this document; use kube-design `Button` variants as
  the public implementation API when available.

## Tokens

Use these values as the strict default contract for generated pages. If the consuming
project already defines equivalent theme tokens, map to those semantics instead of creating
competing palettes.

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
  --ks-command: #242e42;
  --ks-command-hover: #181d28;
  --ks-warning: #f5a623;
  --ks-error: #ca2621;
}
```

### Color Roles

| Role | Value | Use |
|---|---:|---|
| Workspace background | `#eff4f9` | Main chrome behind content |
| Header/sidebar/surface | `#ffffff` | Product frame, cards, table surfaces |
| Subtle surface | `#f9fbfd` | Toolbars, pagination, light icon/action zones |
| Hover/selection base | `#eff4f9` | Quiet hover and selected table cells |
| Border | `#e3e9ef` / `#ccd3db` | Dividers, inputs, selector borders |
| Primary text | `#242e42` | Titles, names, strong labels |
| Secondary text | `#79879c` | Descriptions, helper text, time |
| Nav text | `#4a5974` | Inactive sidebar labels |
| Active/success | `#55bc8a` | Active text, running dots |
| Dark command | `#242e42` | Create/confirm command button |
| Warning | `#f5a623` | Updating/pending |
| Error | `#ca2621` | Failed/destructive |

Dark surfaces are rare: active top management entries, primary command buttons, and
tooltips. The sidebar is never dark.

## Icons

KubeSphere icons are duotone. Generated UI must use semantic `@kubed/icons` for console,
navigation, resource identity, and action icons.

| Context | Component | Primary/color | Secondary/fill |
|---|---|---:|---:|
| Light background, inactive | default `@kubed/icons` | package default | package default |
| Active sidebar item | semantic icon | `#00aa72` | `#90e0c5` |
| Active top icon on dark button | semantic icon | `rgba(255,255,255,0.9)` | `#ffffff66` |

Set both channels in React. Do not recolor only with CSS `color`.

```tsx
<Backup size={20} />
<Backup size={20} color="#00aa72" fill="#90e0c5" />
<Cluster size={24} color="rgba(255,255,255,0.9)" fill="#ffffff66" />
```

Never use lucide icons, emoji, CSS mask icons, monochrome SVG placeholders, or hand-drawn
black blocks for resource/navigation icons.

Use the fixed terminology and icon map below before guessing by name. It is the canonical
source for both Chinese/English display text and `@kubed/icons` selection in navigation
menus, resource table identity cells, list cards, selector rows, and other icon+name object
patterns. If a label is not listed, then choose the closest semantic `@kubed/icons`
component by exact resource/menu name and use established Kubernetes/KubeSphere wording.

| Chinese | English | `@kubed/icons` |
|---|---|---|
| 集群 | Cluster | `Cluster` |
| 企业空间 | Workspace | `Workspace` |
| 概览 | Overview | `Dashboard` |
| 节点 | Nodes | `Nodes` |
| 项目 | Projects / Namespace | `Project` |
| 工作负载 | Workloads | `Appcenter` |
| 部署 | Deployments | `Backup` |
| 有状态副本集 | Statefulsets | `StatefulSet` |
| 守护进程集 | Daemonsets | `DeamonSet` |
| 任务 | Jobs | `Backup` |
| 定时任务 | Cronjobs | `Backup` |
| 容器组 | Pods | `PodDuotone` |
| 工作负载模板 | Workload Templates | `NoteCopyDuotone` |
| 弹性伸缩 | Elastic Scaling | `Stretch` |
| 服务与网络 | Service And Network | `Earth` |
| 服务 | Services | `Appcenter` |
| 应用路由 | Ingresses | `Loadbalancer` |
| 网关 | Gateway | `GatewayDuotone` |
| 网络策略 | Network Policies | `Firewall` |
| 容器组 IP 池 | Pod IP Pools | `EipGroup` |
| 存储 | Storage | `Database` |
| 持久卷声明 | Persistent Volume Claims | `Storage` |
| 卷快照 | Volume Snapshot | `Snapshot` |
| 配置 | Configuration | `Hammer` |
| 保密字典 | Secrets | `Key` |
| 配置字典 | Configmaps | `Hammer` |
| 服务帐户 | Service Accounts | `Client` |
| 定制资源定义 | CRDs | `Select` |
| 监控告警 | Monitoring & Alerting | `Monitor` |
| 告警 | Alerts | `Loudspeaker` |
| 告警规则组 | Alerts Rule Groups | `BellGearDuotone` |
| 集群设置 | Cluster Settings | `Cogwheel` |
| 日志 | Logs | `Log` |
| 应用管理 | App Management | `AppsGearDuotone` |
| 应用仓库 | App Repositories | `Catalog` |
| 企业空间设置 | Workspace Settings | `Cogwheel` |
| 权限管理 | Access Control | `Key` |
| 用户 | Users | `Human` |
| 角色 | Roles | `Role` |
| 组件坞 | Component Dock | `Grid2Duotone` |
| 刷新 | Refresh | `Refresh` |
| 删除 | Delete | `Trash` |

Inactive top management icons must preserve both duotone channels as well. Do not set only
`color="#79879c"` on the inactive Workspace icon, because that drops the secondary fill and
makes the icon look flat. Either omit both props and use the package default, or set both
inactive channels explicitly.

## Object Identity Pattern

This is a core KubeSphere visual signature. Use it for table name columns, resource list
cards, selector rows, modal record rows, and detail headers.

```text
[40px semantic duotone icon] 12px gap [primary line + secondary line]
```

Rules:

- Icon area is `40px x 40px`, quiet, and aligned to the row center.
- Use the resource icon sizing defined for the resource kind. Many list identity cells use
  a `40px` icon/component directly, while smaller identity contexts may place a `20px-24px`
  icon inside a `40px` area.
- Table/list identity icons are normally inactive/default duotone. Do not use the active
  sidebar green pair (`#00aa72`/`#90e0c5`) for every row icon unless the row itself is a
  selected active navigation item.
- Resource-specific status overlays are allowed when the resource pattern calls for them,
  such as a small status indicator anchored to a `40px` Pod icon.
- Do not wrap each table icon in a heavy bordered card or avatar tile.
- Primary line: `12px-13px`, `600`, `#242e42`, single line ellipsis.
- Secondary line: `12px`, regular, `#79879c`, single line ellipsis.
- Text stack line-height: about `20px`.

## Typography And Language

Minimum font size is `12px`. Table text, badges, pagination, helper text, menu labels,
buttons, and secondary lines must not drop below `12px`.

| Text role | Size | Weight | Color |
|---|---:|---:|---|
| Page title | `18px` | `600` | `#242e42` |
| Section/table title | `12px-13px` | `600` | `#242e42` |
| Body/table text | `12px-13px` | `400` | `#242e42` |
| Secondary/help | `12px` | `400` | `#79879c` |
| Sidebar parent | `13px` | `600` when active | `#4a5974` / `#55bc8a` |
| Sidebar child | `12px-13px` | `400` or `600` active | `#4a5974` / `#55bc8a` |
| Button text | `12px-13px` | `600` | button-dependent |

## Button Usage

Use these button roles when `Button` from `@kubed/components` is available. The visual role
comes first; the props below are the public implementation path for that role. Do not treat
generic kube-design demo snippets as more authoritative than the concrete rules in this
document.

| Role | Component |
|---|---|
| Primary action | `<Button variant="filled" color="secondary" radius="xl" size="sm">Create</Button>` |
| Secondary action | `<Button variant="filled" color="default" radius="xl" size="sm">Cancel</Button>` |
| Toolbar icon action | `<Button variant="text" className="btn-refresh"><Refresh /></Button>` |
| Table row more | `<Button variant="text" radius="lg"><More size={16} /></Button>` |
| Pagination icon action | `<Button variant="text" radius="sm"><Previous size={20} /></Button>` |
| Dangerous action | `<Button variant="filled" color="error" radius="xl" size="sm">Delete</Button>` |

Button shape:

- Height: `32px`.
- Text action radius: usually `xl` / visual `32px` pill.
- Text button padding: about `0 20px`.
- Icon-only text buttons use the same `32px` control height and no border/background at
  rest.
- Icon-only radius is context-specific: toolbar buttons can rely on table toolbar classes,
  row more commonly uses `radius="lg"`, and pagination commonly uses `radius="sm"`.

Usage rules:

- Most generated pages should use only primary and secondary buttons.
- List toolbar create buttons are primary text-only buttons. Do not add a plus icon unless
  the user explicitly asks or an existing project shell does.
- Toolbar Refresh and Cogwheel/custom-column actions use icon text buttons.
- Table row `More` actions use icon text buttons, usually `variant="text" radius="lg"`.
- Modal footer pattern: cancel is secondary, confirm is primary.
- Dangerous confirmation pattern: cancel is secondary, confirm/delete is dangerous.
- Do not use green buttons for create/confirm actions. Green is reserved for active,
  running, success, and selected icon states.

Use one locale per generated page. Follow the consuming project locale if known; otherwise
follow the user's prompt language. Do not render bilingual duplicates such as
`容器组 (Pods)` or `Deployments / 部署`.

For resource/menu labels, use the fixed terminology and icon map in `## Icons` as the
single source of truth. Do not create a second translation table for labels that are
already listed there.

Additional UI terms without fixed icons:

| English | Chinese |
|---|---|
| Running | 运行中 |
| Updating | 更新中 |
| Waiting | 等待中 |
| Error | 异常 |
| Create | 创建 |
| Cancel | 取消 |
| Confirm | 确定 |
| Platform Admin | 平台管理员 |

If unsure about a Kubernetes resource name, keep the standard Kubernetes English term
rather than inventing a translation.

## Console Frame

### Global Header

Baseline:

- Height: `64px`.
- Background: `#ffffff`.
- Bottom border: `1px solid #e3e9ef`.
- Layout: brand area, management entries, flexible spacer, Component Dock, divider, profile.
- No page title in the header.

Brand:

- Width: `224px`.
- Padding-left: `24px`.
- Logo image uses the exact URL in the Critical Visual Contract.
- Render as an `<img>` with appropriate alt text.
- Do not recreate the mark with text, CSS shapes, or generated images.

Management view entries:

- Normal desktop header shows icon-only controls for Cluster and Workspace views.
- Each entry target is `36px x 36px`.
- Inactive entry: subtle/light background or transparent, dark duotone icon.
- Inactive Workspace/Cluster icons must be duotone. Do not pass only a muted `color` prop;
  preserve defaults or provide both `color` and `fill`.
- Active entry: dark `#242e42` button, light duotone icon, shadow allowed, plus bottom
  indicator `28px x 4px`, radius `2px`, centered under the icon.
- Visible labels such as `集群管理`/`企业空间` should not appear next to the normal desktop
  icon controls; use `aria-label`, `title`, or tooltip.

Right actions:

- Component Dock sits before the user profile.
- Component Dock is a `36px` high light pill with an icon and text.
- Component Dock label is locale-specific: `组件坞` or `Component Dock`.
- Use `Grid2Duotone`.
- Do not draw the Component Dock icon with CSS squares or reuse the table Cogwheel icon.
- A vertical divider may separate Component Dock from the profile.
- Profile shows avatar, username, role/subtitle, and chevron.

### Sidebar

Baseline:

- Width: `220px`.
- Background: `#ffffff`.
- Border-right: `1px solid #e3e9ef`.
- Padding: `16px 12px 12px`.
- Do not use a dark sidebar, dark nav block, or marketing-style navigation.

Scoped selector:

- Size: about `196px x 68px-70px`.
- White surface with `1px` border `#ccd3db`, radius `4px`.
- Padding: about `12px`.
- Contains semantic scope icon, bold title, muted subtitle, and chevron.
- Cluster view selector subtitle: `Cluster` / `集群`.
- Workspace view selector subtitle: `Workspace` / `企业空间`.
- Scope selector icon should use the normal dark duotone resource/scope icon on the light
  selector background. Do not use active green unless the selector itself is explicitly in
  an active selected state.
- Do not compress this into a tiny button.

Navigation rows:

- Parent rows are `36px` high, single-line, vertically centered.
- Parent row icon: about `20px`.
- Parent label: `13px`, ellipsis instead of wrapping.
- Chevron for collapsible groups is on the right.
- Child rows are text-only by default, indented under the parent.
- Child rows do not show icons unless the consuming project shell already does.
- Active text is green `#55bc8a`.
- Active parent/sidebar icons use `color="#00aa72"` and `fill="#90e0c5"`.

Suggested menu structure:

```text
Cluster view:
  Cluster Status
  Workloads
    Deployments
    StatefulSets
    DaemonSets
    Jobs
    CronJobs
    Pods
  Services and Ingress
  Configuration

Workspace view:
  Overview
  Projects
  Application Workloads
  Services and Ingress
  Configuration
  Access Control
```

Use the menu labels and hierarchy that best match the requested page, but preserve the
visual shell.

## Page Layout

Desktop baseline:

```text
viewport
  header: 64px
  body: calc(100vh - 64px)
    sidebar: 220px
    main: flex, background #eff4f9
      page header: 56px
      content: padding 20px
```

Main content should not start at the viewport edge. The pale workspace background remains
visible around the list surface.

Page header:

- Height: `56px`.
- Background: `#ffffff`.
- Border-bottom: `1px solid #e3e9ef`.
- Title: `18px`, `600`, `#242e42`.
- Title-only by default.
- No breadcrumb unless explicitly requested.
- No descriptive subtitle/help paragraph by default.

## Resource List Page

### List Surface

Use one integrated white surface:

```text
ListSurface
  Toolbar
  TableMain
    Table
  Pagination
```

Surface rules:

- Background: `#ffffff`.
- Radius: `4px`.
- Border: `1px solid #e3e9ef` or subtle shadow only if project convention requires it.
- Overflow: hidden or clipped to preserve attached toolbar/table/footer.
- Do not nest cards inside cards.
- Do not turn the toolbar, table, and footer into separate floating cards.

### Toolbar

Baseline:

- Background: `#f9fbfd`.
- Padding: `10px 20px`.
- Min-height: about `52px-56px`.
- Layout: left filters, center growing search, right actions.
- Gap: `12px`.

Toolbar zones:

| Zone | Content |
|---|---|
| Left | Project/namespace/status filters, selects, compact field controls |
| Center | One growing FilterInput-style search |
| Right | Refresh icon, Cogwheel/custom columns icon, dark primary command |

The search control should stretch across available space. It should not be a small isolated
input unless the toolbar has many left filters and limited width.

### FilterInput

Search should resemble kube-design `FilterInput`:

- Height: `32px`.
- Radius: `18px`.
- Background: `#eff4f9`.
- Border: transparent at rest.
- Icon: `Magnifier`, `16px`, muted.
- Text: `12px-13px`.
- Placeholder follows locale.
- It grows with the toolbar and stays between left filters and right actions.

Avoid a generic square input with a strong border for the main list search.

### Toolbar Actions

Refresh and Cogwheel/custom-columns are icon-only, borderless, backgroundless text/icon
buttons at rest:

```tsx
<Button variant="text" className="btn-refresh"><Refresh /></Button>
<Button variant="text" className="btn-setting"><Cogwheel /></Button>
```

They should share the same compact shape as icon controls near the dark command button, not
become boxed tiles.

Primary create/action button:

- Use `<Button variant="filled" color="secondary" radius="xl" size="sm">`.
- Height: `32px`.
- Radius: visual `32px` pill.
- Padding: about `0 20px`.
- White bold text.
- Text only by default.
- Do not use green for create/action. Green is reserved for success/active/running states.

### Table

Table main:

- Inset: `margin: 0 12px 12px`.
- Background: `#ffffff`.
- The table should not be flush against the list surface left/right/bottom edges.

Header:

- Background: `#ffffff`.
- Height: about `44px`.
- Text: `12px`, `600`, muted `#79879c`.
- Divider: `1px solid #e3e9ef`.
- Do not use a strong gray/blue table-header fill.

Rows:

- Height: about `56px`.
- Divider: `1px solid #e3e9ef`.
- Text: `12px-13px`.
- Checkbox column width: about `44px-52px`.
- Resource identity column uses the Object Identity Pattern.
- Status column uses a `7px-8px` dot plus localized text.
- Row more action uses `More size={16}` in
  `<Button variant="text" radius="lg">`.

Selected row:

- Cells use pale `#eff4f9`.
- A thin `1px` green outline around the selected row is acceptable.
- Checkbox is checked with active green.
- Do not use thick left selection bars, mint-green row fills, blue fills, or oversized
  selection effects.

Hover:

- Use subtle `#f9fbfd`/`#eff4f9` changes only.
- Keep layout stable; no row height change.

### Status

| Status | Dot color | Text |
|---|---:|---|
| Running | `#55bc8a` | Running / 运行中 |
| Updating | `#f5a623` | Updating / 更新中 |
| Error | `#ca2621` | Error / 异常 |
| Waiting/Unknown | `#b6c2cd` | Waiting/Unknown / 等待中/未知 |

Running must not be gray or blue-gray.

### Row Actions

Use a compact overflow menu:

- Trigger: borderless/backgroundless icon button at rest.
- Icon: `More size={16}` from `@kubed/icons`.
- Preferred trigger:
  `<Button variant="text" radius="lg"><More size={16} /></Button>`.
- Menu: `Dropdown` + `Menu` + `MenuItem` when available.
- Destructive item text may use error color.
- Do not show large boxed ellipsis buttons or text-only action links in every row.

### Pagination

Attached footer:

- Background: `#f9fbfd`.
- Border-top: `1px solid #e3e9ef`.
- Height: about `48px-56px`.
- Padding: `0 20px`.
- Left: page size control, divider, total count.
- Right: previous icon, current page text `1 / N`, next icon.
- Icons: `Previous` and `Next` from `@kubed/icons`.
- Text: `12px`, muted/primary mix.
- No numbered page buttons for the generic list pattern.

## Component Usage

Prefer public kube-design primitives:

| Need | Preferred |
|---|---|
| Primary button | `Button variant="filled" color="secondary" radius="xl" size="sm"` |
| Secondary button | `Button variant="filled" color="default" radius="xl" size="sm"` |
| Toolbar icon button | `Button variant="text"` with table toolbar classes |
| Row more button | `Button variant="text" radius="lg"` |
| Pagination icon button | `Button variant="text" radius="sm"` |
| Dangerous button | `Button variant="filled" color="error" radius="xl" size="sm"` |
| Checkbox | `Checkbox` |
| Dropdown menu | `Dropdown`, `Menu`, `MenuItem` |
| Search icon | `Magnifier` |
| Refresh action | `Refresh` |
| Column settings | `Cogwheel` |
| Row more | `More` |
| Management views | `Cluster`, `Enterprise`/workspace-equivalent icon |
| Resource icons | Fixed icon map first; otherwise closest semantic resource icon |

If a public component's default style conflicts with this guide and no supported prop fixes
it, compose local markup/CSS around kube-design icons and primitives. Fidelity wins over
blind component use.

## Responsive Behavior

Desktop baseline is `1440px` wide; generated pages should also hold at `1920px`.

- Header remains `64px`.
- Sidebar remains `220px` on desktop.
- Search expands across available toolbar width.
- Table columns can redistribute, but row height and first-column identity layout remain
  stable.
- Labels use ellipsis, not wrapping, in sidebar, toolbar controls, and table identity cells.
- On narrow widths, sidebar may collapse or drawer if the consuming app supports it; keep
  the light visual style and semantic icons.

Do not scale font sizes with viewport width.

## Anti-Patterns

Reject these outputs:

- Fake logo, text-only logo, random generated brand mark, or missing logo.
- Dark sidebar, dark navigation rail, oversized admin dashboard shell.
- Visible `集群管理`/`企业空间` text next to normal desktop top management icons.
- Component Dock replaced with settings, extensions, apps, or a gear button.
- Sidebar icons from lucide/custom SVG/emoji instead of semantic `@kubed/icons`.
- Active sidebar icons recolored only through CSS text color.
- Child sidebar rows with default icons.
- Breadcrumbs or long explanatory page subtitles by default.
- Mixed Chinese/English UI labels or bilingual duplicate labels.
- Font below `12px`.
- Green create button.
- Generic bordered search input instead of FilterInput.
- Toolbar icon actions rendered as filled/bordered square tiles.
- Table header with strong tinted background.
- Table body flush to the outer surface.
- Table first column without the Object Identity Pattern.
- Resource icons rendered as heavy mini-cards or arbitrary diagrams.
- Running status with gray/blue-gray dot.
- Selected row with thick green side bar, mint fill, or blue fill.
- Boxed row more buttons.
- Numbered pagination buttons or plain text-only pagination.
- Floating card inside another card.

## Implementation Checklist

Before finalizing a generated page, confirm:

1. The real logo URL is used.
2. Header, Component Dock, profile, sidebar, scoped selector, and page header match this
   guide.
3. Top and sidebar icons use the correct duotone channels for their state/background.
4. Page language is single-locale and all text is at least `12px`.
5. Toolbar has left filters, growing FilterInput, Refresh, Cogwheel, and dark command.
6. Table main uses the `0 12px 12px` inset.
7. First column uses the Object Identity Pattern with semantic icon.
8. Selected row, row actions, status dots, and pagination match this guide.
9. The first viewport reads as one KubeSphere Enterprise console frame, not a generic admin
   template.
