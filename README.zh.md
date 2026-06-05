# KubeSphere Design

[English](README.md)

一套可移植的 AI Agent Skill，用于设计、生成和审查 KubeSphere Enterprise 控制台风格的 React 界面。

它适用于 Codex、Claude Code、OpenCode 等 coding agent。Skill 提供统一的视觉规范、执行流程和可复用实现骨架，帮助 agent 更稳定地生成 KubeSphere 风格的控制台 UI。

## 结构

```text
kubesphere-design/
  SKILL.md
  DESIGN.md
  IMPLEMENTATION.md
```

- `SKILL.md` 定义 agent 执行流程、依赖策略、构建顺序和检查项。
- `DESIGN.md` 定义视觉系统、布局规则、组件、图标和语言规则。
- `IMPLEMENTATION.md` 提供控制台框架和通用页面结构的可移植 React 实现骨架。

## 使用

把 `kubesphere-design/` 目录提供给你的 agent，并使用类似下面的 prompt：

```text
Use kubesphere-design/SKILL.md, kubesphere-design/DESIGN.md, and kubesphere-design/IMPLEMENTATION.md.
Build a KubeSphere Enterprise console-style React page.
```

在 Codex 中，可以安装为本地 skill：

```bash
git clone https://github.com/XiaohuiSept/skills.git
mkdir -p ~/.codex/skills/kubesphere-design
cp skills/kubesphere-design/SKILL.md ~/.codex/skills/kubesphere-design/SKILL.md
cp skills/kubesphere-design/DESIGN.md ~/.codex/skills/kubesphere-design/DESIGN.md
cp skills/kubesphere-design/IMPLEMENTATION.md ~/.codex/skills/kubesphere-design/IMPLEMENTATION.md
```

然后开启新的 Codex 会话，并要求它使用 `kubesphere-design` skill。
