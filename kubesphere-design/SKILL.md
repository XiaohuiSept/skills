---
name: kubesphere-design
description: Design, build, or review high-fidelity KubeSphere Enterprise console-style React interfaces using modular KubeSphere design rules, page shells, recipes, and public kube-design primitives.
---

# KubeSphere Design

Generate KubeSphere Enterprise console-style React UI with a recognizable product frame,
semantic `@kubed/icons`, public `@kubed/components`, and local composition only for
product shell pieces that kube-design does not export.

This skill is optimized for fidelity over creativity. Do not produce a generic admin UI.

## Public Package Rule

This skill is public and portable. Do not depend on private repositories, local screenshots,
local filesystem paths, internal test projects, Figma links, live-console login details, or
skill-development notes. Use only the rules and public package guidance contained in this
skill folder plus public `@kubed/components` and `@kubed/icons`.

## Core Workflow

1. Identify task type, locale, management view, resource type, and whether the page is a
   basic list page.
2. Read the files listed for that task in `Read By Task`, in order.
3. Resolve resource title, active menu, icon, columns, actions, status labels, and mock data
   from `assets/icon-map.md` and `assets/recipes.md` before inventing UI.
4. Pass the Primitive Dependency Gate before writing page code.
5. Build in frame-first order: Header -> Sidebar -> Page Header -> List Surface -> Toolbar
   -> Table -> Pagination.
6. Verify the frame before judging table details. If Header or Sidebar drifts, revise the
   frame first.
7. Verify imports, visible controls, language consistency, and public-package hygiene before
   finishing.

## Acceptance Order

Treat generation as a sequence of gates, not a loose checklist:

1. **Package gate**: imports include real `@kubed/components` and `@kubed/icons`, and the
   app is wrapped in `KubedConfigProvider`.
2. **Frame gate**: official logo, Header quick-nav, Component Dock, profile trigger,
   Sidebar scope selector, and menu hierarchy match `references/console-frame.md`.
3. **Icon gate**: mapped icons come from `assets/icon-map.md`; every forced duotone state
   sets both `color` and `fill`.
4. **List gate**: toolbar, table inset, object identity, row actions, selection, and
   pagination match `assets/recipes.md`.
5. **Language gate**: one locale only, with no bilingual duplicates and no text below
   `12px`.

Do not consider a page acceptable because the central table looks polished. Header and
Sidebar fidelity are higher priority than content decoration.

## Read By Task

### Resource List Page

| Priority | File | Purpose |
|---|---|---|
| MUST-READ | `DESIGN.md` | Overall KubeSphere visual contract |
| MUST-READ | `assets/page-shells/list.md` | Copyable list page shell |
| MUST-READ | `assets/tokens.md` | Color, size, spacing, and CSS variable truth |
| MUST-READ | `assets/icon-map.md` | UI terms, resource keys, and icon mapping |
| MUST-READ | `assets/recipes.md` | Resource, button, toolbar, table, status recipes |
| MUST-READ | `references/console-frame.md` | Header/sidebar/frame fidelity rules |
| MUST-READ | `references/components.md` | Required kube-design primitive usage |
| IF-NEEDED | `references/list-pages.md` | List page detail and edge cases |
| IF-NEEDED | `references/language.md` | Locale, terminology, and official glossary policy |
| IF-NEEDED | `references/case-studies/*` | Use when reviewing or fixing repeated failures |

### Page Review Or Visual Fix

Read `DESIGN.md`, `assets/tokens.md`, `assets/icon-map.md`, `assets/recipes.md`, and the
case study matching the failure. If the issue is frame-level, also read
`references/console-frame.md`; if it is table/list-level, read `references/list-pages.md`.

## Conflict Resolution

| Decision area | Priority |
|---|---|
| User intent | Explicit user instruction wins unless it breaks the task |
| Visual contract | `DESIGN.md` and `assets/tokens.md` win |
| Shell structure | `assets/page-shells/*` wins |
| Icons and terms | `assets/icon-map.md` wins |
| Component/resource choices | `assets/recipes.md` wins |
| Explanation and edge cases | `references/*` wins |
| Component API correctness | Consuming project imports/types > public kube-design docs > local fallback |

