# KubeSphere Design Skill

[English](README.md)

这个仓库提供一套可移植的 AI Agent Skill，用于生成接近 KubeSphere Enterprise 控制台风格的前端页面。

它适用于 Codex、Claude Code、OpenCode 等 coding agent。当前重点是高还原度的通用资源列表页：顶部导航、集群 / 企业空间入口、组件坞、浅色侧边栏、作用域选择器、紧凑页面标题、一体化 toolbar/table/pagination，以及 KubeSphere 风格的资源名称展示。

## 文件

```text
kubesphere-design/
  SKILL.md    Agent 执行流程、依赖策略、构建顺序和检查项
  DESIGN.md   KubeSphere 控制台视觉规范
```

两个文件需要一起使用。`SKILL.md` 告诉 agent 怎么做，`DESIGN.md` 定义生成页面应该长什么样。

## 快速使用

把 `kubesphere-design/` 目录提供给你的 agent，然后使用类似下面的 prompt：

```text
Use kubesphere-design/SKILL.md and kubesphere-design/DESIGN.md.
Generate a KubeSphere-style resource list page with the full console frame.
Use public @kubed/components and @kubed/icons when available.
Do not depend on private KubeSphere console source code.
```

在 Codex 中，也可以安装为本地 skill：

```bash
git clone https://github.com/XiaohuiSept/skills.git
mkdir -p ~/.codex/skills/kubesphere-design
cp skills/kubesphere-design/SKILL.md ~/.codex/skills/kubesphere-design/SKILL.md
cp skills/kubesphere-design/DESIGN.md ~/.codex/skills/kubesphere-design/DESIGN.md
```

然后开启新的 Codex 会话，并要求它使用 `kubesphere-design` skill。
