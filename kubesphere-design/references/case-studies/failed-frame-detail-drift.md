---
status: active
failure: frame-detail-drift
---

# Failed Case: Frame Details Drift After Using Kube-Design

## Wrong

The page imports `@kubed/components` and `@kubed/icons`, but product-frame details drift:

- inactive Workspace/Cluster top icon becomes a black silhouette or transparent-fill glyph;
- Component Dock is restyled as a custom bordered chip;
- profile is wrapped in a large gray bordered pill;
- scope selector adds a leading Cluster/Workspace icon or feels like active navigation;
- expanded sidebar parent row has strong outlined/pill treatment;
- kube-design `Select` receives native select CSS and a data-URI chevron;
- table identity icons sit on gray tiles/cards;
- last-column `More` action is clipped or overlaps the table edge.

## Why It Fails

This is a common mid-stage failure: the agent uses the correct package imports, then
overrides too much of the product chrome with custom CSS. The result is closer than a
generic admin page, but still not recognizable enough as the KubeSphere console.

## Correct

- Keep inactive top management entries as `36px` light circular quick-nav controls with
  explicit `color="#36435c"` and `fill="#b6c2cd"`. Do not rely on package defaults for
  Workspace, and do not pass `fill="transparent"`.
- Render Component Dock as a `36px` no-border light pill with `Grid2Duotone`
  `color="#324558"` and `fill="#b6c2cd"`, `6px` icon gap, semibold label, and no custom
  border.
- Render profile as a compact transparent avatar + username/role + chevron trigger without
  a heavy container.
- Keep the scope selector as a light title/subtitle field with right chevron only, and keep
  expanded sidebar parents as quiet rows.
- Let `Select` render its own control. Style layout width only.
- Use a plain `40px` object identity icon slot with no background tile.
- Reserve enough action-cell width for the `More` text button and dropdown trigger.

## Checklist

- Top inactive Workspace/Cluster icon has two visible duotone channels, not black.
- Component Dock looks like the product light pill, not a handmade bordered chip.
- Profile does not have a prominent bordered gray pill background.
- Scope selector has no leading resource icon; expanded parent row is quiet, light, and
  unboxed.
- No `appearance: none` or `background-image` chevron is applied to kube-design `Select`.
- `.ks-object-icon` has no gray background/border.
- `.ks-table-main` uses `margin: 0 12px 12px`.
- Row action cell does not clip the `More` button.