If a public component API differs from these examples, keep the visual role and adapt to the
installed API. Do not replace missing shell components with a generic admin template.

## Primitive Dependency Gate

KubeSphere fidelity depends on real kube-design primitives. For a new React project, install
or import public kube-design packages before generating the page:

```text
@kubed/components
@kubed/icons
```

When using `@kubed/components`, wrap the React root in the package provider before rendering
KubeSphere UI. For a standalone Chinese page, use
`<KubedConfigProvider locale="zh" themeType="light">`; for English, use `locale="en"`.
Missing the provider can make kube-design controls render incorrectly or crash at runtime.
Install any public peer dependencies required by the selected package version, especially
theme/runtime dependencies such as `styled-components`.

If the consuming project already has equivalent public kube-design imports, reuse them. If
the packages are unavailable and cannot be installed, stop and clearly state that the result
will be an approximation instead of silently hand-writing replacements.

Installing kube-design packages is not enough. A resource list page must actually use
`@kubed/components` for visible controls:

- `Button` for toolbar actions, command buttons, row more triggers, and pagination icon
  actions.
- `Checkbox` for table header and row selection.
- `Dropdown`, `Menu`, and `MenuItem` for row action menus.
- `Select` for project/filter selects when exported by the installed package.
- `FilterInput` when exported for toolbar search. Use `simpleMode` for plain keyword
  search. A local FilterInput-style wrapper is allowed only when the installed package does
  not export `FilterInput`.

If final page code imports only `@kubed/icons` and implements controls with raw
`button`, `select`, custom checkbox CSS, or custom dropdown state, it fails this gate.

Never create local substitutes for these primitives:

- `src/icons.tsx` or hand-drawn SVG copies of KubeSphere resource/navigation icons.
- `src/components.tsx` clones of `Button`, `Checkbox`, `Dropdown`, `Menu`, or `MenuItem`.
- Native-control clones for list controls, such as CSS-only checkboxes, raw select styling,
  or custom dropdown/menu popovers.
- CSS-only icon masks, emoji icons, lucide/react-icons/shadcn icons, or generic admin icon
  sets for mapped KubeSphere terms.

Local composition is allowed only for product shell roles that kube-design does not provide:
`ConsoleHeader`, `ResourceSidebar`, `PageHeader`, `ListSurface`, `ResourceIdentityCell`,
`StatusCell`, and pagination layout wrappers. These wrappers must still use public
`@kubed/components` and `@kubed/icons` inside them.

Header quick-nav entries, the Component Dock trigger, profile trigger, scope selector, and
sidebar menu rows are product shell controls. If no public kube-design component exports
that exact product shell behavior, compose them locally using semantic `@kubed/icons` and
the dimensions in `DESIGN.md`; do not replace them with generic admin widgets.

## CSS Strategy

Before writing styles, inspect the consuming project styling conventions.

1. Prefer the existing project pattern: Emotion/styled-components, CSS Modules, global CSS,
   or project-local style helpers.
2. Use `assets/tokens.md` values and CSS variable names as the visual source of truth.
3. Avoid inline `<style>{...}</style>` blocks in React output unless the target environment
   has no practical stylesheet path.
4. Use `!important` sparingly, only to correct public kube-design control outer dimensions,
   padding, background, or shadow when component styles otherwise override the product
   contract. Do not use it to rewrite component internals, create a new theme, or hide
   layout mistakes.
5. Use the font stack in `assets/tokens.md` for shell CSS and custom text. Do not put
   `Inter`, `Geist`, or generic `system-ui` before the kube-design/KubeSphere font stack.
6. Do not create a competing color palette or viewport-scaled typography.

## Fallback Strategy

- Icons: use `assets/icon-map.md`; if absent, choose the closest semantic `@kubed/icons`
  component by exact resource/menu name. Do not use lucide, emoji, CSS masks, or handmade
  black blocks for KubeSphere resource/navigation icons.
