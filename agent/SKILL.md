---
name: kube-design-agent
description: Use when building, modifying, or reviewing React frontend pages that must look like the KubeSphere Enterprise console using public kube-design primitives. Focus on high-fidelity generic console resource list pages with the KubeSphere header, Cluster/Workspace management entries, Component Dock, light sidebar, scoped selector, compact page header, integrated list table, and pagination.
---

# Kube Design Agent

Generate KubeSphere-style console pages with public `kube-design` primitives.

Current priority: **high-fidelity basic resource list pages**. For detail pages, creation
flows, dashboards, or advanced business workflows, keep the console frame accurate and say
that this skill currently covers the list-page surface pattern first.

## Core Contract

Use the companion `DESIGN.md` as the self-contained visual contract. This `SKILL.md`
controls workflow, dependency choices, fallbacks, and verification.

Generated pages must:

- Look like KubeSphere Enterprise console pages, not generic admin templates.
- Use public `@kubed/components` primitives and semantic `@kubed/icons`.
- Avoid dependencies on private console packages, private stores, and local source checkouts.
- Build missing shell/list pieces locally when the consuming app does not provide them.
- Preserve the full console frame before optimizing resource content.

## Inputs And References

Assume the user only provides:

- this `SKILL.md`
- the companion `DESIGN.md`
- a consuming project with installed `@kubed/*` packages

Never require a local `kube-design` checkout. Public `kubesphere/kube-design` upstream or
package documentation may be consulted only when internet access is available. Never require,
inspect, import from, or depend on private repositories such as `kse-console-kse` unless the
user explicitly provides that repository and asks to use it in the current task.

## Conflict Resolution

Use this matrix instead of one flat priority stack:

| Decision area | Priority |
|---|---|
| User intent | The user's explicit instruction wins unless it breaks the task or build. |
| Component API/imports | Consuming project usage > installed `@kubed/*` types/build feedback > public upstream docs. |
| Visual appearance | `DESIGN.md` wins for colors, spacing, shell structure, density, and active states. |
| Workflow and validation | This `SKILL.md` wins for build order, fallbacks, and verification. |
| Examples/screenshots | Useful references only; they do not override `DESIGN.md` unless the user asks. |

If an exact component or icon API is uncertain, choose the closest public kube-design
primitive, preserve the visual contract, and use TypeScript/build feedback to correct APIs.

## Required Reading Order

1. Read this `SKILL.md`.
2. Read the companion `DESIGN.md`.
3. Inspect the consuming project for existing shell/list components and `@kubed/*` imports.
4. Generate the page using available public APIs plus local composition where needed.
5. Verify the first viewport against `DESIGN.md`.

## Build Order

Do not start by building only a table. Build the console frame first.

| Step | Piece | Build locally if missing |
|---:|---|---|
| 1 | `ConsoleHeader` with logo, Cluster/Workspace entries, Component Dock, profile | Yes |
| 2 | Active management view and matching scoped selector | Yes |
| 3 | `ResourceSidebar` with light resource tree | Yes |
| 4 | `PageHeader` with compact title and no breadcrumb | Yes |
| 5 | `ListSurface` containing toolbar, table, footer | Yes |
| 6 | Toolbar search/actions | Yes |
| 7 | `ResourceIdentityCell`, `StatusCell`, row actions | Yes |
| 8 | Attached pagination | Yes |
| 9 | Empty/loading/error states | Yes |

## Runtime Baseline

When the consuming app does not already provide kube-design setup, wrap the generated UI:

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

Use the consuming project's existing locale/language if it is known. Do not mix Chinese and
English labels in the same navigation group unless the product already does so.

## Default Page Skeleton

Use this structure for a generic resource list page. Names and columns may change; the frame
order should not.

```tsx
function ResourceListPage() {
  return (
    <AppShell>
      <div className="ks-console">
        <ConsoleHeader activeView="clusters" />
        <div className="ks-console-body">
          <ResourceSidebar activeView="clusters" activeKey="deployments" />
          <main className="ks-main">
            <PageHeader title="Deployments" />
            <section className="ks-content">
              <ListSurface
                toolbar={<ListToolbar />}
                table={<ResourceTable rows={rows} />}
                pagination={<TablePagination />}
              />
            </section>
          </main>
        </div>
      </div>
    </AppShell>
  );
}
```

## Portable Composition

`@kubed/components` provides primitives, not a complete KubeSphere product shell. Do not
assume these exist unless the consuming project already imports or documents them:
`TopNav`, `SideNav`, `ConsoleShell`, `ResourceSidebar`, `ListPage`, `DataTable`, `NavMenu`,
`NavSwitcher`, `useActionMenu`.

When a private or consuming-app component is unavailable, decompose it:

| Console role | Portable construction |
|---|---|
| Header | local `header` + real KubeSphere logo + flex layout + kube-design buttons/dropdowns/icons |
| Management entries | `Cluster` and `Workspace` icons from `@kubed/icons`; active dark buttons use light icon variant |
| Component Dock | local button using `/assets/grid.svg` if available; otherwise semantic `@kubed/icons` fallback |
| Sidebar tree | local light `aside/nav` + semantic resource icons + chevrons |
| Scoped selector | local field-like block with title, subtitle, chevron |
| Page header | local white band with compact title, no breadcrumb |
| List surface | local white wrapper containing toolbar, table, footer |
| Identity cell | Object Identity Pattern: `40px` semantic icon + `12px` gap + two-line text |
| Status cell | `StatusDot` plus text |
| Action menu | `Dropdown` + `Menu` + `MenuItem` |

