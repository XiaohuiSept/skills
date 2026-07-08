# Tokens

This is the visual value source of truth. Use these values before inventing colors,
spacing, typography, or row dimensions.

## Core Tokens

| Token | Value | Use |
|---|---:|---|
| `--ks-logo-url` | `https://ks-com-cn-staging.pek3b.qingstor.com/images/logo.svg` | Header logo |
| `--ks-font-sans` | `Roboto, "PingFang SC", "Lantinghei SC", "Helvetica Neue", Helvetica, Arial, "Microsoft YaHei", "微软雅黑", "STHeitiSC-Light", simsun, "宋体", "WenQuanYi Zen Hei", "WenQuanYi Micro Hei", sans-serif` | Console text |
| `--ks-header-height` | `64px` | Top navigation |
| `--ks-sidebar-width` | `220px` | Desktop sidebar |
| `--ks-page-header-height` | `56px` | Page title band |
| `--ks-content-padding` | `20px` | Main content padding |
| `--ks-control-height` | `32px` | Buttons, filters, search |
| `--ks-nav-row-height` | `36px` | Sidebar rows |
| `--ks-table-row-height` | `56px` | Table rows |
| `--ks-toolbar-icon-min-width` | `56px` | Toolbar Refresh/Cogwheel text buttons |
| `--ks-toolbar-icon-padding` | `0 20px` | Toolbar Refresh/Cogwheel text buttons |
| `--ks-pagination-icon-button-size` | `32px` | Pagination icon buttons |
| `--ks-min-font-size` | `12px` | Minimum visible text |
| `--ks-base-line-height` | `1.67` | Compact 12px text rhythm |

## Colors

| Token | Value | Use |
|---|---:|---|
| `--ks-bg` | `#eff4f9` | Workspace chrome |
| `--ks-surface` | `#ffffff` | Header, sidebar, table, surfaces |
| `--ks-subtle` | `#f9fbfd` | Toolbar, pagination, quiet controls |
| `--ks-hover` | `#eff4f9` | Hover and selected cell base |
| `--ks-border` | `#e3e9ef` | Dividers |
| `--ks-border-strong` | `#ccd3db` | Inputs, scope selector |
| `--ks-text` | `#242e42` | Primary text, command button |
| `--ks-muted` | `#79879c` | Secondary text |
| `--ks-nav-text` | `#4a5974` | Sidebar inactive text |
| `--ks-active` | `#55bc8a` | Active text, running dot |
| `--ks-icon-active` | `#00aa72` | Active sidebar icon primary |
| `--ks-icon-active-fill` | `#90e0c5` | Active sidebar icon fill |
| `--ks-top-icon` | `#36435c` | Inactive top Cluster/Workspace icon primary |
| `--ks-top-icon-fill` | `#b6c2cd` | Inactive top Cluster/Workspace icon fill |
| `--ks-top-icon-active` | `rgba(255,255,255,0.9)` | Active top Cluster/Workspace icon primary |
| `--ks-top-icon-active-fill` | `#ffffff66` | Active top Cluster/Workspace icon fill |
| `--ks-command-hover` | `#181d28` | Dark button hover |
| `--ks-warning` | `#f5a623` | Updating status |
| `--ks-error` | `#ca2621` | Error/destructive |

## Copyable CSS Anchor

```css
:root {
  --ks-logo-url: url("https://ks-com-cn-staging.pek3b.qingstor.com/images/logo.svg");
  --ks-font-sans: Roboto, "PingFang SC", "Lantinghei SC", "Helvetica Neue", Helvetica, Arial, "Microsoft YaHei", "微软雅黑", "STHeitiSC-Light", simsun, "宋体", "WenQuanYi Zen Hei", "WenQuanYi Micro Hei", sans-serif;
  --ks-header-height: 64px;
  --ks-sidebar-width: 220px;
  --ks-page-header-height: 56px;
  --ks-content-padding: 20px;
  --ks-control-height: 32px;
  --ks-nav-row-height: 36px;
  --ks-table-row-height: 56px;
  --ks-toolbar-icon-min-width: 56px;
  --ks-toolbar-icon-padding: 0 20px;
  --ks-pagination-icon-button-size: 32px;
  --ks-min-font-size: 12px;
  --ks-base-line-height: 1.67;

  --ks-bg: #eff4f9;
  --ks-surface: #ffffff;
  --ks-subtle: #f9fbfd;
  --ks-hover: #eff4f9;
  --ks-border: #e3e9ef;
  --ks-border-strong: #ccd3db;
  --ks-text: #242e42;
  --ks-muted: #79879c;
  --ks-nav-text: #4a5974;
  --ks-active: #55bc8a;
  --ks-icon-active: #00aa72;
  --ks-icon-active-fill: #90e0c5;
  --ks-top-icon: #36435c;
  --ks-top-icon-fill: #b6c2cd;
  --ks-top-icon-active: rgba(255,255,255,0.9);
  --ks-top-icon-active-fill: #ffffff66;
  --ks-command-hover: #181d28;
  --ks-warning: #f5a623;
  --ks-error: #ca2621;
}
```