- Components: use installed `@kubed/components` APIs first. If a prop differs, adapt while
  preserving the role in `assets/recipes.md`.
- Shell: if the project lacks Header/Sidebar/DataTable components, compose locally from the
  list page shell, but do not clone kube-design primitives.
- Build errors: fix imports and prop names first; do not change the visual contract to make
  compilation easier.

## Fidelity Gates

Fail and revise if any of these occur:

- Fake/text logo, missing KubeSphere logo, or random brand icon.
- Dark sidebar, generic admin navigation, visible breadcrumbs by default.
- Missing Cluster/Workspace top management entries when the full console frame is in scope.
- Inactive top Workspace/Cluster icons render as black silhouettes or transparent-fill
  glyphs instead of a light circular quick-nav control with default/dark icon variant.
- Inactive top management icons omit explicit `color="#36435c"` and `fill="#b6c2cd"`.
- Active top management icons omit both light channels: `color="rgba(255,255,255,0.9)"`
  and `fill="#ffffff66"`.
- Component Dock replaced by settings/Cogwheel or CSS grid squares.
- Component Dock is restyled into a custom bordered chip instead of the no-border light
  pill product shape.
- Component Dock `Grid2Duotone` omits explicit default duotone channels
  `color="#324558"` and `fill="#b6c2cd"`.
- Profile menu is rendered as an oversized gray/bordered pill rather than a compact
  transparent account trigger.
- Profile trigger lacks the compact product structure: `32px` avatar, `12px` gap,
  username/role text stack, and chevron. Prefer `.ks-profile-trigger` for generated shell
  code.
- Profile username and role/subtitle are centered, staggered, or not left-aligned inside
  `.ks-profile-text`.
- Sidebar/resource icons ignore the fixed icon map or use single-color glyphs.
- Active sidebar duotone icons do not set both `color="#00aa72"` and `fill="#90e0c5"`.
- Scope selector has a leading Cluster/Workspace icon, a black/green-only glyph, or looks
  like an active nav item.
- Sidebar resource navigation starts more than `12px` below the scope selector.
- Sidebar menu labels use `600` or `700` font weight instead of the required `500`.
- Expanded sidebar parent rows use strong outlined/pill styling instead of quiet light rows.
- Expanded sidebar parent row remains inactive gray when one of its child items is active.
- Toolbar is not left filters + growing FilterInput + Refresh + Cogwheel + dark text-only
  command.
- Toolbar is built as one flat row where FilterInput can overlap Select or actions instead
  of three sibling zones: filters, search, actions.
- Dark primary create/action button omits explicit kube-design `shadow` prop.
- Refresh/Cogwheel toolbar buttons collapse into square `32px x 32px` icon buttons instead
  of `32px` high text buttons with `0 20px` horizontal padding.
- Refresh/Cogwheel toolbar buttons or row More buttons use command-button shadow instead of
  no-shadow `variant="text"` styling.
- Refresh/Cogwheel use raw `button` elements or custom icon wrappers instead of kube-design
  `Button variant="text"`.
- Toolbar search is hand-written as `label + input` even though public kube-design
  `FilterInput` is available.
- Toolbar search visually contains Refresh/Cogwheel/Create, or Refresh/Cogwheel are
  implemented as search suffix icons/adornments instead of separate right action buttons.
- Kube-design `Select` is styled with native select CSS such as `appearance`, data-URI
  chevrons, or raw option styling.
- Toolbar `Select` filter is missing the kube-design dropdown arrow or hides
  `.kubed-select-arrow`.
- Toolbar `Select` arrow is covered by selector text/search input, lacks explicit fallback
  `ChevronDown` suffix when needed, or is not right-aligned.
- Toolbar project Select root or `.kubed-select-selector` lacks a stable `240px x 32px`
  box, causing the search field to visually press into or overlap the selector.
