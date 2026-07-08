# Resource List Page Shell

This is a structural page shell for KubeSphere Enterprise resource list pages. Adapt resource
labels, columns, icons, and data from `assets/recipes.md` and `assets/icon-map.md`.

Use this file to preserve product frame order and class hooks. Do not treat it as permission
to create a local design system. `Button`, `Checkbox`, `Dropdown`, `Menu`, `MenuItem`, and
all icons must come from public kube-design packages whenever the task is expected to build
a real React page.

Forbidden substitutions:

- `import { Button, Checkbox, Dropdown } from './components'`
- `import { Cluster, PodDuotone } from './icons'`
- raw `button`, `select`, CSS-only checkbox, or custom dropdown replacements for list
  controls
- hand-written SVG icons that imitate `@kubed/icons`
- local component clones created only to avoid installing `@kubed/components`

## Imports And Configuration

```tsx
import type { ReactNode } from 'react';
import { Button, Checkbox, Dropdown, FilterInput, KubedConfigProvider, Menu, MenuItem, Select } from '@kubed/components';
import {
  Appcenter,
  Backup,
  ChevronDown,
  Cluster,
  Cogwheel,
  Grid2Duotone,
  More,
  Next,
  Previous,
  Refresh,
  Workspace,
} from '@kubed/icons';

const LOGO_URL = 'https://ks-com-cn-staging.pek3b.qingstor.com/images/logo.svg';

type Locale = 'en' | 'zh';
type ManagementView = 'clusters' | 'workspaces';
type ResourceStatus = 'running' | 'updating' | 'error' | 'waiting';

const resourceConfig = {
  activeKey: 'deployments',
  titleKey: 'deployments',
  identityIcon: Backup,
  primaryColumn: 'image',
  secondaryColumn: 'pods',
} as const;

const rows: Array<{
  name: string;
  description: string;
  status: ResourceStatus;
  project: string;
  image: string;
  pods: string;
  updatedTime: string;
  selected?: boolean;
}> = [
  {
    name: 'nginx-frontend',
    description: 'default',
    status: 'running',
    project: 'default',
    image: 'nginx:1.21',
    pods: '3/3',
    updatedTime: '2 hours ago',
  },
  {
    name: 'api-gateway',
    description: 'production',
    status: 'running',
    project: 'production',
    image: 'kong:2.8',
    pods: '2/2',
    updatedTime: '1 day ago',
    selected: true,
  },
  {
    name: 'redis-master',
    description: 'production',
    status: 'updating',
    project: 'production',
    image: 'redis:alpine',
    pods: '1/2',
    updatedTime: '10 minutes ago',
  },
];
```

The row array above is abbreviated to keep the shell readable. Generated pages should use
`8-10` visible mock rows by default unless the prompt asks for fewer rows.

## Root Provider

Wrap standalone pages with kube-design's provider. Choose one locale and do not add a
runtime language switcher for a basic list page.

```tsx
ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <KubedConfigProvider locale="zh" themeType="light">
      <ResourceListPage locale="zh" />
    </KubedConfigProvider>
  </React.StrictMode>,
);
```

## Page Structure

Keep this structure narrow for a basic list page. Do not add status cards, dashboard
summaries, locale switchers, toast systems, modal workflows, animated loading screens, or
column-management dialogs unless the user explicitly asks for those features.

```tsx
export function ResourceListPage({ locale = 'zh' }: { locale?: Locale }) {
  const text = dictionary[locale];

  return (
    <div className="ks-console">
      <ConsoleHeader locale={locale} activeView="clusters" />
      <div className="ks-console-body">
        <ResourceSidebar locale={locale} activeView="clusters" activeKey={resourceConfig.activeKey} />
        <main className="ks-main">
          <PageHeader title={text[resourceConfig.titleKey]} />
          <section className="ks-content">
            <div className="ks-list-surface">
              <ListToolbar locale={locale} />
              <div className="ks-table-main">
                <ResourceTable locale={locale} />
              </div>
              <TablePagination locale={locale} total={rows.length} page={1} pages={1} />
            </div>
          </section>
        </main>
      </div>
    </div>
  );
}
```

## Header Shell

