# Console Frame Reference

This is an explanation file. Use it to understand and verify the KubeSphere frame. Do not
invent shell layout beyond the page shell.

## Header

- Height: `64px`, white background, bottom border `#e3e9ef`.
- Brand area: `224px`, left logo image from the official URL.
- Normal desktop management entries are icon-only controls for Cluster and Workspace.
- Each management entry is a `36px` control. Inactive state is a light circular button
  (`#f9fbfd`) with the dark/default icon variant. Active state is a dark `#242e42`
  rounded-square button, light icon variant, shadow, and `28px x 4px` bottom indicator.
- Inactive Cluster/Workspace icons must explicitly set `color="#36435c"` and
  `fill="#b6c2cd"`. Do not pass only a muted `color` prop or make them black silhouettes.
- Active Cluster/Workspace icons use the light channels `color="rgba(255,255,255,0.9)"`
  and `fill="#ffffff66"` on the dark active button.
- Component Dock sits before profile and uses `Grid2Duotone` with explicit default
  duotone channels `color="#324558"` and `fill="#b6c2cd"` plus text `组件坞` or
  `Component Dock`.
- Component Dock is a no-border light pill: `height: 36px`, `min-width: 84px`,
  `padding: 0 16px`, background `#f9fbfd`, radius `18px`; hover background `#eff4f9`
  with radius `8px`. Use `Grid2Duotone` or the official grid asset, then a `6px` gap and
  semibold label. Do not substitute Cogwheel or a CSS-drawn grid.
- Profile shows a `32px` round avatar, username, role/subtitle, and chevron.
- Profile is a compact transparent account trigger, preferably named
  `.ks-profile-trigger`. Do not put the entire profile in a gray/bordered pill; keep the
  avatar, text stack, and chevron aligned with `12px` spacing.

## Sidebar

- Width: `220px`, white background, right border `#e3e9ef`, padding `16px 12px 12px`.
- Scoped selector is about `196px x 68px`, bordered, light, with title, subtitle, and a
  right-side chevron. In expanded sidebar state it does not show a leading Cluster or
  Workspace resource icon.
- Scope selector should look like a light field/card, not an active navigation item. Do not
  add a large leading icon, green active icon, or black-only scope glyph.
- Parent nav rows are `36px`, single-line, with semantic duotone icon.
- Child rows are text-only by default and indented.
- Active text is `#55bc8a`.
- Active parent icons set `color="#00aa72"` and `fill="#90e0c5"`.
- When a child menu item is active, its expanded parent row also uses active brand text
  color `#55bc8a`; it should not remain inactive gray.
- Expanded parent rows are still light rows. Do not draw a bordered outline, strong pill,
  or large active block around the expanded parent; only the parent label/icon state and
  child active text should carry emphasis.

## Page Header

- Height: `56px`, white background, bottom border.
- Title is `18px`, `600`, `#242e42`.
- No breadcrumb by default.
- No long descriptive subtitle by default.

## Frame Failures

Reject dark sidebars, fake logos, visible top management text labels beside desktop icons,
missing Component Dock, Cogwheel-as-Component-Dock, inactive Workspace/Cluster icons that
look black or single-channel, bordered custom Component Dock chips, oversized profile
pills, scope selectors with leading resource icons or active-nav styling, expanded parent
rows with strong outlined/pill styling, and child rows with default icons.