- Toolbar Cogwheel is a decorative button only and does not open a column visibility menu.
- Toolbar Cogwheel opens the column visibility menu but clicking the same Cogwheel again
  does not close it. Do not set `hideOnClick={false}` or custom popover state that breaks
  trigger toggle behavior.
- Column visibility menu is rendered as checkboxes, a blue selected panel, a modal, or a
  custom popover unrelated to the DataTable settings menu. It must use kube-design
  `Dropdown placement="bottom-end" maxWidth={160}` with `Menu width={160}
  className="menu-setting"`. The menu is dark, `4px` radius, `padding: 4px 0`, shadowed,
  with `MenuLabel` title `定制内容` / `Custom Columns`; label padding is `8px 12px`. Rows
  are `MenuItem`s with `padding: 6px 12px`, regular `400` text, dark-hover background, and
  `Eye size={16}` for visible columns or `EyeClosed size={16}` for hidden columns.
  Preserve stable semantic classes such as `.menu-setting`, `.menu-label`, `.item-inner`,
  `.item-icon`, `.item-body`, and `.item-label`; do not copy generated `sc-*` hash classes.
- Column visibility includes the fixed first data column (`Name` / `名称`) or final
  operation column. Those columns must always stay visible and must not be listed as
  customizable.
- Table body is flush to the surface, lacks Object Identity Pattern, or uses wrong selected
  row styling.
- Table body inset is `12px` on all sides instead of `0 12px 12px`.
- Local table shell uses `border-collapse: separate`, bottom-border row separators, or
  fixed-height cells instead of the source-like `border-collapse: collapse`, `th` padding
  `16px 12px`, `td` padding `8px 12px`, and body `border-top: 1px solid #eff4f9` model.
- Table checkbox/select column is not fixed at `40px`.
- Ordinary single-line table body cells use muted secondary color or bold weight instead of
  primary text color and regular `400`.
- Table normal rows have no hover state, or hover changes text weight, adds a side bar, or
  overrides the selected-row outline. Normal row hover should be subtle pale blue-gray cells.
- Basic demo table opens with preselected rows instead of starting with no selected rows and
  supporting checkbox select/unselect.
- Selected row outline disappears on hover/focus because hover styles override
  `tr.row-selected td` or first/last cell border edges.
- Selected row is drawn with a thick side bar, mint/blue fill, or disconnected shadow
  fragments instead of real green cell borders around the row in a collapsed table.
- A row checkbox is visually checked, but the row itself lacks the selected outline. Checked
  checkbox state and `row-selected` state must be synchronized; add
  `tr:has(input[type="checkbox"]:checked)` selected CSS as a fallback.
- Table resource icons have added gray tile/card backgrounds.
- Status labels beside status dots are muted or regular weight instead of semibold primary
  text.
- Row `More` action is clipped, overlapped, or pushed against the table edge.
- Row `More` action is compressed into a square `32px x 32px` icon button instead of a
  `32px` high text button with `0 20px` horizontal padding.
- Row `More` action cell removes the normal `0 12px` table cell padding or is not fixed at
  `80px` column width.
- Row action dropdown uses the default centered/left `bottom` placement instead of
  `placement="bottom-end"`, so the menu panel's right edge does not align with the More
  button's right edge.
- Row action dropdown sets `placement="bottom-end"` but the inner `.ks-row-menu` keeps a
  wider default width and overflows the aligned tippy panel.
- Table row operation menu uses error/danger text color for Delete/删除 instead of normal
  menu text color.
- Pagination uses numbered page buttons instead of the attached KubeSphere layout.
- Pagination uses old Chinese copy such as `每页行数` or `共 9 条记录` instead of `每页显示`
  and `总数：9`.
- Pagination page-size control uses `20px` horizontal padding instead of `8px`.
- Pagination page-size dropdown visual panel outer width is wider than `96px` for the four
  numeric options.
- Pagination page-size dropdown option backgrounds have uneven left/right side margins.
  The `96px` panel should use `0` popover content padding, a `96px` menu with compact
  `8px` side padding, `80px` options, and no extra option side margin.
