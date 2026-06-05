# KubeSphere Implementation Blueprint

This file is a portable implementation blueprint for agents that need to generate a
KubeSphere Enterprise console-style page with public components and local composition.

Use it as a structural template, not as a new component library. Adapt imports, data,
labels, and styling conventions to the consuming project, but keep the frame hierarchy,
icon treatment, toolbar layout, table spacing, and pagination behavior.

Do not require downstream users to have private source repositories or private packages;
this file carries the reusable implementation shape.

## When To Use

Read this file when generating or substantially modifying a KubeSphere-style React console
page. Prefer an existing project shell only when it already matches the visual contract in
`DESIGN.md`. If the project has only public `@kubed/*` primitives, compose the shell locally
from this reference.

## Canonical Page Structure

```tsx
import type { ReactNode } from 'react';
import {
  Button,
  Checkbox,
  Dropdown,
  Menu,
  MenuItem,
} from '@kubed/components';
import {
  Appcenter,
  Backup,
  ChevronDown,
  Cluster,
  Cogwheel,
  Grid2Duotone,
  Magnifier,
  More,
  Next,
  Previous,
  Refresh,
  Workspace,
} from '@kubed/icons';

const LOGO_URL = 'https://ks-com-cn-staging.pek3b.qingstor.com/images/logo.svg';

type Locale = 'en' | 'zh';
type ManagementView = 'clusters' | 'workspaces';
type ResourceStatus = 'running' | 'updating' | 'error';

const resourceConfig = {
  activeKey: 'deployments',
  titleKey: 'deployments',
  identityIcon: Backup,
  primaryColumn: 'image',
  secondaryColumn: 'pods',
} as const;

const workloadChildKeys = [
  'deployments',
  'statefulsets',
  'daemonsets',
  'jobs',
  'cronjobs',
  'pods',
] as const;

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
    description: 'nginx-frontend description',
    status: 'running',
    project: 'production',
    image: 'nginx:1.21',
    pods: '3/3',
    updatedTime: '10 minutes ago',
  },
  {
    name: 'api-gateway',
    description: 'api-gateway description',
    status: 'running',
    project: 'production',
    image: 'kong:2.8',
    pods: '2/2',
    updatedTime: '2 hours ago',
    selected: true,
  },
  {
    name: 'redis-master',
    description: 'redis-master description',
    status: 'updating',
    project: 'production',
    image: 'redis:7.0',
    pods: '1/2',
    updatedTime: '5 minutes ago',
  },
  {
    name: 'auth-service',
    description: 'auth-service description',
    status: 'running',
    project: 'default',
    image: 'auth:v1.2.0',
    pods: '2/2',
    updatedTime: '3 days ago',
  },
  {
    name: 'payment-processor',
    description: 'payment-processor description',
    status: 'error',
    project: 'finance',
    image: 'payment:v2.1',
    pods: '0/1',
    updatedTime: '10 minutes ago',
  },
  {
    name: 'user-profile-db',
    description: 'user-profile-db description',
    status: 'running',
    project: 'production',
    image: 'postgres:13',
    pods: '1/1',
    updatedTime: '5 days ago',
  },
  {
    name: 'notification-worker',
    description: 'notification-worker description',
    status: 'running',
    project: 'default',
    image: 'worker:latest',
    pods: '4/4',
    updatedTime: '1 hour ago',
  },
  {
    name: 'search-engine',
    description: 'search-engine description',
    status: 'running',
    project: 'default',
    image: 'elasticsearch:7',
    pods: '3/3',
    updatedTime: '2 weeks ago',
  },
  {
    name: 'recommendation-api',
    description: 'recommendation-api description',
    status: 'updating',
    project: 'default',
    image: 'reco-api:v1.0',
    pods: '2/3',
    updatedTime: '2 minutes ago',
  },
  {
    name: 'inventory-sync',
    description: 'inventory-sync description',
    status: 'running',
    project: 'operations',
    image: 'inv-sync:stable',
    pods: '1/1',
    updatedTime: '4 hours ago',
  },
];

export function ResourceListPage({ locale = 'zh' }: { locale?: Locale }) {
  const text = dictionary[locale];

  return (
    <div className="ks-console">
      <ConsoleHeader locale={locale} activeView="clusters" />
      <div className="ks-console-body">
        <ResourceSidebar
          locale={locale}
          activeView="clusters"
          activeKey={resourceConfig.activeKey}
        />
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

## Header

Top management entries are icon-only controls in the normal desktop header. The visible
label belongs to `aria-label`, `title`, or a tooltip. Do not render visible text such as
`集群管理` or `企业空间` next to the icons.

```tsx
function ConsoleHeader({
  locale,
  activeView,
}: {
  locale: Locale;
  activeView: ManagementView;
}) {
  const text = dictionary[locale];

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
              color={activeView === 'clusters' ? 'rgba(255,255,255,0.9)' : undefined}
              fill={activeView === 'clusters' ? '#ffffff66' : undefined}
            />
          }
        />
        <ManagementEntry
          label={text.workspaceManagement}
          active={activeView === 'workspaces'}
          icon={
            <Workspace
              size={24}
              color={activeView === 'workspaces' ? 'rgba(255,255,255,0.9)' : undefined}
              fill={activeView === 'workspaces' ? '#ffffff66' : undefined}
            />
          }
        />
      </nav>

      <div className="ks-header-spacer" />

      <Button
        className="ks-component-dock"
        variant="filled"
        color="default"
        radius="xl"
        size="sm"
        leftIcon={<Grid2Duotone size={16} />}
      >
        {text.componentDock}
      </Button>

      <span className="ks-header-divider" />

      <button className="ks-profile" type="button">
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