```tsx
function ConsoleHeader({ locale, activeView }: { locale: Locale; activeView: ManagementView }) {
  const text = dictionary[locale];
  const getTopIconProps = (active: boolean) =>
    active
      ? { color: 'rgba(255,255,255,0.9)', fill: '#ffffff66' }
      : { color: '#36435c', fill: '#b6c2cd' };

  return (
    <header className="ks-header">
      <div className="ks-header-brand">
        <img src={LOGO_URL} alt="KubeSphere" />
      </div>
      <nav className="ks-management-nav" aria-label={text.managementViews}>
        <ManagementEntry
          label={text.clusterManagement}
          active={activeView === 'clusters'}
          icon={
            <Cluster
              size={24}
              {...getTopIconProps(activeView === 'clusters')}
            />
          }
        />
        <ManagementEntry
          label={text.workspaceManagement}
          active={activeView === 'workspaces'}
          icon={
            <Workspace
              size={24}
              {...getTopIconProps(activeView === 'workspaces')}
            />
          }
        />
      </nav>
      <div className="ks-header-spacer" />
      <button className="ks-component-dock" type="button">
        <Grid2Duotone size={20} color="#324558" fill="#b6c2cd" />
        {text.componentDock}
      </button>
      <span className="ks-header-divider" />
      <button className="ks-profile-trigger" type="button">
        <span className="ks-avatar">A</span>
        <span className="ks-profile-text">
          <strong>admin</strong>
          <span>{text.adminRole}</span>
        </span>
        <ChevronDown size={16} />
      </button>
    </header>
  );
}

function ManagementEntry({ label, active, icon }: { label: string; active: boolean; icon: ReactNode }) {
  return (
    <button className={active ? 'ks-management-entry is-active' : 'ks-management-entry'} type="button" aria-label={label} title={label}>
      {icon}
      {active && <span className="ks-management-indicator" />}
    </button>
  );
}
```

## Sidebar Shell

```tsx
const workloadChildKeys = ['deployments', 'statefulsets', 'daemonsets', 'jobs', 'cronjobs', 'pods'] as const;

function ResourceSidebar({ locale, activeView, activeKey }: { locale: Locale; activeView: ManagementView; activeKey: string }) {
  const text = dictionary[locale];
  const scopeSubtitle = activeView === 'clusters' ? text.cluster : text.workspace;

  return (
    <aside className="ks-sidebar">
      <button className="ks-scope-selector" type="button">
        <span>
          <strong>{activeView === 'clusters' ? 'demo-cluster' : 'default-workspace'}</strong>
          <small>{scopeSubtitle}</small>
        </span>
        <ChevronDown size={16} />
      </button>
      <nav className="ks-resource-nav">
        <button className="ks-nav-row" type="button">
          <Cluster size={20} />
          <span>{text.clusterStatus}</span>
        </button>
        <button className="ks-nav-row is-open" type="button">
          <Appcenter size={20} color="#00aa72" fill="#90e0c5" />
          <span>{text.workloads}</span>
          <ChevronDown size={16} />
        </button>
        <div className="ks-nav-children">
          {workloadChildKeys.map((key) => (
            <button key={key} className={activeKey === key ? 'ks-nav-child is-active' : 'ks-nav-child'} type="button">
              {text[key]}
            </button>
          ))}
        </div>
      </nav>
    </aside>
  );
}
```

## Toolbar, Table, Pagination Shell