- Pagination page-size or previous/next controls use pill radius instead of `4px` radius.
- Pagination previous/next buttons are not exact `32px x 32px` controls with centered icons.
- Pagination page-size label, value, and chevron are glued together, vertically offset, or
  not aligned through the kube-design Button inner wrappers.
- Pagination previous/next buttons are clipped, flush against the right edge, or hidden by
  list-surface overflow.
- UI labels mix Chinese and English or any visible text is below `12px`.
- Shell/custom text uses an AI-template font stack such as `Inter`, `Geist`, or
  `system-ui` before the kube-design/KubeSphere font stack.
- A normal list page includes a runtime language switcher. Choose one locale from the prompt
  and render only that locale.
- The page adds status summary cards, language switchers, toast systems, delete modals, or
  column-setting modals when the prompt only asked for a basic resource list page. Use the
  compact Cogwheel dropdown for column visibility instead.
- Search input has a fixed narrow `max-width` that prevents it from filling the toolbar.
- Search input grows outside the center search zone and overlaps the Select filter or
  right action buttons.
- `.ks-filter-input` has a positive fixed `min-width` such as `220px` instead of
  `min-width: 0`, so it can force toolbar overlap.
- FilterInput search text is bold or primary-colored instead of regular secondary text.
- Verification treats `.kubed-select-selection-search-input` as the toolbar search field.
  That input belongs to `Select`; verify the real `FilterInput` by placeholder or
  `.filter-input`.
- The page installs `@kubed/components` but uses raw HTML controls for toolbar, table
  selection, row actions, or pagination.
- The page uses `@kubed/components` without a root `KubedConfigProvider`.
- The generated project contains private paths, local test names, screenshots, internal
  repository names, credentials, process notes, or evaluation prompts.

## Generation Rule

For resource list pages, select a resource recipe before generating code. The resource
recipe determines page title, active menu, icon, columns, mock data shape, and localized
labels. Do not copy Deployments columns into Pods, Services, Ingresses, or Gateway pages
unless the recipe says so.

For Phase 1 list pages, keep behavior intentionally narrow: shell, toolbar, table, row
actions, and pagination. Do not add dashboards, stat cards, locale toggles, notifications,
modal workflows, animated loading states, or large column-management screens unless the
user explicitly requests them. The compact Cogwheel column dropdown is part of the basic
list toolbar.

For detail pages, create flows, dashboards, or other unsupported page types, preserve the
same Header, Sidebar, Page Header, typography, language, icon, and button contracts. Keep
the non-list content conservative and state any unsupported pattern as an approximation
instead of inventing a new console style.

## Runtime Verification Checklist

When the environment supports running the page, verify before finishing:

- Build/typecheck passes.
- Browser opens the page and `#root` contains the console UI.
- No current console errors after reload.
- Official logo image is present.
- Header is `64px` high and Sidebar is `220px` wide on desktop.
- Header quick-nav icons use the correct active/inactive duotone colors.
- Inactive Workspace icon is measured visually as a two-channel duotone icon, not a black
  glyph.
- Sidebar scope selector has no leading icon and sidebar labels do not truncate unexpectedly.
- Sidebar resource nav begins `12px` below the scope selector.
- Sidebar menu labels have computed `font-weight: 500` for parent, expanded parent, and
  active child rows.
- Profile text stack is left-aligned: username and role/subtitle share the same left edge.
- Expanded parent sidebar row with an active child uses active brand text color.
- Toolbar order is Select/filter, growing search, Refresh, Cogwheel, dark primary action.
- Toolbar layout has three sibling zones: filters, search, actions. Search stays inside the
  center zone; actions are not inside or underneath the FilterInput.
- Toolbar Select filter shows the kube-design dropdown arrow.
- Toolbar Select arrow remains visible after layout styling; use explicit `ChevronDown`
  suffix if package defaults do not render it.
- Toolbar Select root and `.kubed-select-selector` are both stable `240px x 32px`; selector
  uses `box-sizing: border-box` and does not collide with the search field.
