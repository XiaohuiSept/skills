# KubeSphere Design

English | [简体中文](README.zh.md)

A portable AI-agent skill for designing, building, and reviewing React interfaces in the
KubeSphere Enterprise console style.

Use it with Codex, Claude, Antigravity, OpenCode, or other coding agents when you want
generated UI to follow KubeSphere-style console structure, visual rules, component usage,
icon mapping, and terminology.

## Usage

### Codex

Install it as a local Codex skill:

```bash
git clone https://github.com/XiaohuiSept/skills.git
mkdir -p ~/.codex/skills
cp -R skills/kubesphere-design ~/.codex/skills/kubesphere-design
```

Then start a new Codex session and ask it to use the `kubesphere-design` skill.

### Claude

Add the `kubesphere-design/` folder to your project or provide it as context, then prompt
Claude with:

```text
Use kubesphere-design/SKILL.md to build a KubeSphere Enterprise console-style React page.
Follow the files it tells you to read.
```

### Antigravity

Add the `kubesphere-design/` folder to the workspace or attach it as reference context, then
ask Antigravity to follow the skill entry:

```text
Use kubesphere-design/SKILL.md for KubeSphere Enterprise console-style UI generation.
```

### OpenCode

Place the `kubesphere-design/` folder in your repo or provide it as context, then reference
the skill in your prompt:

```text
Use kubesphere-design/SKILL.md and follow its required reading order.
Build a KubeSphere Enterprise console-style React page.
```
