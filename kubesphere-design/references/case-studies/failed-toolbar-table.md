---
status: active
---

# Failed Toolbar And Table

## Wrong Output

Toolbar is split into a separate card, search is a generic bordered input, Refresh/Cogwheel
collapse into square icon-only tiles or become oversized, create is green or has a plus
icon, create omits the kube-design button shadow, table body is flush to the card, and
pagination uses numbered page buttons.

## Why It Fails

KubeSphere list pages use one integrated surface. Toolbar, table, and pagination are a
single compact operational system.

## Correct Pattern

- Use the toolbar recipe from `assets/recipes.md`.
- Keep Refresh and Cogwheel as `32px` high text buttons with `0 20px` horizontal padding
  and a centered `16px` icon, even though the visible label is aria-only.
- Use a dark kube-design primary action with explicit `shadow`.
- Keep table main inset `0 12px 12px`.
- Use Object Identity Pattern in the first data column.
- Use attached pagination: page size, total, previous, `1 / N`, next.

## Checklist

- Search uses real kube-design `FilterInput` when exported and grows.
- Refresh and Cogwheel are borderless `32px` high text buttons with horizontal padding at
  rest.
- Create is dark, text-only by default, and has explicit kube-design `shadow`.
- Table header is white.
- Selected row uses pale cells plus thin green outline.
