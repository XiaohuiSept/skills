# Icon And Terminology Map

This is an execution table. Use it before choosing resource/menu icons or localized UI
labels. If a label is absent, choose the closest semantic `@kubed/icons` component by exact
resource/menu name.

| Chinese | English | Key | `@kubed/icons` | Notes |
|---|---|---|---|---|
| 集群 | Cluster | `cluster` | `Cluster` | Scope/top entry |
| 企业空间 | Workspace | `workspace` | `Workspace` | Scope/top entry |
| 概览 | Overview | `overview` | `Dashboard` | Navigation |
| 节点 | Nodes | `nodes` | `Nodes` | Resource |
| 项目 | Projects / Namespace | `projects` | `Project` | KubeSphere UI uses Projects |
| 工作负载 | Workloads | `workloads` | `Appcenter` | Navigation group |
| 部署 | Deployments | `deployments` | `Backup` | Workloads child |
| 有状态副本集 | StatefulSets | `statefulsets` | `StatefulSet` | Workloads child |
| 守护进程集 | DaemonSets | `daemonsets` | `DeamonSet` | Workloads child |
| 任务 | Jobs | `jobs` | `Backup` | Workloads child |
| 定时任务 | CronJobs | `cronjobs` | `Backup` | Workloads child |
| 容器组 | Pods | `pods` | `PodDuotone` | Resource |
| 工作负载模板 | Workload Templates | `workload-templates` | `NoteCopyDuotone` | Resource |
| 弹性伸缩 | Elastic Scaling | `elastic-scaling` | `Stretch` | Resource |
| 服务与网络 | Service And Network | `service-network` | `Earth` | Navigation group |
| 服务 | Services | `services` | `Appcenter` | Resource |
| 应用路由 | Ingresses | `ingresses` | `Loadbalancer` | Resource |
| 网关 | Gateway | `gateway` | `GatewayDuotone` | Resource |
| 网络策略 | Network Policies | `network-policies` | `Firewall` | Resource |
| 容器组 IP 池 | Pod IP Pools | `pod-ip-pools` | `EipGroup` | Resource |
| 存储 | Storage | `storage` | `Database` | Navigation group |
| 持久卷声明 | Persistent Volume Claims | `persistent-volume-claims` | `Storage` | Resource |
| 卷快照 | Volume Snapshot | `volume-snapshot` | `Snapshot` | Resource |
| 配置 | Configuration | `configuration` | `Hammer` | Navigation group |
| 保密字典 | Secrets | `secrets` | `Key` | Resource |
| 配置字典 | ConfigMaps | `configmaps` | `Hammer` | Resource |
| 服务帐户 | Service Accounts | `service-accounts` | `Client` | UI term follows existing menu |
| 定制资源定义 | CRDs | `crds` | `Select` | Resource |
| 监控告警 | Monitoring & Alerting | `monitoring-alerting` | `Monitor` | Navigation group |
| 告警 | Alerts | `alerts` | `Loudspeaker` | Resource |
| 告警规则组 | Alerts Rule Groups | `alert-rule-groups` | `BellGearDuotone` | Resource |
| 集群设置 | Cluster Settings | `cluster-settings` | `Cogwheel` | Settings |
| 日志 | Logs | `logs` | `Log` | Resource |
| 应用管理 | App Management | `app-management` | `AppsGearDuotone` | Navigation group |
| 应用仓库 | App Repositories | `app-repositories` | `Catalog` | Resource |
| 企业空间设置 | Workspace Settings | `workspace-settings` | `Cogwheel` | Settings |
| 权限管理 | Access Control | `access-control` | `Key` | Navigation group |
| 用户 | Users | `users` | `Human` | Resource |
| 角色 | Roles | `roles` | `Role` | Resource |
| 组件坞 | Component Dock | `component-dock` | `Grid2Duotone` | Header action |
| 刷新 | Refresh | `refresh` | `Refresh` | Toolbar action |
| 删除 | Delete | `delete` | `Trash` | Destructive action |

## Duotone State Rules

| Context | Primary/color | Secondary/fill |
|---|---:|---:|
| Inactive top management icon | `#36435c` | `#b6c2cd` |
| Inactive resource/navigation icon on light background | package default | package default |
| Active sidebar item | `#00aa72` | `#90e0c5` |
| Active top icon on dark button | `rgba(255,255,255,0.9)` | `#ffffff66` |

Set both channels in React when overriding icon color. Do not recolor only with CSS
`color`. Top Cluster/Workspace management entries must set both channels even when
inactive; relying on the icon package default may render the Workspace icon as a black
single-channel glyph.
