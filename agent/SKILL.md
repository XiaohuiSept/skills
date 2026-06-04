---
name: kubesphere-design
description: Design, build, or review KubeSphere Enterprise console-style React UI using public kube-design primitives. Use for KubeSphere-like console pages, especially resource list pages with the product header, light sidebar, scoped selector, integrated table toolbar, pagination, and Kubernetes resource identity patterns.
---

# KubeSphere Design

Generate KubeSphere Enterprise console-style pages with public `kube-design` primitives.

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
inspect, import from, or depend on private console repositories unless the user explicitly
provides one and asks to use it in the current task.

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

Do not start by building only a table. Build the console frame first, and do not treat the
table as acceptable until the header and sidebar pass the frame checks.

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

## Frame Fidelity Gate

The generated page fails if the product frame is not recognizable as KubeSphere, even when
the table content looks polished.

Before refining table data, verify:

- Header uses the real logo URL from `DESIGN.md`; never draw or spell out a fake logo.
- Header includes both Cluster and Workspace management entries when both views are in
  scope.
- Active top management icon uses a dark `36px` selected button and a light icon variant.
- Component Dock appears before the profile menu as a labeled `36px` pill action, not as a
  gear icon.
- Sidebar is light, `220px`, has the scoped selector, and uses semantic `@kubed/icons`.
- Sidebar parent rows are `36px`, single-line, and child rows are text-only by default.
- Page header has only the compact title band by default; no breadcrumb.

## Runtime Baseline

When the consuming app does not already provide kube-design setup, wrap the generated UI:

```tsx
import { CssBaseline, KubedConfigProvider } from '@kubed/components';

export function AppShell({
  children,
  locale,
}: {
  children: React.ReactNode;
  locale: 'en' | 'zh';
}) {
  return (
    <KubedConfigProvider themeType="light" locale={locale}>
      <CssBaseline />
      {children}
    </KubedConfigProvider>
  );
}
```

Use the consuming project's existing locale/language if it is known. If it is not known,
follow the main language of the user's prompt: English prompt -> English UI, Chinese prompt
-> Chinese UI. Never mix Chinese and English labels in the same generated page, and never
render bilingual duplicate labels such as `Deployments / 部署`.

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

- Logo: use the exact KubeSphere logo URL defined in `DESIGN.md`:
  `https://ks-com-cn-staging.pek3b.qingstor.com/images/logo.svg`. Do not generate a logo,
  draw a mark, use a random icon, or render text-only branding. Use a consuming-app logo
  only when it is already the official KubeSphere brand asset or the user explicitly asks.
- Component Dock icon: use `/assets/grid.svg` when present; otherwise use a semantic
  `@kubed/icons` app/grid/dashboard-style icon available in the project. Keep the label and
  pill shape; do not replace it with the table settings/cog action.
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

Use one locale for all generated UI text. In English UI, keep Kubernetes resource names in
their standard English form, such as `Deployment`, `Pod`, `Service`, `Ingress`, `Gateway`,
`ConfigMap`, and `Secret`. In Chinese UI, use KubeSphere terminology from `DESIGN.md`, such
as `部署`, `工作负载`, `容器组`, `企业空间`, `配置字典`, and `保密字典`. If a term is uncertain,
preserve the Kubernetes English kind rather than inventing a translation.

## Visual Regression Traps

Reject and revise if any of these appear:

| Bad output | Correct target |
|---|---|
| Dark sidebar or dark global header by default | Light `64px` header and light `220px` sidebar |
| Missing Cluster/Workspace top entries | Persistent management-view entries in the header |
| Active top icon keeps dark colors on dark background | Use light icon variant on active dark top entry |
| Missing Component Dock before profile | Component Dock sits before the user profile menu |
| Fake/generated/text logo such as `LogoText Kube Sphere` | Exact official logo URL from `DESIGN.md` |
| Scoped selector oversized or with large decorative icon | Compact field-like selector tied to active management view |
| Parent sidebar labels wrap to two lines | `36px` single-line row with ellipsis |
| Child nav rows all have icons | Child rows are text-only by default unless requested |
| Name column is plain text or uses a small/generic avatar | Object Identity Pattern with `40px` icon and `12px` text gap |
| Sidebar active row uses dark fill, thick bar, or big pill | Quiet green text/icon active state |
| Sidebar icons from lucide/emoji/custom placeholder SVG | Semantic `@kubed/icons` |
| Mixed Chinese/English UI labels | One locale per generated page |
| Any UI text below `12px` | Minimum font size is `12px` |
| Breadcrumb navigation appears by default | No breadcrumb unless explicitly requested |
| Toolbar, table, and pagination split into cards | One integrated list surface |
| Gray/blue filled table header | White table header |
| Search is square, white, or narrow fixed-width | FilterInput-style search fills toolbar center |
| Toolbar missing refresh or column settings | Fixed Refresh + Cogwheel actions before create |
| Table body touches the card edge | Table main inset uses `0 12px 12px` |
| Pagination is plain text only | Attached footer with page-size, total count, and controls |
| Only 4-5 rows on a full list page | About 8-10 visible rows when data exists |
| Decorative gradients/glass/hero/dashboard composition | Compact operational resource list |

## Verification

### Static Verification

Always check:

1. Imports compile against the consuming project's installed `@kubed/*` packages.
2. CSS uses the project style system and `DESIGN.md` tokens for strict values.
3. Top shell has `64px` white header, exact official KubeSphere logo URL,
   Cluster/Workspace entries, Component Dock, and profile menu.
4. Active top management view matches `/clusters...` or `/workspaces...`.
5. Active top icons use the light variant on dark selected backgrounds.
6. Sidebar is light, `220px` by default, with scoped selector matching active view.
7. Parent sidebar labels remain single-line and `36px` high; child rows are text-only by
   default.
8. No breadcrumb appears unless explicitly requested.
9. List page uses one integrated toolbar/table/pagination surface.
10. Search is FilterInput-style, `32px` high, and fills the toolbar center channel.
11. Toolbar right side includes Refresh, Cogwheel/custom columns, then the dark primary action.
12. Table main area is inset from the card with `0 12px 12px`.
13. Resource name cells and resource list cards use the Object Identity Pattern: `40px`
    semantic icon, `12px` gap, bold primary title, regular muted description, ellipsis.
14. Table header is white; rows are about `56px`; pagination is attached.
15. Selected sidebar duotone icons use `--ks-icon-active` and `--ks-icon-active-fill`.
16. UI text uses one locale and no font is smaller than `12px`.

### Runtime Verification

When the environment supports it:

1. Inspect the desktop layout at the `1440px` baseline.
2. Check a wider viewport such as `1920px` for density preservation.
3. Run the relevant build, typecheck, or tests.

Final responses should mention the shell/list patterns used and any assumptions caused by
missing shell components in public `kube-design`.
