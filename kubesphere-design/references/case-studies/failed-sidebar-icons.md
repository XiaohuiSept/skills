---
status: active
---

# Failed Sidebar Icons

## Wrong Output

Sidebar uses lucide icons, black custom blocks, CSS masks, single-color glyphs, or active
icons with only CSS text color applied.

## Why It Fails

KubeSphere navigation relies on semantic duotone `@kubed/icons`. Wrong icons immediately
make the console look generic.

## Correct Pattern

Use `assets/icon-map.md`. Active parent/sidebar icons must set both channels:

```tsx
<Appcenter size={20} color="#00aa72" fill="#90e0c5" />
```

Child rows are text-only by default.

## Checklist

- Icon component comes from `@kubed/icons`.
- Active duotone uses both `color` and `fill`.
- Child rows do not show icons by default.
- Expanded scope selector does not show a leading Cluster/Workspace icon.
