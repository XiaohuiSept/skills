# Language Reference

Use one locale per page. Do not render bilingual duplicates such as `容器组 (Pods)` or
`Deployments / 部署`.

## Priority

| Source | Priority |
|---|---|
| Explicit user locale | Highest |
| Consuming project locale | High |
| `assets/icon-map.md` UI terms | High |
| KubeSphere official glossary | Supplemental |
| Kubernetes English original | Fallback |

KubeSphere official glossary:
https://docs.kubesphere.com.cn/v4.2.1/25-glossary/glossary-final/

Do not copy the official glossary wholesale into generated pages. Use it to resolve
uncertain terminology. If official wording and current console UI wording differ, prefer
the fixed UI mapping in `assets/icon-map.md` for generated console labels.

## Rules

- English UI keeps standard Kubernetes casing such as `Pods`, `StatefulSets`, `DaemonSets`,
  `CronJobs`, and `ConfigMaps`.
- Chinese UI uses established KubeSphere/Kubernetes terms from `assets/icon-map.md`.
- If unsure about a Kubernetes resource name, keep the standard Kubernetes English term
  rather than inventing a translation.
- Do not mix UI languages in navigation, toolbar, table headers, status, pagination, or
  action menus.
