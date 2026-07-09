# KubeSphere Design

English | [简体中文](README.zh.md)

A portable AI-agent skill for designing, building, and reviewing React interfaces in the
KubeSphere Enterprise console style.

Use it with Codex, Claude Code, OpenCode, or other coding agents when you want generated UI
to follow KubeSphere-style console structure, visual rules, component usage, icon mapping,
and terminology.

## Usage

Give the `kubesphere-design/` folder to your agent and include a prompt like:

```text
Use kubesphere-design/SKILL.md to build a KubeSphere Enterprise console-style React page.
Follow the files it tells you to read.
```

For Codex, install it as a local skill:

```bash
git clone https://github.com/XiaohuiSept/skills.git
mkdir -p ~/.codex/skills
cp -R skills/kubesphere-design ~/.codex/skills/kubesphere-design
```

Then start a new Codex session and ask it to use the `kubesphere-design` skill.
