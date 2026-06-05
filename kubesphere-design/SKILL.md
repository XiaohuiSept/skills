---
name: kubesphere-design
description: Design, build, or review high-fidelity KubeSphere Enterprise console-style React interfaces using public kube-design primitives for Kubernetes and cloud-native management workflows.
---

# KubeSphere Design

Generate high-fidelity KubeSphere Enterprise console-style React UI with public
`@kubed/components`, semantic `@kubed/icons`, and portable local composition.

This skill optimizes for one outcome: the generated page must be immediately recognizable
as a KubeSphere Enterprise console page. The console frame comes first; resource content is
judged only after the header, sidebar, page shell, list surface, table, and pagination pass
the visual contract.

## Three-File Contract

Use all three files together:

| File | Role |
|---|---|
| `SKILL.md` | Agent workflow, dependency rules, build order, fallbacks, and verification |
| `DESIGN.md` | Visual source of truth: dimensions, colors, typography, language, shell/list rules |
| `IMPLEMENTATION.md` | Portable React/CSS blueprint for the shell, resource list, actions, and pagination |

Do not duplicate or invent competing visual values in generated code. If a visual value is
needed, take it from `DESIGN.md`. If the consuming app has no equivalent console shell,
start from `IMPLEMENTATION.md` and adapt it.

## Inputs And References

Assume the user may only provide this skill folder and a consuming project with installed
`@kubed/*` packages.

Allowed:

- Read this `SKILL.md`, `DESIGN.md`, and `IMPLEMENTATION.md`.
- Inspect the consuming project for existing imports, theme setup, shell components, and
  build conventions.
- Consult public `kubesphere/kube-design` upstream or package docs when internet access is
  available.

Not allowed:

- Requiring a local `kube-design` checkout.
- Requiring, importing from, or depending on non-public repositories.
- Replacing missing console shell components with a generic admin template.

## Conflict Resolution

Use this matrix when sources disagree:

| Decision area | Priority |
|---|---|
| User intent | Explicit user instruction wins unless it breaks the build or task |
| KubeSphere UI rules | `DESIGN.md`/`IMPLEMENTATION.md` rules > screenshots > generic kube-design docs |
| Component API/imports | Consuming project usage > installed package build/types > public upstream docs |
| Visual appearance | `DESIGN.md` wins for the portable visual contract |
| Portable structure | `IMPLEMENTATION.md` wins when no equivalent project shell/list component exists |
| Workflow/validation | This `SKILL.md` wins for reading order, fallbacks, gates, and verification |

If an exact API is uncertain, preserve the visual pattern in `DESIGN.md` first, choose the
closest available public primitive, then correct imports with TypeScript/build feedback.

## Required Reading Order

1. Read this file.
2. Read `DESIGN.md`, especially `Critical Visual Contract`.
3. For any build or substantial UI edit, read `IMPLEMENTATION.md`.
4. Inspect the consuming project for existing `@kubed/*` usage, locale setup, and shell/list
   components.
5. Generate or edit the page using project APIs plus local composition where needed.
6. Verify the first viewport against `DESIGN.md` and the gates below.

## Build Order

Do not begin with a standalone table. Build and verify in this order:

| Step | Piece | Rule |
|---:|---|---|
| 1 | `ConsoleHeader` | Real logo, management entries, Component Dock, profile |
| 2 | Active management view | Cluster/Workspace state and matching scoped selector |
| 3 | `ResourceSidebar` | Light sidebar, semantic duotone icons, nested nav |
| 4 | `PageHeader` | Compact title band, no breadcrumb by default |
| 5 | `ListSurface` | One integrated toolbar + table + pagination surface |
| 6 | Toolbar | Left filters, growing FilterInput, Refresh, Cogwheel, dark action |
| 7 | Table | Inset table, object identity cells, status, row actions, selection |
| 8 | Pagination | Attached kube-design-style footer |
| 9 | States | Empty/loading/error states using the same frame |

Header and sidebar must pass before table details are considered acceptable.

## Fidelity Gates

### Frame Gate

Fail and revise the output if any of these are missing:

- Real KubeSphere logo from `DESIGN.md`; no fake logo, text logo, generated mark, or random
  icon.