function ManagementEntry({
  label,
  active,
  icon,
}: {
  label: string;
  active: boolean;
  icon: ReactNode;
}) {
  return (
    <button
      className={active ? 'ks-management-entry is-active' : 'ks-management-entry'}
      type="button"
      aria-label={label}
      title={label}
    >
      {icon}
      {active && <span className="ks-management-indicator" />}
    </button>
  );
}
```

## Sidebar

Parent nav rows may have icons. Child rows are text-only by default, including the active
child. Active parent icons must set both duotone channels.

```tsx
function ResourceSidebar({
  locale,
  activeView,
  activeKey,
}: {
  locale: Locale;
  activeView: ManagementView;
  activeKey: string;
}) {
  const text = dictionary[locale];
  const scopeSubtitle = activeView === 'clusters' ? text.cluster : text.workspace;

  return (
    <aside className="ks-sidebar">
      <button className="ks-scope-selector" type="button">
        <Cluster size={40} />
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
            <button
              key={key}
              className={activeKey === key ? 'ks-nav-child is-active' : 'ks-nav-child'}
              type="button"
            >
              {text[key]}
            </button>
          ))}
        </div>
      </nav>
    </aside>
  );
}
```

## Page Header

The default list page header is title-only. Do not add breadcrumbs, subtitles, resource
documentation, or helper text unless the user explicitly asks.

```tsx
function PageHeader({ title }: { title: string }) {
  return (
    <div className="ks-page-header">
      <h1>{title}</h1>
    </div>
  );
}
```

## List Toolbar

Toolbar actions after the search are fixed: Refresh, Cogwheel/custom columns, then the dark
primary command. Refresh and settings are `variant="text"` icon buttons with toolbar
classes at rest. Create is a text-only primary button by default.

```tsx
function ListToolbar({ locale }: { locale: Locale }) {
  const text = dictionary[locale];

  return (
    <div className="ks-list-toolbar">
      <select className="ks-project-filter" aria-label={text.project}>
        <option>{text.allProjects}</option>
      </select>

      <label className="ks-filter-input">
        <Magnifier size={16} />
        <input placeholder={text.searchPlaceholder} />
      </label>

      <Button
        className="ks-toolbar-icon-button"
        variant="text"
        aria-label={text.refresh}
      >
        <Refresh size={16} />
      </Button>
      <Button
        className="ks-toolbar-icon-button"
        variant="text"
        aria-label={text.tableSettings}
      >
        <Cogwheel size={16} />
      </Button>
      <Button
        className="ks-command-button"
        variant="filled"
        color="secondary"
        radius="xl"
        size="sm"
      >
        {text.create}
      </Button>
    </div>
  );
}
```

## Table

Use the Object Identity Pattern in the first data column: `40px` semantic icon area,
`12px` gap, bold primary title, muted secondary text. Do not render resource icons as
custom mini diagrams, boxed avatars, or generic glyphs.

```tsx
function ResourceTable({ locale }: { locale: Locale }) {
  const text = dictionary[locale];
  const primaryHeader = text[resourceConfig.primaryColumn];
  const secondaryHeader = text[resourceConfig.secondaryColumn];

  return (
    <table className="ks-resource-table">
      <thead>
        <tr>
          <th className="ks-select-cell"><Checkbox /></th>
          <th>{text.name}</th>
          <th>{text.status}</th>
          <th>{text.project}</th>
          <th>{primaryHeader}</th>
          <th>{secondaryHeader}</th>
          <th>{text.updatedTime}</th>
          <th className="ks-action-cell" />
        </tr>
      </thead>
      <tbody>
        {rows.map((row) => (
          <tr key={row.name} className={row.selected ? 'row-selected' : undefined}>
            <td className="ks-select-cell"><Checkbox checked={row.selected} /></td>
            <td>
              <ResourceIdentityCell title={row.name} description={row.description} />
            </td>
            <td>
              <span className={`ks-status-dot is-${row.status}`} />
              <span className="ks-status-text">{text[row.status]}</span>
            </td>
            <td>{row.project}</td>
            <td>{row.image}</td>
            <td>{row.pods}</td>
            <td className="ks-muted">{row.updatedTime}</td>
            <td className="ks-action-cell">
              <Dropdown content={<RowMenu locale={locale} />}>
                <Button
                  className="ks-row-more"
                  variant="text"
                  radius="lg"
                  aria-label={text.more}
                >
                  <More size={16} />
                </Button>
              </Dropdown>
            </td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}

function ResourceIdentityCell({
  title,
  description,
}: {
  title: string;
  description: string;
}) {
  const IdentityIcon = resourceConfig.identityIcon;

  return (
    <div className="ks-object-identity">
      <span className="ks-object-icon" aria-hidden="true">
        <IdentityIcon size={40} />
      </span>
      <span>
        <strong>{title}</strong>
        <small>{description}</small>
      </span>
    </div>
  );
}

function RowMenu({ locale }: { locale: Locale }) {
  const text = dictionary[locale];

  return (
    <Menu>
      <MenuItem>{text.edit}</MenuItem>
      <MenuItem>{text.viewYaml}</MenuItem>
      <MenuItem className="ks-menu-danger">{text.delete}</MenuItem>
    </Menu>
  );
}
```

## Pagination

Do not render numbered page buttons. Use the BasePagination shape: page size and total on
the left, previous icon, `1 / N`, and next icon on the right.

```tsx
function TablePagination({
  locale,
  total,
  page,
  pages,
}: {
  locale: Locale;
  total: number;
  page: number;
  pages: number;
}) {
  const text = dictionary[locale];

  return (
    <footer className="ks-pagination">
      <div className="ks-pagination-left">
        <button className="ks-page-size" type="button">
          {text.perPage} 10 <ChevronDown size={14} />
        </button>
        <span className="ks-pagination-divider" />
        <span className="ks-muted">{text.total(total)}</span>
      </div>
      <div className="ks-pagination-right">
        <Button
          className="ks-pagination-icon"
          variant="text"
          radius="sm"
          aria-label={text.previous}
        >
          <Previous size={20} />
        </Button>
        <strong>{page} / {pages}</strong>
        <Button
          className="ks-pagination-icon"
          variant="text"
          radius="sm"
          aria-label={text.next}
        >
          <Next size={20} />
        </Button>
      </div>
    </footer>
  );
}
```

## CSS Anchor

Use the consuming project's CSS convention, but preserve these values and relationships.

```css
.ks-console {
  min-height: 100vh;
  color: #242e42;
  background: #eff4f9;
  font-size: 12px;
}

.ks-header {
  height: 64px;
  display: flex;
  align-items: center;
  background: #fff;
  padding: 0 20px;
  border-bottom: 1px solid #e3e9ef;
}

.ks-header-brand {
  width: 224px;
  flex-shrink: 0;
}

.ks-header-brand img {
  display: block;
  width: 180px;
  height: auto;
}

.ks-management-nav {
  height: 64px;
  display: flex;
  align-items: center;
}

.ks-management-entry {
  position: relative;
  width: 36px;
  height: 36px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: 0;
  border-radius: 50%;
  background: #f9fbfd;
  box-shadow: none;
  margin-right: 20px;
}

.ks-management-entry.is-active {
  border-radius: 8px;
  background: #242e42;
  box-shadow: 0 4px 8px rgba(36, 46, 66, 0.24);
}

.ks-management-entry:not(.is-active):hover {
  border-radius: 8px;
  background: #eff4f9;
}

.ks-management-indicator {
  position: absolute;
  left: 4px;
  bottom: -14px;
  width: 28px;
  height: 4px;
  border-radius: 2px;
  background: #242e42;
}

.ks-header-spacer { flex: 1; }

.ks-profile {
  height: 36px;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  border: 0;
  background: #f9fbfd;
  border-radius: 18px;
  padding: 0 12px;
  color: #242e42;
  font-size: 12px;
  font-weight: 600;
}

.ks-component-dock {
  min-width: 84px;
  height: 36px;
  padding: 0 16px;
}

.ks-avatar {
  width: 32px;
  height: 32px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex: 0 0 32px;
  border-radius: 50%;
  background: #eff4f9;
  color: #242e42;
  font-size: 12px;
  font-weight: 600;
}

.ks-profile-text {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  line-height: 16px;
}

.ks-profile-text span,
.ks-muted {
  color: #79879c;
}

.ks-header-divider {
  width: 1px;
  height: 20px;
  margin: 0 12px;
  background: #e3e9ef;
}

.ks-console-body {
  display: flex;
  min-height: calc(100vh - 64px);
}

.ks-sidebar {
  width: 220px;
  flex: 0 0 220px;
  padding: 16px 12px 12px;
  background: #fff;
  border-right: 1px solid #e3e9ef;
}

.ks-scope-selector {
  width: 196px;
  height: 68px;
  display: grid;
  grid-template-columns: 40px 1fr 16px;
  align-items: center;
  gap: 12px;
  padding: 0 12px;
  border: 1px solid #ccd3db;
  border-radius: 4px;
  background: linear-gradient(359.02deg, #f9fbfd 0.81%, #ffffff 99.13%);
  text-align: left;
}

.ks-scope-selector span,
.ks-object-identity span {
  min-width: 0;
  display: flex;
  flex-direction: column;
}

.ks-scope-selector strong,
.ks-object-identity strong {
  overflow: hidden;
  color: #242e42;
  font-weight: 600;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.ks-scope-selector small,
.ks-object-identity small {
  overflow: hidden;
  color: #79879c;
  font-size: 12px;
  font-weight: 400;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.ks-resource-nav {
  margin-top: 24px;
}

.ks-nav-row {
  width: 100%;
  height: 36px;
  display: grid;
  grid-template-columns: 20px 1fr 16px;
  align-items: center;
  gap: 8px;
  border: 0;
  background: transparent;
  color: #4a5974;
  font-size: 12px;
  text-align: left;
}

.ks-nav-row span,
.ks-nav-child {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.ks-nav-child {
  width: 100%;
  height: 36px;
  display: block;
  padding: 0 0 0 44px;
  border: 0;
  background: transparent;
  color: #4a5974;
  font-size: 12px;
  text-align: left;
}

.ks-nav-child.is-active {
  color: #55bc8a;
  font-weight: 600;
}

.ks-main {
  flex: 1;
  min-width: 0;
  background: #eff4f9;
}

.ks-page-header {
  height: 56px;
  display: flex;
  align-items: center;
  padding: 0 20px;
  background: #fff;
  border-bottom: 1px solid #e3e9ef;
}

.ks-page-header h1 {
  margin: 0;
  font-size: 18px;
  line-height: 24px;
  font-weight: 600;
}

.ks-content {
  padding: 20px;
}

.ks-list-surface {
  overflow: hidden;
  background: #fff;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(36, 46, 66, 0.04);
}

.ks-list-toolbar {
  height: 56px;
  display: grid;
  grid-template-columns: 148px minmax(280px, 1fr) 32px 32px auto;
  align-items: center;
  gap: 12px;
  padding: 10px 20px;
  background: #f9fbfd;
  border-bottom: 1px solid #e3e9ef;
}

.ks-project-filter,
.ks-filter-input,
.ks-command-button {
  height: 32px;
  font-size: 12px;
}

.ks-project-filter {
  border: 1px solid #ccd3db;
  border-radius: 4px;
  background: #fff;
  padding: 0 12px;
}

.ks-filter-input {
  display: flex;
  align-items: center;
  gap: 8px;
  border: 1px solid transparent;
  border-radius: 18px;
  background: #eff4f9;
  padding: 0 16px;
}

.ks-filter-input input {
  width: 100%;
  min-width: 0;
  border: 0;
  outline: 0;
  background: transparent;
  color: #242e42;
  font-size: 12px;
}

.ks-toolbar-icon-button,
.ks-row-more,
.ks-pagination-icon {
  width: 32px;
  height: 32px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: 0;
  border-radius: 16px;
  background: transparent;
  box-shadow: none;
  color: #36435c;
}

.ks-toolbar-icon-button:hover,
.ks-row-more:hover,
.ks-pagination-icon:hover {
  background: #eff4f9;
}

.ks-command-button {
  min-width: 88px;
  padding: 0 20px;
  border: 0;
  border-radius: 16px;
  background: #242e42;
  color: #fff;
  font-weight: 600;
}

.ks-table-main {
  margin: 0 12px 12px;
}

.ks-resource-table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0;
  table-layout: fixed;
}

.ks-resource-table th,
.ks-resource-table td {
  height: 56px;
  border-bottom: 1px solid #e3e9ef;
  padding: 0 12px;
  text-align: left;
  vertical-align: middle;
  font-size: 12px;
}

.ks-resource-table th {
  height: 44px;
  background: #fff;
  color: #79879c;
  font-weight: 600;
}

.ks-select-cell {
  width: 44px;
}

.ks-action-cell {
  width: 48px;
  text-align: right;
}

.ks-object-identity {
  display: grid;
  grid-template-columns: 40px minmax(0, 1fr);
  align-items: center;
  gap: 12px;
}

.ks-object-icon {
  position: relative;
  width: 40px;
  height: 40px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.ks-status-dot {
  width: 8px;
  height: 8px;
  display: inline-block;
  margin-right: 8px;
  border-radius: 50%;
  vertical-align: middle;
}

.ks-status-dot.is-running {
  background: #55bc8a;
}

.ks-status-dot.is-updating {
  background: #f5a623;
}

.ks-status-dot.is-error {
  background: #ca2621;
}

.ks-status-text {
  vertical-align: middle;
}

.ks-menu-danger {
  color: #ca2621;
}

.ks-resource-table tbody tr.row-selected td {
  background: #eff4f9;
  border-top: 1px solid #55bc8a;
  border-bottom: 1px solid #55bc8a;
}

.ks-resource-table tbody tr.row-selected td:first-child {
  border-left: 1px solid #55bc8a;
}

.ks-resource-table tbody tr.row-selected td:last-child {
  border-right: 1px solid #55bc8a;
}

.ks-pagination {
  height: 52px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 20px;
  background: #f9fbfd;
  border-top: 1px solid #e3e9ef;
}

.ks-pagination-left,
.ks-pagination-right,
.ks-page-size {
  display: inline-flex;
  align-items: center;
  gap: 8px;
}

.ks-page-size {
  height: 32px;
  border: 0;
  background: transparent;
  color: #242e42;
  font-size: 12px;
  font-weight: 600;
}

.ks-pagination-divider {
  width: 1px;
  height: 16px;
  background: #e3e9ef;
}
```

## Locale Dictionary Pattern

Keep generated UI single-locale. Do not render labels such as `容器组 (Pods)`.

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
    workloads: '应用负载',
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
    workloads: 'Application Workloads',
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

## Final Self-Check

Before accepting the output, compare the generated code against this checklist:

- Header logo uses `LOGO_URL`, not text branding or generated artwork.
- Top management entries are icon-only controls with active dark button and bottom
  indicator.
- Inactive top management icons preserve duotone color/fill; they are not recolored with
  only one muted `color` prop.
- Component Dock label is exactly `组件坞` or `Component Dock`.
- Component Dock uses `Grid2Duotone`; it is not a Cogwheel and not CSS-drawn squares.
- Sidebar is white, `220px`, with a `68px-70px` scoped selector.
- Parent sidebar icons use semantic `@kubed/icons`; active duotone sets both `color` and
  `fill`.
- Scoped selector and table identity icons use normal/default duotone unless explicitly
  selected; they are not forced to active green.
- Child sidebar rows are text-only by default.
- Toolbar uses the three-zone layout and FilterInput shape.
- Refresh and settings use toolbar text buttons; row more uses
  `<Button variant="text" radius="lg">`. They are borderless/backgroundless at rest.
- Primary command uses
  `<Button variant="filled" color="secondary" radius="xl" size="sm">` and is text-only by
  default.
- Secondary actions use `<Button variant="filled" color="default" radius="xl" size="sm">`;
  dangerous confirmation uses `<Button variant="filled" color="error" radius="xl" size="sm">`.
- Table first data column follows the Object Identity Pattern.
- Selected rows use pale cells plus thin green outline, not green fill or a left bar.
- Pagination uses page size, total, previous icon, `1 / N`, and next icon; no numbered page
  buttons.