```tsx
function PageHeader({ title }: { title: string }) {
  return <div className="ks-page-header"><h1>{title}</h1></div>;
}

function ListToolbar({ locale }: { locale: Locale }) {
  const text = dictionary[locale];

  return (
    <div className="ks-list-toolbar">
      <Select
        className="ks-project-filter"
        aria-label={text.project}
        value="all"
        showArrow
        suffixIcon={<ChevronDown size={16} color="#324558" fill="#b6c2cd" />}
      >
        <Select.Option value="all">{text.allProjects}</Select.Option>
      </Select>
      <FilterInput className="ks-filter-input" simpleMode placeholder={text.searchPlaceholder} />
      <Button className="ks-toolbar-icon-button" variant="text" aria-label={text.refresh}><Refresh size={16} /></Button>
      <Button className="ks-toolbar-icon-button" variant="text" aria-label={text.tableSettings}><Cogwheel size={16} /></Button>
      <Button className="ks-command-button" variant="filled" color="secondary" radius="xl" size="sm" shadow>{text.create}</Button>
    </div>
  );
}

function ResourceTable({ locale }: { locale: Locale }) {
  const text = dictionary[locale];
  const IdentityIcon = resourceConfig.identityIcon;

  return (
    <table className="ks-resource-table">
      <thead>
        <tr>
          <th className="ks-select-cell"><Checkbox /></th>
          <th>{text.name}</th>
          <th>{text.status}</th>
          <th>{text.project}</th>
          <th>{text[resourceConfig.primaryColumn]}</th>
          <th>{text[resourceConfig.secondaryColumn]}</th>
          <th>{text.updatedTime}</th>
          <th className="ks-action-cell" />
        </tr>
      </thead>
      <tbody>
        {rows.map((row) => (
          <tr key={row.name} className={row.selected ? 'row-selected' : undefined}>
            <td className="ks-select-cell"><Checkbox checked={row.selected} /></td>
            <td>
              <div className="ks-object-identity">
                <span className="ks-object-icon" aria-hidden="true"><IdentityIcon size={40} /></span>
                <span><strong>{row.name}</strong><small>{row.description}</small></span>
              </div>
            </td>
            <td><span className={`ks-status-dot is-${row.status}`} /><span className="ks-status-text">{text[row.status]}</span></td>
            <td>{row.project}</td>
            <td>{row.image}</td>
            <td>{row.pods}</td>
            <td className="ks-muted">{row.updatedTime}</td>
            <td className="ks-action-cell">
              <Dropdown content={<RowMenu locale={locale} />}>
                <Button className="ks-row-more" variant="text" radius="xl" size="sm" aria-label={text.more}><More size={16} /></Button>
              </Dropdown>
            </td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}

function RowMenu({ locale }: { locale: Locale }) {
  const text = dictionary[locale];

  return (
    <Menu className="ks-row-menu">
      <MenuItem>{text.edit}</MenuItem>
      <MenuItem>{text.viewYaml}</MenuItem>
      <MenuItem>{text.delete}</MenuItem>
    </Menu>
  );
}

function TablePagination({ locale, total, page, pages }: { locale: Locale; total: number; page: number; pages: number }) {
  const text = dictionary[locale];

  return (
    <footer className="ks-pagination">
      <div className="ks-pagination-left">
        <Button className="ks-page-size" variant="text" radius="sm">
          <span>{text.perPage}</span>
          <span>10</span>
          <ChevronDown size={14} />
        </Button>
        <span className="ks-pagination-divider" />
        <span className="ks-muted">{text.total(total)}</span>
      </div>
      <div className="ks-pagination-right">
        <Button className="ks-pagination-icon" variant="text" radius="sm" aria-label={text.previous}><Previous size={20} /></Button>
        <strong>{page} / {pages}</strong>
        <Button className="ks-pagination-icon" variant="text" radius="sm" aria-label={text.next}><Next size={20} /></Button>
      </div>
    </footer>
  );
}
```

## CSS Anchor

Use the consuming project's styling system. These class names and values are the shell
anchor; move them into Emotion, CSS Modules, or global CSS as appropriate.

