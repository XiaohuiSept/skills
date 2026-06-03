---
name: kube-design-agent
description: Use when building, modifying, or reviewing React frontend pages that must look like the KubeSphere Enterprise console using public kube-design primitives. Focus on high-fidelity generic console resource list pages with the KubeSphere header, Cluster/Workspace management entries, Component Dock, light sidebar, scoped selector, compact page header, integrated list table, and pagination.
---

# Kube Design Agent

This skill guides agents to generate KubeSphere-style console pages with public
`kube-design`.

Current priority: **high-fidelity basic resource list pages**. Do this well before
attempting detail pages, creation flows, dashboards, or advanced business workflows.

## Core Contract

Use `DESIGN.md` as the self-contained visual contract. It already distills the live console
appearance and `kse-console-kse` implementation patterns into portable rules.

Generated pages must:

- Look like KubeSphere Enterprise console pages, not generic admin templates.
- Use public `kube-design` primitives and `@kubed/icons`.
- Avoid dependencies on private console packages or business stores.
- Build missing shell/list pieces locally when the consuming app does not provide them.
- Preserve the full console frame: header, management entries, Component Dock, sidebar,
  scoped selector, page header, list surface, toolbar, table, and pagination.

## Dependency And Reference Policy

Assume the user does not have a local `kube-design` source checkout. Never require one.

Required inputs are only:

- this `SKILL.md`
- the companion `DESIGN.md`
- the consuming project with installed `@kubed/*` packages

For component and icon APIs, use this priority:

1. Existing imports and usage patterns in the consuming project.
2. Installed `@kubed/components` and `@kubed/icons` package types/build feedback.
3. Public `kubesphere/kube-design` upstream repository or package documentation, only when
   internet access is available.
4. A local `kube-design` checkout, only if the user explicitly provides one.

Do not block page generation because the public upstream or local source is unavailable.
If an exact component or icon API is uncertain, choose the closest public kube-design
primitive, keep the visual contract from `DESIGN.md`, and let TypeScript/build feedback
guide corrections.

Never require, inspect, import from, or depend on private console repositories such as
`kse-console-kse` unless the user explicitly provides that repository and asks to use it in
the current task.

## Required Reading Order

1. Read this `SKILL.md`.
2. Read the companion `DESIGN.md` provided with this skill.
3. Generate the page using public `@kubed/components` and `@kubed/icons` APIs available in
   the consuming project.
4. Verify the first viewport against `DESIGN.md`.

If sources conflict:

- Component API correctness follows the priority in **Dependency And Reference Policy**.
- Visual appearance comes from `DESIGN.md`.
- This `SKILL.md` controls workflow and validation.

Screenshots, stories, and examples must not override `DESIGN.md` unless the user explicitly
asks for a new visual direction.

## First Steps

Before generating UI:

1. Identify the page type. Default to `basic resource list page`.
2. Identify the management view:
   - `clusters`: cluster-scoped infrastructure and cluster resource pages.
   - `workspaces`: enterprise-space, project, member, role, and workspace-owned app pages.
3. Check whether the consuming app already has a matching shell/list component.
4. If no matching shell exists, create a small local shell wrapper.
5. If no high-level table component matches the required density, compose the list surface
   manually with kube-design primitives and local CSS.

Do not start by building only a table. Build the console frame first.

## Runtime Baseline

Wrap generated UI with kube-design provider and baseline when the consuming project does not
already do so:

```tsx
import { CssBaseline, KubedConfigProvider } from '@kubed/components';

export function AppShell({ children }: { children: React.ReactNode }) {
  return (
    <KubedConfigProvider themeType="light" locale="zh">
      <CssBaseline />
      {children}
    </KubedConfigProvider>
  );
}
```

Use `@kubed/components` for primitives and `@kubed/icons` for product/resource/action
icons. Confirm imports with the consuming project's installed packages, type feedback, or
build output.

## Default Page Structure

When the user asks for a generic page or only gives a resource name, generate this structure:

```text
global-header
  logo
  management-view-entries
    Cluster
    Workspace
  optional extension/custom entries
  component-dock
  profile-menu
resource-sidebar
  scoped-selector
  resource-tree
main
  page-header
    title
  content
    list-surface
      toolbar
      table
      pagination
```

Required local component pieces when no project shell exists:

- `ConsoleHeader`
- `ResourceSidebar`
- `PageHeader`
- `ListSurface`
- `ResourceIdentityCell`
- `StatusCell`
- `RowActions`

Build in this order:

1. Global top navigation.
2. Cluster/Workspace active state and matching scoped selector.
3. Left resource navigation.
4. Page title band.
5. Content area and integrated list surface.
6. Toolbar search/actions.
7. Table rows.
8. Pagination.
9. Empty/loading/error states.

## Portable Composition

`@kubed/components` provides primitives, not a complete KubeSphere product shell. Do not
assume these exist unless the consuming project already imports or documents them:
`TopNav`, `SideNav`, `ConsoleShell`, `ResourceSidebar`, `ListPage`, `DataTable`,
`NavMenu`, `NavSwitcher`, `useActionMenu`.

