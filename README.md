# KubeSphere Design

[简体中文](README.zh.md)

A portable AI-agent skill for designing, building, and reviewing React interfaces in the
KubeSphere Enterprise console style.

It can be used with Codex, Claude Code, OpenCode, and other coding agents. The skill gives
agents a shared visual contract, workflow, and reusable implementation blueprint so they can
produce KubeSphere-like console UI more consistently.

## Structure

```text
kubesphere-design/
  SKILL.md
  DESIGN.md
  IMPLEMENTATION.md
```

- `SKILL.md` defines the agent workflow, dependency policy, build order, and verification.
- `DESIGN.md` defines the visual system, layout rules, components, icons, and language rules.
- `IMPLEMENTATION.md` provides a portable React blueprint for the console shell and common page structure.

## Usage

Give the `kubesphere-design/` folder to your agent and include a prompt like:

```text
Use kubesphere-design/SKILL.md, kubesphere-design/DESIGN.md, and kubesphere-design/IMPLEMENTATION.md.
Build a KubeSphere Enterprise console-style React page.
```

For Codex, install it as a local skill:

```bash
git clone https://github.com/XiaohuiSept/skills.git
mkdir -p ~/.codex/skills/kubesphere-design
cp skills/kubesphere-design/SKILL.md ~/.codex/skills/kubesphere-design/SKILL.md
cp skills/kubesphere-design/DESIGN.md ~/.codex/skills/kubesphere-design/DESIGN.md
cp skills/kubesphere-design/IMPLEMENTATION.md ~/.codex/skills/kubesphere-design/IMPLEMENTATION.md
```

Then start a new Codex session and ask it to use the `kubesphere-design` skill.