- `64px` white top header with `224px` brand area.
- Cluster and Workspace management entries as icon-only `36px` controls when both views are
  in scope.
- Active top entry: dark selected icon button plus `28px x 4px` bottom indicator.
- Inactive top Workspace/Cluster icons preserve duotone color and fill; do not set only one
  muted `color` prop.
- Component Dock before profile, labeled `组件坞` or `Component Dock`, not a gear/settings
  substitute.
- Profile menu with avatar, username, role/subtitle, and chevron.
- White `220px` sidebar with scoped selector and resource navigation; never a dark sidebar.
- Compact page header with title only and no breadcrumb by default.

### Navigation/Icon Gate

Fail and revise if:

- Sidebar icons are lucide icons, emoji, masks, custom black blocks, or single-color glyphs.
- Navigation/resource icons ignore the fixed icon map in `DESIGN.md`. Use that map for
  listed Chinese/English labels before guessing by name.
- Sidebar icons do not use semantic `@kubed/icons` names that match the menu/resource when
  the label is not in the fixed map.
- Active sidebar icon does not set both duotone channels:
  `color="#00aa72"` and `fill="#90e0c5"`.
- Active top icon on a dark button does not use the light duotone values from `DESIGN.md`.
- The scoped selector icon uses active green by default on a light selector background.
  Scope selector icons should normally use default dark duotone.
- Component Dock icon is hand-drawn with CSS squares or replaced by Cogwheel/settings. Use
  `Grid2Duotone`.
- Child menu rows show icons by default; child rows should be text-only unless an existing
  project shell proves otherwise.

### List Gate

Fail and revise if:

- Toolbar, table, and pagination are separate cards instead of one integrated surface.
- Search is a generic bordered input or isolated centered pill instead of the FilterInput
  shape from `DESIGN.md`.
- Toolbar right actions are missing, reordered, boxed, or filled. They must be Refresh,
  Cogwheel/custom columns, then dark primary action; icon actions are borderless text/icon
  triggers at rest.
- Toolbar icon actions are not implemented with kube-design text buttons when
  `Button` is available. Prefer the table toolbar pattern:
  `<Button variant="text" className="btn-refresh"><Refresh /></Button>` and the same
  pattern for Cogwheel/settings.
- Primary create/action button is green. It must be the dark command button.
- Create/action button shows an unnecessary leading plus icon. The default list toolbar
  create button is text-only unless the user or existing project shell requires an icon.
- Table body is flush to the surface. It must keep the `DESIGN.md` table inset.
- First data column does not use the Object Identity Pattern: `40px` semantic icon, `12px`
  gap, bold primary text, muted secondary text.
- First data column uses active green resource icons for every row. Table identity icons
  should normally use default dark duotone inside the `40px` icon area, not active sidebar
  colors.
- Status dots use gray/blue for running. Running/success uses green.
- Selected rows use thick green bars, mint fills, or blue fills. Use the thin-outline pale
  selection style from `DESIGN.md`.
- Row more actions are boxed/filled/text-only. Use compact borderless `More size={16}`.
- Pagination is generic numbered pages or plain footer text instead of the attached
  kube-design-style page-size/total/previous/current/next layout.

### Language And Type Gate

Fail and revise if:

- UI labels mix Chinese and English or render bilingual duplicates such as `容器组 (Pods)`.
- Cloud-native terms are invented or loosely translated. Use `DESIGN.md` terminology.
- Any visible text is below `12px`.
- List page titles are oversized. Use the compact title sizing from `DESIGN.md`.

## Implementation Strategy

Use project components only when they already match the visual contract. Otherwise compose
locally from public primitives.

Do not assume these console-level components exist unless the consuming project already
imports them: `TopNav`, `SideNav`, `ConsoleShell`, `ResourceSidebar`, `ListPage`,
`DataTable`, `NavMenu`, `NavSwitcher`, `useActionMenu`.

When unavailable, use the portable roles from `IMPLEMENTATION.md`:

| Console role | Portable construction |
|---|---|
| Header | Local header + real logo + flex layout + kube-design icons/dropdowns |
| Management entries | `Cluster` and `Workspace` icons; active dark button and light icon |
| Component Dock | Local labeled pill using `Grid2Duotone` |
| Sidebar | Local light `aside/nav`, scoped selector, semantic resource icons |
| Page header | Local white title band |
| List surface | Local white wrapper containing toolbar, table, footer |
| Row actions | `Dropdown` + `Menu` + `MenuItem` with borderless `More` trigger |

Use these button roles when `@kubed/components/Button` is available. The visual role comes
first; the exact props are the public implementation path for that role.

| Role | Required kube-design button |
|---|---|
| Primary | `<Button variant="filled" color="secondary" radius="xl" size="sm">` |
| Secondary | `<Button variant="filled" color="default" radius="xl" size="sm">` |
| Toolbar icon | `<Button variant="text">` with table toolbar classes |
| Row more | `<Button variant="text" radius="lg">` with `<More size={16} />` |
| Pagination icon | `<Button variant="text" radius="sm">` with `<Previous/Next size={20} />` |
| Dangerous | `<Button variant="filled" color="error" radius="xl" size="sm">` |

Text action buttons are generally `32px` high with pill radius and about `0 20px`
horizontal padding. Icon button radius varies by context (`sm`, `lg`, or unstyled text
button classes); do not force every icon button to `xl`. If `Button` is
unavailable or its installed API differs, adapt to the closest supported public API. Use a
local native button only as a fallback, and match the same role semantics.

## Adaptation Rules

Start from `IMPLEMENTATION.md` and adapt only what the requested resource requires:

- Resource kind, active menu key, title, and locale labels.
- Resource-specific icon from the fixed icon map in `DESIGN.md`; if absent, choose the
  closest semantic `@kubed/icons` component by name.
- One primary identity column plus one to three resource-specific columns.
- Mock data values and statuses.
- Imports required by the installed package version.

Keep these stable unless the user explicitly asks otherwise: header hierarchy, sidebar
hierarchy, scoped selector, page header, toolbar zones, table inset, object identity cell,
selected row behavior, row action trigger, and pagination structure.

## Data And Locale

Use realistic Kubernetes/cloud-native mock data: 8-10 visible rows, mixed statuses, real
resource-like names, namespaces/projects, timestamps, and one or two resource-specific
columns.

Minimum table pattern:

| Column | Requirement |
|---|---|
| Selection | Checkbox column |
| Name | Object Identity Pattern with semantic icon |
| Status | Status dot + localized text |
| Scope | Namespace/project/workspace/cluster as relevant |
| Specifics | 1-3 columns such as image, pods, type, ports, replicas |
| Time | Muted created/updated time |
| Actions | Borderless `More` dropdown |

Locale follows the user prompt or project locale. English UI keeps standard Kubernetes
resource names. Chinese UI uses established KubeSphere/Kubernetes terms from `DESIGN.md`.
If unsure, keep the Kubernetes original term instead of inventing a translation.

## Fallback Strategy

For detail pages, creation flows, dashboards, or other views not fully covered yet:

- Still use the same header, sidebar, scoped selector, page header, typography, and locale
  contract.
- Reuse list-page primitives where they fit.
- Prefer conservative KubeSphere-style local composition over generic admin UI.
- Mention in the response that the reusable list pattern is the strongest covered pattern
  if the requested view goes beyond it.

## Verification

### Static Verification

Always check:

1. `DESIGN.md` logo URL is used for the header logo.
2. No fake logo, dark sidebar, breadcrumbs by default, bilingual duplicate labels, or text
   under `12px`.
3. Header and sidebar pass the Frame Gate.
4. Active/sidebar icons pass the duotone channel rules.
5. Toolbar, table inset, selection, row actions, and pagination pass the List Gate.
6. Public imports compile against the consuming project's installed packages.

### Runtime Verification

When the environment supports it:

1. Run typecheck/build/lint commands used by the project.
2. Inspect at `1440x900` and `1920x1080`.
3. Confirm the search control grows across the toolbar, sidebar labels do not wrap, table
   density is stable, selected rows match `DESIGN.md`, and the first viewport reads as one
   KubeSphere console frame.

Do not finish by saying a generic page is "close enough" if any fidelity gate fails.
