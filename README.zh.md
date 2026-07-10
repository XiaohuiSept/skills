# KubeSphere Design

[English](README.md) | 简体中文

一套可移植的 AI Agent Skill，用于设计、生成和审查 KubeSphere Enterprise 控制台风格的
React 界面。

当你希望 Codex、Claude、Antigravity、OpenCode 或其他 coding agent 生成的 UI 遵循
KubeSphere 控制台的页面结构、视觉规范、组件用法、图标映射和术语规则时，可以使用这套
skill。

## 使用

### Codex

安装为 Codex 本地 skill：

```bash
git clone https://github.com/XiaohuiSept/skills.git
mkdir -p ~/.codex/skills
cp -R skills/kubesphere-design ~/.codex/skills/kubesphere-design
```

然后开启新的 Codex 会话，并要求它使用 `kubesphere-design` skill。

### Claude

把 `kubesphere-design/` 目录加入项目或作为上下文提供给 Claude，并使用类似下面的
prompt：

```text
Use kubesphere-design/SKILL.md to build a KubeSphere Enterprise console-style React page.
Follow the files it tells you to read.
```

### Antigravity

把 `kubesphere-design/` 目录加入 workspace 或作为参考上下文提供给 Antigravity，并让它读取
skill 入口：

```text
Use kubesphere-design/SKILL.md for KubeSphere Enterprise console-style UI generation.
```

### OpenCode

把 `kubesphere-design/` 目录放到仓库中或作为上下文提供给 OpenCode，并在 prompt 中引用：

```text
Use kubesphere-design/SKILL.md and follow its required reading order.
Build a KubeSphere Enterprise console-style React page.
```