When a private or consuming-app component is unavailable, decompose it:

| Console role | Portable construction |
|---|---|
| Header | local `header` + official KubeSphere logo + flex layout + kube-design buttons/dropdowns/icons |
| Management entries | `Cluster` and `Workspace` icons from `@kubed/icons`; active dark buttons use light icon variant |
| Component Dock | local button using `/assets/grid.svg` if available |
| Sidebar tree | local `aside/nav` + resource icons + chevrons |
| Scoped selector | local field-like block with title, subtitle, chevron |
| Page header | local white band with compact title |
| List surface | local white wrapper containing toolbar, table, footer |
| Identity cell | icon + bold name + muted secondary line |
| Status cell | `StatusDot` plus text |
| Action menu | `Dropdown` + `Menu` + `MenuItem` |

Missing private components are not an excuse to switch to a generic SaaS/admin layout.

## Data And Content

For visual generation without real data:

- Use 8-10 visible rows at desktop baseline.
- Use realistic names such as `nginx-frontend`, `api-gateway`, `redis-master`,
  `auth-service`.
- Include namespace/project, status, ready/total count, image or type, time, and row actions.
- Include mixed states: running/success, updating/warning, abnormal/error.
- Avoid placeholder rows such as `Item 1`, `Test`, `Foo`, or empty mock data.

## Component Selection

Prefer these public primitives:

| Need | Prefer |
|---|---|
| Buttons | `Button` |
| Search | `FilterInput` if exported; otherwise local FilterInput-style wrapper |
| Select/filter | `Select` |
| Dense list | `Table` or a local table wrapper |
| Status | `StatusDot`, compact `Tag` only when needed |
| Menus | `Dropdown`, `Menu`, `MenuItem` |
| Grouping | `Group` |
| Surface | `Card` or small local surface wrapper |
| Icons | `@kubed/icons` |

Use other components only when the task needs them. Do not add charts, metrics, tabs,
modals, drawers, sheets, or dashboards to a basic list page unless explicitly requested.

## Visual Regression Traps

Reject and revise if any of these appear:

- Dark sidebar or dark global header by default.
- Generic admin template shell.
- Missing full top navigation.
- Missing persistent Cluster/Workspace management entries.
- Cluster/Workspace entries rendered as text tabs or arbitrary shortcuts.
- Active top management view does not match route/scope.
- Active top management icon keeps dark/default duotone colors on a dark selected background.
- Missing Component Dock before the user profile menu.
- Component Dock merged into the user menu or rendered as a gear/settings button.
- Missing scoped selector on scoped resource pages.
- Sidebar scoped selector does not match the active management view.
- Scoped selector rendered as an oversized card or with a large decorative left icon.
- Fake logo text such as `LogoText Kube Sphere`.
- Parent sidebar labels wrap to two lines or increase row height.
- Sidebar active row uses dark fill, thick left bar, big pill, or loud background.
- Sidebar icons come from lucide, emoji, custom placeholder SVG, or random abstract icons.
- Breadcrumb navigation appears unless the user explicitly requests it.
- Toolbar, table, and pagination are split into separate cards.
- Table header has a gray/blue filled background.
- Search is a square white input or a narrow fixed-width pill.
- Pagination is plain text instead of attached controls.
- Selected sidebar duotone icons are single-color or keep stale gray fill.
- Only 4-5 rows show on a full list page while data is available.
- Default table text or filters are oversized.
- Decorative gradients, glass effects, bokeh/orbs, illustrations, or landing-page composition.

## Verification

Before finishing:

1. Confirm imports compile against the consuming project's installed `@kubed/*` packages.
2. Confirm sidebar icons use semantic `@kubed/icons` names; if a specific icon is unavailable,
   choose the closest semantic kube-design icon rather than using lucide, emoji, or custom
   placeholder SVG.
3. Verify shell fidelity:
   - `64px` white top header.
   - real KubeSphere logo from the consuming app theme/config or the official website
     `https://kubesphere.com.cn`; use restrained `KubeSphere` text only as a last fallback.
   - persistent Cluster/Workspace management entries when available.
   - active management view matches `/clusters...` or `/workspaces...`.
   - active top management icons use the light variant on dark selected backgrounds.
   - Component Dock appears before user profile.
   - light `220px` sidebar by default.
   - scoped selector title/subtitle/action match active view.
   - parent sidebar labels remain single-line and `36px` high.
   - compact page header; no breadcrumb by default.
4. Verify list fidelity:
   - one integrated toolbar/table/pagination surface.
   - `32px` controls.
   - FilterInput-style search fills the toolbar center channel.
   - white table header.
   - `56-58px` rows.
   - attached footer pagination.
   - realistic row data.
   - status dots and overflow actions.
   - selected sidebar duotone icons use `color: #00aa72` and `fill: #90e0c5`.
5. If runnable, inspect the desktop layout in a browser.
6. If tests/build/typecheck exist, run the relevant command.

Final responses should mention the shell/list patterns used and any assumptions caused by
missing shell components in public `kube-design`.