```css
html, body, #root { min-height: 100%; margin: 0; }
body { color: var(--ks-text); background: var(--ks-bg); font-family: var(--ks-font-sans); font-size: 12px; line-height: var(--ks-base-line-height); -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; text-rendering: optimizeLegibility; }
button, input, textarea, select { font: inherit; }
.ks-console { min-height: 100vh; color: var(--ks-text); background: var(--ks-bg); font-family: var(--ks-font-sans); font-size: 12px; line-height: var(--ks-base-line-height); }
.ks-header { height: 64px; display: flex; align-items: center; background: #fff; padding: 0 20px; border-bottom: 1px solid var(--ks-border); }
.ks-header-brand { width: 224px; flex-shrink: 0; }
.ks-header-brand img { display: block; width: 180px; height: auto; }
.ks-management-nav { height: 64px; display: flex; align-items: center; }
.ks-management-entry { position: relative; width: 36px; height: 36px; display: inline-flex; align-items: center; justify-content: center; border: 0; border-radius: 50%; background: var(--ks-subtle); margin-right: 20px; }
.ks-management-entry.is-active { border-radius: 8px; background: var(--ks-text); box-shadow: 0 4px 8px rgba(36, 46, 66, 0.24); }
.ks-management-indicator { position: absolute; left: 4px; bottom: -14px; width: 28px; height: 4px; border-radius: 2px; background: var(--ks-text); }
.ks-header-spacer { flex: 1; }
.ks-component-dock { min-width: 84px; height: 36px; display: inline-flex; align-items: center; justify-content: center; gap: 6px; border: 0; border-radius: 18px; background: var(--ks-subtle); padding: 0 16px; color: var(--ks-text); font-size: 12px; font-weight: 600; white-space: nowrap; }
.ks-component-dock:hover { background: #eff4f9; border-radius: 8px; }
.ks-header-divider { width: 1px; height: 20px; margin: 0 12px; background: var(--ks-border); }
.ks-profile-trigger { height: 36px; display: inline-flex; align-items: center; gap: 12px; border: 0; background: transparent; padding: 0; color: var(--ks-text); font-size: 12px; font-weight: 600; }
.ks-avatar { width: 32px; height: 32px; display: inline-flex; align-items: center; justify-content: center; border-radius: 50%; background: var(--ks-bg); }
.ks-profile-text, .ks-scope-selector span, .ks-object-identity span { display: flex; flex-direction: column; min-width: 0; }
.ks-profile-text span, .ks-muted, .ks-scope-selector small, .ks-object-identity small { color: var(--ks-muted); font-size: 12px; }
.ks-console-body { display: flex; min-height: calc(100vh - 64px); }
.ks-sidebar { width: 220px; flex: 0 0 220px; padding: 16px 12px 12px; background: #fff; border-right: 1px solid var(--ks-border); }
.ks-scope-selector { width: 196px; height: 68px; display: grid; grid-template-columns: minmax(0, 1fr) 16px; align-items: center; gap: 4px; padding: 12px; border: 1px solid var(--ks-border-strong); border-radius: 4px; background: linear-gradient(359.02deg, #f9fbfd 0.81%, #ffffff 99.13%); text-align: left; }
.ks-resource-nav { margin-top: 24px; }
.ks-nav-row { width: 100%; height: 36px; display: grid; grid-template-columns: 20px 1fr 16px; align-items: center; gap: 8px; border: 0; background: transparent; color: var(--ks-nav-text); font-size: 12px; text-align: left; }
.ks-nav-row.is-open { color: var(--ks-active); font-weight: 600; }
.ks-nav-row.is-open svg { color: var(--ks-icon-active); }
.ks-nav-child { width: 100%; height: 36px; display: block; padding: 0 0 0 44px; border: 0; background: transparent; color: var(--ks-nav-text); font-size: 12px; text-align: left; }
.ks-nav-child.is-active { color: var(--ks-active); font-weight: 600; }
.ks-main { flex: 1; min-width: 0; background: var(--ks-bg); }
.ks-page-header { height: 56px; display: flex; align-items: center; padding: 0 20px; background: #fff; border-bottom: 1px solid var(--ks-border); }
.ks-page-header h1 { margin: 0; font-size: 18px; line-height: 24px; font-weight: 600; }
.ks-content { padding: 20px; }
.ks-list-surface { overflow: visible; background: #fff; border-radius: 4px; box-shadow: 0 2px 4px rgba(36, 46, 66, 0.04); }
.ks-list-toolbar { height: 56px; display: grid; grid-template-columns: 148px minmax(280px, 1fr) auto auto auto; align-items: center; gap: 12px; padding: 10px 20px; background: var(--ks-subtle); border-bottom: 1px solid var(--ks-border); }
.ks-project-filter, .ks-filter-input, .ks-command-button { height: 32px; font-size: 12px; }
.ks-project-filter { min-width: 140px; width: 148px; }
.ks-project-filter .kubed-select-selector { padding-right: 34px; }
.ks-project-filter .kubed-select-arrow { display: inline-flex !important; align-items: center; justify-content: center; right: 10px; opacity: 1 !important; visibility: visible !important; pointer-events: none; color: #324558; }
.ks-project-filter .kubed-select-arrow svg { color: #324558; fill: #b6c2cd; }
/* Let kube-design Select render its own border, chevron, and option styling. */
.ks-filter-input { width: 100%; min-width: 220px; }
.ks-filter-input input.filter-input { color: var(--ks-muted) !important; font-weight: 400 !important; }
.ks-filter-input input.filter-input::placeholder { color: var(--ks-muted) !important; opacity: 1; font-weight: 400 !important; }
/* Use public kube-design FilterInput. Do not hand-write label + input when FilterInput is exported. Do not set a fixed narrow max-width on `.ks-filter-input`; it should grow through the toolbar. Do not target `.kubed-select-selection-search-input`; that belongs to Select. */
.ks-toolbar-icon-button { width: auto; min-width: 56px; height: 32px; min-height: 32px; max-height: 32px; display: inline-flex; align-items: center; justify-content: center; border: 0; border-radius: 16px; background: transparent; box-shadow: none; color: #36435c; padding: 0 20px; }
.ks-toolbar-icon-button > * { width: auto; min-width: 16px; height: 32px; min-height: 32px; display: inline-flex; align-items: center; justify-content: center; padding: 0; }
.ks-row-more { width: auto; min-width: 56px; height: 32px; min-height: 32px; max-height: 32px; display: inline-flex; align-items: center; justify-content: center; border: 0; border-radius: 16px; background: transparent; box-shadow: none; color: #36435c; padding: 0 20px; }
.ks-row-more > * { width: auto; min-width: 16px; height: 32px; min-height: 32px; display: inline-flex; align-items: center; justify-content: center; padding: 0; }
.ks-pagination-icon { width: auto; min-width: auto; max-width: none; height: 32px; min-height: 32px; max-height: 32px; display: inline-flex; align-items: center; justify-content: center; border: 0; border-radius: 4px; background: transparent; box-shadow: none; color: #36435c; padding: 5px 12px; flex: 0 0 auto; overflow: visible; }
.ks-pagination-icon > * { width: auto; min-width: 20px; height: 20px; min-height: 20px; display: inline-flex; align-items: center; justify-content: center; padding: 0; }
.ks-toolbar-icon-button svg, .ks-row-more svg, .ks-pagination-icon svg { flex: 0 0 auto; }
.ks-toolbar-icon-button:hover, .ks-row-more:hover, .ks-pagination-icon:hover { background: var(--ks-bg); }
.ks-command-button { min-width: 88px; padding: 0 20px; border: 0; border-radius: 16px; background: var(--ks-text); color: #fff; font-weight: 600; }
.ks-table-main { margin: 0 12px 12px; }
.ks-resource-table { width: 100%; border-collapse: separate; border-spacing: 0; table-layout: fixed; }
.ks-resource-table th, .ks-resource-table td { height: 56px; border-bottom: 1px solid var(--ks-border); padding: 0 12px; text-align: left; vertical-align: middle; font-size: 12px; }
.ks-resource-table th { height: 44px; background: #fff; color: var(--ks-muted); font-weight: 600; }
.ks-select-cell { width: 44px; }
.ks-action-cell { width: 80px; padding: 0 12px; text-align: center; }
.ks-object-identity { display: grid; grid-template-columns: 40px minmax(0, 1fr); align-items: center; gap: 12px; }
.ks-object-icon { width: 40px; height: 40px; display: inline-flex; align-items: center; justify-content: center; background: transparent; border: 0; }
.ks-object-identity strong { color: var(--ks-text); font-weight: 700; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.ks-object-identity small { color: var(--ks-muted); font-weight: 400; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.ks-status-dot { width: 8px; height: 8px; display: inline-block; margin-right: 8px; border-radius: 50%; vertical-align: middle; }
.ks-status-text { color: var(--ks-text); font-weight: 600; }
.ks-status-dot.is-running { background: var(--ks-active); }
.ks-status-dot.is-updating { background: var(--ks-warning); }
.ks-status-dot.is-error { background: var(--ks-error); }
.ks-status-dot.is-waiting { background: #b6c2cd; }
.ks-resource-table tbody tr.row-selected td { background: var(--ks-bg); border-top: 1px solid var(--ks-active); border-bottom: 1px solid var(--ks-active); }
.ks-resource-table tbody tr.row-selected td:first-child { border-left: 1px solid var(--ks-active); }
.ks-resource-table tbody tr.row-selected td:last-child { border-right: 1px solid var(--ks-active); }
.ks-pagination { height: 52px; display: flex; align-items: center; justify-content: space-between; gap: 16px; padding: 0 20px; background: var(--ks-subtle); border-top: 1px solid var(--ks-border); overflow: visible; }
.ks-pagination-left, .ks-pagination-right, .ks-page-size { display: inline-flex; align-items: center; gap: 8px; }
.ks-pagination-left, .ks-pagination-right { min-width: 0; }
.ks-pagination-right { flex: 0 0 auto; padding-right: 0; }
.ks-page-size { height: 32px; display: inline-flex; align-items: center; border: 0; border-radius: 4px; background: transparent; color: var(--ks-text); font-size: 12px; font-weight: 600; line-height: 20px; padding: 5px 12px; }
.ks-page-size > div { display: inline-flex; align-items: center; height: 20px; line-height: 20px; }
.ks-page-size > div > span { display: inline-flex; align-items: center; gap: 4px; height: 20px; line-height: 20px; }
.ks-page-size > div > span > span, .ks-page-size > div > span > svg { display: inline-flex; align-items: center; line-height: 20px; }
.ks-page-size svg { flex: 0 0 auto; margin-top: 0; }
.ks-pagination-divider { width: 1px; height: 16px; background: var(--ks-border); }
```