Missing private components are not an excuse to switch to a generic SaaS/admin layout.

## Component And Style Strategy

Prefer public kube-design primitives:

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

For CSS, follow the consuming project's style system first: existing styled/emotion
patterns, CSS modules, project CSS utilities, or colocated stylesheet conventions. Avoid
large inline `<style>` blocks in React components unless the project is a single-file demo.
Use CSS custom properties from `DESIGN.md` as the strict numerical and color anchors.

## Fallback Strategy

- Logo: use the consuming app theme/config logo or an official KubeSphere asset from
  `https://kubesphere.com.cn`; use restrained `KubeSphere` text only as a last fallback.
- Component Dock icon: use `/assets/grid.svg` when present; otherwise use a semantic
  `@kubed/icons` app/grid/dashboard-style icon available in the project.
- Resource icons: use the semantic `@kubed/icons` component by Kubernetes kind or menu name.
  If the exact icon is missing, choose the closest semantic kube-design icon. Do not use
  lucide, emoji, or random hand-drawn placeholders for resource menus.
- Component props: if docs/examples conflict with installed package types, adjust to the
  installed package.
- High-level table/list components: if they cannot match the live-console density, compose a
  local surface with kube-design primitives and CSS.

## Data And Content

For mock list pages, include 8-10 visible rows at desktop baseline. Minimum columns:

| Column | Example | Notes |
|---|---|---|
| Selection | checkbox | Compact, first column |
| Name | `nginx-frontend` | Object Identity Pattern: `40px` semantic icon, `12px` gap, bold title, muted description |
| Status | `Running`, `Updating`, `Error` | Dot plus text |
| Scope | `default`, `production` | Namespace/project/workspace as relevant |
| Resource attribute | `3/3`, `nginx:1.21`, `ClusterIP` | 1-3 resource-specific columns |
| Time | `2 hours ago` | Muted created/updated time |
| Actions | overflow menu | Far right |

Avoid placeholder rows such as `Item 1`, `Test`, `Foo`, or empty mock data.

## Visual Regression Traps

Reject and revise if any of these appear:

| Bad output | Correct target |
|---|---|
| Dark sidebar or dark global header by default | Light `64px` header and light `220px` sidebar |
| Missing Cluster/Workspace top entries | Persistent management-view entries in the header |
| Active top icon keeps dark colors on dark background | Use light icon variant on active dark top entry |
| Missing Component Dock before profile | Component Dock sits before the user profile menu |
| Fake logo text such as `LogoText Kube Sphere` | Real logo asset or restrained `KubeSphere` text fallback |
| Scoped selector oversized or with large decorative icon | Compact field-like selector tied to active management view |
| Parent sidebar labels wrap to two lines | `36px` single-line row with ellipsis |
| Child nav rows all have icons | Child rows are text-only by default unless requested |
| Name column is plain text or uses a small/generic avatar | Object Identity Pattern with `40px` icon and `12px` text gap |
| Sidebar active row uses dark fill, thick bar, or big pill | Quiet green text/icon active state |
| Sidebar icons from lucide/emoji/custom placeholder SVG | Semantic `@kubed/icons` |
| Breadcrumb navigation appears by default | No breadcrumb unless explicitly requested |
| Toolbar, table, and pagination split into cards | One integrated list surface |
| Gray/blue filled table header | White table header |
| Search is square, white, or narrow fixed-width | FilterInput-style search fills toolbar center |
| Pagination is plain text only | Attached footer controls |
| Only 4-5 rows on a full list page | About 8-10 visible rows when data exists |
| Decorative gradients/glass/hero/dashboard composition | Compact operational resource list |

## Verification

### Static Verification

Always check:

1. Imports compile against the consuming project's installed `@kubed/*` packages.
2. CSS uses the project style system and `DESIGN.md` tokens for strict values.
3. Top shell has `64px` white header, real/fallback KubeSphere logo, Cluster/Workspace
   entries, Component Dock, and profile menu.
4. Active top management view matches `/clusters...` or `/workspaces...`.
5. Active top icons use the light variant on dark selected backgrounds.
6. Sidebar is light, `220px` by default, with scoped selector matching active view.
7. Parent sidebar labels remain single-line and `36px` high; child rows are text-only by
   default.
8. No breadcrumb appears unless explicitly requested.
9. List page uses one integrated toolbar/table/pagination surface.
10. Search is FilterInput-style, `32px` high, and fills the toolbar center channel.
11. Resource name cells and resource list cards use the Object Identity Pattern: `40px`
    semantic icon, `12px` gap, bold primary title, regular muted description, ellipsis.
12. Table header is white; rows are about `56px`; pagination is attached.
13. Selected sidebar duotone icons use `--ks-icon-active` and `--ks-icon-active-fill`.

### Runtime Verification

When the environment supports it:

1. Inspect the desktop layout at the `1440px` baseline.
2. Check a wider viewport such as `1920px` for density preservation.
3. Run the relevant build, typecheck, or tests.

Final responses should mention the shell/list patterns used and any assumptions caused by
missing shell components in public `kube-design`.
