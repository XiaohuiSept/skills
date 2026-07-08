# KubeSphere Design

English | [简体中文](README.zh.md)

A portable AI-agent skill for designing, building, and reviewing React interfaces in the
KubeSphere Enterprise console style.

Use it with Codex, Claude Code, OpenCode, or other coding agents when you want generated UI
to follow KubeSphere-style console structure, visual rules, component usage, icon mapping,
and terminology.

## Structure

```text
kubesphere-design/
  SKILL.md
  DESIGN.md
  assets/
    tokens.md
    icon-map.md
    recipes.md
    page-shells/
      list.md
  references/
    console-frame.md
    list-pages.md
    components.md
    language.md
    case-studies/
```

`SKILL.md` is the main entry for agents. It routes the task and tells the agent which
supporting files to read. `DESIGN.md` provides the visual contract. Files under `assets/`
and `references/` provide reusable rules, page shells, recipes, and failure patterns.

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
