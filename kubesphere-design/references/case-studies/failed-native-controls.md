---
status: active
failure: installed-kubed-but-native-controls
---

# Failed Case: Installed Kube-Design But Used Native Controls

## Wrong

The project installs `@kubed/components` and `@kubed/icons`, but page code imports only
icons. Toolbar actions, the create button, table checkboxes, row menus, select filters, and
pagination actions are implemented with raw `button`, `select`, `input type="checkbox"`,
custom dropdown state, and custom CSS.

## Why It Fails

This creates a page that follows some class names and tokens but misses kube-design control
behavior: sizing, hover, active, focus, disabled, menu, and selection details drift from the
product UI. It also encourages every generated page to invent its own component behavior.

## Correct

Use real kube-design controls:

- `Button` for toolbar icons, command buttons, row more triggers, and pagination actions.
- `Checkbox` for table header and row selection.
- `Dropdown`, `Menu`, and `MenuItem` for row action menus.
- `Select` for project/resource filters when available.
- Real `FilterInput` for toolbar search when the package exports it; local wrapper only
  when the package does not export `FilterInput`.

Raw elements are allowed for structural layout wrappers and the table DOM, not for reusable
interactive controls that kube-design already provides.

## Checklist

- Final source imports from both `@kubed/components` and `@kubed/icons`.
- No custom checkbox CSS for table selection.
- No raw `select` for project filters when `Select` is available.
- No custom row-menu popover state when `Dropdown/Menu/MenuItem` is available.
- No runtime language switcher in a basic list page.