## Locale Dictionary Pattern

```tsx
const dictionary = {
  zh: {
    managementViews: '管理视角',
    clusterManagement: '集群管理',
    workspaceManagement: '企业空间管理',
    componentDock: '组件坞',
    adminRole: '平台管理员',
    cluster: '集群',
    workspace: '企业空间',
    clusterStatus: '集群状态',
    workloads: '工作负载',
    deployments: '部署',
    statefulsets: '有状态副本集',
    daemonsets: '守护进程集',
    jobs: '任务',
    cronjobs: '定时任务',
    pods: '容器组',
    project: '项目',
    allProjects: '全部项目',
    searchPlaceholder: '搜索名称、IP 或节点...',
    refresh: '刷新',
    tableSettings: '表格设置',
    create: '创建',
    name: '名称',
    status: '状态',
    image: '镜像',
    updatedTime: '更新时间',
    running: '运行中',
    updating: '更新中',
    error: '异常',
    waiting: '等待中',
    more: '更多操作',
    edit: '编辑',
    viewYaml: '查看配置文件 (YAML)',
    delete: '删除',
    perPage: '每页行数:',
    previous: '上一页',
    next: '下一页',
    total: (count: number) => `共 ${count} 条记录`,
  },
  en: {
    managementViews: 'Management views',
    clusterManagement: 'Cluster Management',
    workspaceManagement: 'Workspace Management',
    componentDock: 'Component Dock',
    adminRole: 'Platform Admin',
    cluster: 'Cluster',
    workspace: 'Workspace',
    clusterStatus: 'Cluster Status',
    workloads: 'Workloads',
    deployments: 'Deployments',
    statefulsets: 'StatefulSets',
    daemonsets: 'DaemonSets',
    jobs: 'Jobs',
    cronjobs: 'CronJobs',
    pods: 'Pods',
    project: 'Project',
    allProjects: 'All Projects',
    searchPlaceholder: 'Search by name, IP, or node...',
    refresh: 'Refresh',
    tableSettings: 'Table Settings',
    create: 'Create',
    name: 'Name',
    status: 'Status',
    image: 'Image',
    updatedTime: 'Updated Time',
    running: 'Running',
    updating: 'Updating',
    error: 'Error',
    waiting: 'Waiting',
    more: 'More actions',
    edit: 'Edit',
    viewYaml: 'View YAML',
    delete: 'Delete',
    perPage: 'Rows per page:',
    previous: 'Previous page',
    next: 'Next page',
    total: (count: number) => `Total: ${count} items`,
  },
} as const;
```