- Cogwheel opens the compact DataTable settings menu. It uses
  `Dropdown placement="bottom-end" maxWidth={160}` with `Menu width={160}
  className="menu-setting"`, `MenuLabel`, and `MenuItem` rows. The panel is dark, `4px`
  radius, `padding: 4px 0`, shadowed, and uses title `定制内容` / `Custom Columns`. Rows use
  `padding: 6px 12px`, regular `400` text, dark-hover background, `Eye size={16}` for
  visible columns, and `EyeClosed size={16}` for hidden columns. Its content structure uses
  `.menu-setting`, `.menu-label`, `.item-inner`, `.item-icon`, `.item-body`, and
  `.item-label`; generated `sc-*` hash classes are not stable and should not be copied.
  Toggling a normal data column hides and shows its header and body cells, while the first
  `Name` / `名称` data column and final operation column remain visible.
- Cogwheel trigger behavior works both ways: first click opens the column menu and a second
  click on the Cogwheel closes it. The generated code must not set `hideOnClick={false}` on
  the column-settings Dropdown.
- Refresh/Cogwheel are `32px` high text buttons with `0 20px` horizontal padding.
- Refresh/Cogwheel are separate right action buttons after the FilterInput, not search
  suffix icons or input adornments.
- Refresh/Cogwheel and row More are `32px` high text buttons with `0 20px` horizontal
  padding and no shadow; pagination arrows are exact `32px x 32px` text buttons with `0`
  padding and `4px` radius.
- Dark create/action button uses `Button variant="filled" color="secondary" radius="xl"
  size="sm" shadow`, and computed `box-shadow` is not `none`.
- Row More action cell keeps `12px` left/right cell padding and fixed `80px` column width.
- Checkbox/select column is fixed at `40px`; row More action column is fixed at `80px`.
- Row action dropdown uses `Dropdown placement="bottom-end"` with compact `maxWidth`
  around `160px`, the inner `.ks-row-menu` is constrained to the same `160px` width, and
  the visible menu right edge aligns with the More trigger right edge.
- Ordinary single-line table body cells use primary text color and regular `400`; only
  Object Identity descriptions/helper lines use secondary text color.
- Row operation menu items, including Delete/删除, use normal menu text color.
- FilterInput search text is regular weight and secondary color.
- FilterInput wrapper is `32px` high, `width: 100%`, `min-width: 0`, `max-width: none`, and
  stays inside `.ks-toolbar-search`.
- When inspecting toolbar search, ignore kube-design `Select` internals such as
  `.kubed-select-selection-search-input`; verify the real `FilterInput` input by
  placeholder or `.filter-input`.
- Table has `8-10` visible rows by default unless the user asked for fewer.
- Table uses the source-like collapsed model: computed `border-collapse: collapse`; header
  cells use `16px 12px` padding; body cells use `8px 12px` padding and `border-top:
  1px solid #eff4f9`.
- Basic demo table starts with no selected rows and supports selecting/unselecting rows by
  clicking table checkboxes.
- Table normal row hover uses subtle pale blue-gray cells and does not disturb selected-row
  outline.
- Selected rows use real green cell borders: selected cells get green top borders, the
  first selected cell gets a green left border, the last selected cell gets a green right
  border, and the last selected row closes with a green bottom border. Consecutive selected
  rows avoid double green borders.
- Checked row checkboxes and selected row outline are synchronized; no checked checkbox is
  shown without a selected row frame.
- Pagination footer wrapper uses source-like `padding: 10px 20px`, `background: #f9fbfd`,
  bottom radius `4px`, and muted base text color.
- Pagination is attached and uses page-size, total, previous, `1 / N`, next.
- Pagination Chinese copy is `每页显示` and `总数：N`; English copy is `Rows per page` and
  `Total: N`. Default page size is `10`, with options `10`, `20`, `50`, `100`.
- Pagination page-size and previous/next controls use `4px` radius, and the rightmost
  button is fully visible with no clipping.
- Pagination page-size control has `5px 8px` padding. Previous/next controls are exactly
  `32px x 32px` with centered icons and no shadow.
