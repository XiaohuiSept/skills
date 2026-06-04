# KubeSphere Design Skill

[简体中文](README.zh.md)

This repository contains a portable AI-agent skill for generating frontend pages that look
like the KubeSphere Enterprise console.

It is designed for coding agents such as Codex, Claude Code, OpenCode, and similar tools.
The current focus is high-fidelity generic resource list pages: global header, Cluster /
Workspace navigation, Component Dock, light sidebar, scoped selector, compact page header,
integrated toolbar/table/pagination, and KubeSphere-style resource identity cells.

## Files

```text
agent/
  SKILL.md    Agent workflow, dependency policy, build order, and verification
  DESIGN.md   Visual contract for the KubeSphere console style
```

Use both files together. `SKILL.md` tells the agent how to work; `DESIGN.md` defines what
the generated UI should look like.

## Quick Use

Give your agent the `agent/` folder, then use a prompt like:

```text
Use agent/SKILL.md and agent/DESIGN.md.
Generate a KubeSphere-style resource list page with the full console frame.
Use public @kubed/components and @kubed/icons when available.
Do not depend on private KubeSphere console source code.
```

For Codex, you can also install it as a local skill:

```bash
git clone https://github.com/XiaohuiSept/skills.git
mkdir -p ~/.codex/skills/kubesphere-design
cp skills/agent/SKILL.md ~/.codex/skills/kubesphere-design/SKILL.md
cp skills/agent/DESIGN.md ~/.codex/skills/kubesphere-design/DESIGN.md
```

Then start a new Codex session and ask it to use the `kubesphere-design` skill.
