# KubeSphere 控制台 Agent Skill

[English](README.md) | [简体中文](README.zh.md)

这个仓库提供一套可移植的 AI Agent Skill，用于让 Codex、Claude Code、OpenCode
等 coding agent 生成更接近 KubeSphere Enterprise 控制台风格的前端页面。

## 这是什么

这个 skill 的目标不是生成普通后台模板，而是指导 agent 使用公开的
`kube-design` / `@kubed/components` / `@kubed/icons` 能力，生成具有 KubeSphere
控制台风格的页面。

## 仓库结构

```text
skills/
  README.md
  README.zh.md
  agent/
    SKILL.md
    DESIGN.md
```

两个文件需要一起使用：

- `agent/SKILL.md`：定义 agent 的执行流程、依赖策略、fallback、构建顺序和检查项。
- `agent/DESIGN.md`：定义视觉规范，包括颜色、字体、布局、导航、表格密度、
  Object Identity Pattern 和反模式。

## 适用场景

当你希望 AI Agent 生成或评审下面这些页面时，可以使用这个 skill：

- KubeSphere 风格 React 页面
- Kubernetes 资源列表页
- 基于 `@kubed/components` 和 `@kubed/icons` 的控制台页面
- 需要接近 KubeSphere Enterprise 控制台视觉风格的 mock 页面

不要把它当成普通 admin dashboard 设计指南。它的目标是 KubeSphere 控制台还原度。

## 推荐 Prompt

```text
Use the KubeSphere Console Agent Skill.

First read:
1. agent/SKILL.md
2. agent/DESIGN.md

Generate a high-fidelity KubeSphere-style resource list page using the consuming project's
available @kubed/components and @kubed/icons APIs. Do not depend on private console source
code. Keep the full console frame accurate before optimizing resource-specific content.
```

生成具体页面时可以这样说：

```text
Use agent/SKILL.md and agent/DESIGN.md to generate a KubeSphere-style Deployments list page.
The page should include the full console frame, light sidebar, scoped selector, table
toolbar, realistic rows, status dots, Object Identity Pattern name cells, and attached
pagination. No breadcrumb.
```

## 在 Codex 中使用

Codex 可以使用包含 `SKILL.md` 的技能目录。建议把 `SKILL.md` 和 `DESIGN.md` 放在同一个
skill 目录里。

本地安装示例：

```bash
git clone https://github.com/XiaohuiSept/skills.git
mkdir -p ~/.codex/skills/kube-design-agent
cp skills/agent/SKILL.md ~/.codex/skills/kube-design-agent/SKILL.md
cp skills/agent/DESIGN.md ~/.codex/skills/kube-design-agent/DESIGN.md
```

然后重启 Codex 或开启新会话。生成 KubeSphere 风格页面时，要求 Codex 使用
`kube-design-agent` skill。

如果不想安装，也可以在会话里直接附加或引用这两个文件，并要求 Codex 先读
`SKILL.md`，再读 `DESIGN.md`。

## 在 Claude Code 中使用

Claude Code 项目通常可以用 `CLAUDE.md` 放置长期项目指令。你可以把本仓库的 `agent/`
目录放进项目，然后在 `CLAUDE.md` 里引用。

示例：

```md
When generating KubeSphere-style frontend pages, read these files first:

1. agent/SKILL.md
2. agent/DESIGN.md

Follow SKILL.md for workflow and verification. Follow DESIGN.md as the visual source of
truth. Do not require private KubeSphere console repositories.
```

也可以在 Claude 会话里直接粘贴或附加两个文件，并明确要求 Claude 按它们执行。

## 在 OpenCode 中使用

OpenCode 类工作流通常可以用 `AGENTS.md` 放置项目级 agent 指令。你可以把本仓库的
`agent/` 目录放进项目，然后在 `AGENTS.md` 里引用。

示例：

```md
For KubeSphere-style console UI work:

- Read `agent/SKILL.md` first.
- Read `agent/DESIGN.md` second.
- Use public `@kubed/components` and `@kubed/icons`.
- Do not depend on private console source code.
- Prioritize full console-frame fidelity before resource-specific content.
```

## 其他 Agent 的通用用法

只要某个 coding agent 支持文件上下文、自定义指令或项目记忆，都可以这样使用：

1. 提供 `agent/SKILL.md` 和 `agent/DESIGN.md`。
2. 要求 agent 先读 `SKILL.md`。
3. 要求 agent 把 `DESIGN.md` 当成视觉规范来源。
4. 要求 agent 使用项目里已安装的 `@kubed/*` 包。
5. 如果环境支持，要求 agent 运行 build/typecheck 或做页面视觉检查。

最小指令：

```text
Before building the UI, read agent/SKILL.md and agent/DESIGN.md. SKILL.md controls workflow;
DESIGN.md controls visual fidelity.
```


## 参考链接

- [AGENTS.md](https://agents.md/)
- [DESIGN.md](https://stitch.withgoogle.com/docs/design-md/overview/)