- Pagination page-size dropdown visual panel outer width is `96px`.
- Pagination page-size dropdown has no double or overly wide side spacing: `96px` outer
  panel, `0` popover content padding, `96px` menu, compact `8px` menu side padding,
  `80px` options, and no extra option side margin.
- Pagination page-size Button splits label/value/icon into separate children; computed
  inner label wrapper is inline-flex/center with about `4px` gap.
- UI uses one locale only and no visible text is smaller than `12px`.
- Body and `.ks-console` computed font family follows the kube-design/KubeSphere stack, not
  `Inter` or `Geist` first.

## Visible Control Rule

For a buildable React page, inspect the final source before finishing. A basic list page
should include imports from both `@kubed/icons` and `@kubed/components`. Raw semantic
elements are acceptable for product shell wrappers such as `header`, `aside`, `main`, and
simple navigation rows, but not for kube-design control roles. Toolbar buttons, command
buttons, selection checkboxes, row action menus, and pagination icon actions must use
kube-design components.

## Generated Code Self-Check

Before finishing a generated or repaired list page, inspect the final source for these
high-frequency omissions:

- `KubedConfigProvider` wraps the app.
- The primary create/action button is a real kube-design `Button` with `color="secondary"`
  and `shadow`.
- Refresh, Cogwheel, row More, page-size, previous, and next controls are real kube-design
  `Button variant="text"` controls, not raw buttons.
- Refresh/Cogwheel/Create are siblings of the search zone, not children of `.ks-filter-input`
  or absolutely positioned inside the search field.
- Cogwheel is wrapped in kube-design `Dropdown/Menu/MenuItem` column visibility controls;
  do not leave it as a nonfunctional decorative settings icon.
- Column-settings `Dropdown` does not set `hideOnClick={false}` or custom state that
  prevents clicking the Cogwheel trigger again to close the menu.
- Column visibility UI uses the `160px` DataTable settings menu with `Eye` / `EyeClosed`
  rows, not Checkbox rows, blue highlight panels, native checkbox lists, or a modal.
- Column visibility popover uses the stable semantic structure `.menu-setting`,
  `.menu-label`, `.item-inner`, `.item-icon`, `.item-body`, and `.item-label`; do not
  preserve generated `sc-*` class names from inspected HTML.
- Column settings never include the Object Identity `Name` / `名称` column or the row
  operation column; they are fixed anchors of the table.
- Refresh/Cogwheel/row More have no shadow; create/action has shadow.
- Row menu `Delete` / `删除` is a normal `MenuItem` without danger/error text styling.
- Row action `Dropdown` sets `placement="bottom-end"` and does not rely on the default
  `bottom` placement. Its inner `.ks-row-menu` is also constrained to `160px`; do not let
  the menu overflow the aligned tippy panel.
- Profile trigger text stack uses left alignment for both username and role/subtitle.
- Sidebar rows and children use `font-weight: 500`; do not make active sidebar text
  semibold/bold.
- Table row hover is explicitly defined for non-selected rows, and selected-row CSS also
  covers `tr.row-selected:hover td`.
- Table CSS uses `border-collapse: collapse`, header `padding: 16px 12px`, body
  `padding: 8px 12px`, and body row separators via `border-top: 1px solid #eff4f9`.
- Selected-row CSS uses a source-like real-border full-row frame and includes a
  `tr:has(input[type="checkbox"]:checked)` fallback for checked rows.
- Page-size label, value, and chevron are separate children so CSS can align them with
  `4px` inner gap.
- Page-size label/total use current copy: `每页显示` / `总数：N` or `Rows per page` /
  `Total: N`; page-size values are `10`, `20`, `50`, `100`.
- Page-size dropdown visual panel outer width is constrained to `96px`.
- Page-size menu options use a compact single side-spacing layer: `8px` menu padding and
  `80px` options inside the `96px` panel; do not add option margins.
- No local `icons.tsx`, local primitive clone, private path, test-project path, screenshot
  reference, or process note remains in the output.
