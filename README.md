# KubeSphere Console Agent Skill

[English](README.md) | [简体中文](README.zh.md)

This repository contains a portable AI-agent skill for generating KubeSphere Enterprise
console-style frontend pages with public `kube-design` primitives.

## What This Is

The skill helps coding agents such as Codex, Claude Code, OpenCode, and similar tools
generate frontend pages that look like the KubeSphere Enterprise console instead of generic
admin templates.

Current scope:

- High-fidelity generic resource list pages
- KubeSphere-like top navigation
- Cluster / Workspace management entries
- Component Dock
- Light resource sidebar
- Scoped cluster/workspace selector
- Compact page title band
- Integrated toolbar, table, and pagination
- KubeSphere object identity pattern for resource names and cards

The skill is designed to be portable. It does not require private KubeSphere console source
code or a local `kube-design` checkout.

## Repository Structure

```text
skills/
  README.md
  README.zh.md
  agent/
    SKILL.md
    DESIGN.md
```

Use both files together:

- `agent/SKILL.md` defines the agent workflow, dependency policy, fallback strategy, build
  order, and verification checklist.
- `agent/DESIGN.md` defines the visual contract: colors, typography, layout, navigation,
  table density, object identity pattern, and anti-patterns.

## When To Use It

Use this skill when asking an AI coding agent to build or review:

- KubeSphere-style React pages
- Kubernetes resource list pages
- Console pages based on `@kubed/components` and `@kubed/icons`
- Mock frontend pages that should match KubeSphere Enterprise console visual style

Do not use it as a generic admin dashboard design guide. The goal is KubeSphere console
fidelity.

## Recommended Agent Prompt

Use a prompt like this:

```text
Use the KubeSphere Console Agent Skill.

First read:
1. agent/SKILL.md
2. agent/DESIGN.md

Generate a high-fidelity KubeSphere-style resource list page using the consuming project's
available @kubed/components and @kubed/icons APIs. Do not depend on private console source
code. Keep the full console frame accurate before optimizing resource-specific content.
```

For a concrete page:

```text
Use agent/SKILL.md and agent/DESIGN.md to generate a KubeSphere-style Deployments list page.
The page should include the full console frame, light sidebar, scoped selector, table
toolbar, realistic rows, status dots, Object Identity Pattern name cells, and attached
pagination. No breadcrumb.
```

## Using With Codex

Codex can consume skills as directories containing a `SKILL.md` file. Put both files in the
same skill directory so `SKILL.md` can reference the companion `DESIGN.md`.

Example local install:

```bash
git clone https://github.com/XiaohuiSept/skills.git
mkdir -p ~/.codex/skills/kube-design-agent
cp skills/agent/SKILL.md ~/.codex/skills/kube-design-agent/SKILL.md
cp skills/agent/DESIGN.md ~/.codex/skills/kube-design-agent/DESIGN.md
```

Then restart Codex or start a new session. Ask Codex to use the `kube-design-agent` skill
when generating KubeSphere-style pages.

If you do not want to install the skill, attach or reference both files in the session and
ask Codex to read `SKILL.md` before `DESIGN.md`.

## Using With Claude Code

Claude Code projects commonly use `CLAUDE.md` for persistent project instructions. Put this
repository's `agent/` folder somewhere in your project, then reference it from `CLAUDE.md`.

Example `CLAUDE.md` snippet:

```md
When generating KubeSphere-style frontend pages, read these files first:

1. agent/SKILL.md
2. agent/DESIGN.md

Follow SKILL.md for workflow and verification. Follow DESIGN.md as the visual source of
truth. Do not require private KubeSphere console repositories.
```

You can also paste or attach both files directly in a Claude session and explicitly ask
Claude to follow them.

## Using With OpenCode

OpenCode-style workflows commonly use `AGENTS.md` for project-level agent instructions.
Put this repository's `agent/` folder in your project, then reference both files from
`AGENTS.md`.

Example `AGENTS.md` snippet:

```md
For KubeSphere-style console UI work:

- Read `agent/SKILL.md` first.
- Read `agent/DESIGN.md` second.
- Use public `@kubed/components` and `@kubed/icons`.
- Do not depend on private console source code.
- Prioritize full console-frame fidelity before resource-specific content.
```

## General Usage For Other Agents

For any coding agent that supports file context, custom instructions, or project memory:

1. Provide both `agent/SKILL.md` and `agent/DESIGN.md`.
2. Tell the agent to read `SKILL.md` first.
3. Tell the agent to treat `DESIGN.md` as the visual source of truth.
4. Ask the agent to implement with the consuming project's installed `@kubed/*` packages.
5. Ask the agent to run build/typecheck or visually inspect the page when possible.

Minimum instruction:

```text
Before building the UI, read agent/SKILL.md and agent/DESIGN.md. SKILL.md controls workflow;
DESIGN.md controls visual fidelity.
```

## Important Constraints

- The skill should not require `kse-console-kse` or any private repository.
- The skill should not require a local `kube-design` source checkout.
- The consuming project should provide `@kubed/components` and `@kubed/icons`, or the agent
  should adapt to whatever public kube-design APIs are installed.
- The UI should stay light by default. Do not generate a dark sidebar.
- Generated pages should not include breadcrumb navigation unless explicitly requested.
- The first viewport should look like KubeSphere Enterprise before the resource data is
  judged.

## Expected Output Quality

A good generated page should have:

- `64px` white global header
- Real or restrained KubeSphere logo
- Cluster / Workspace top management entries
- Component Dock before the user menu
- `220px` light sidebar
- Scoped selector tied to the active management view
- Compact `56px` page title band
- One integrated list surface
- FilterInput-style search
- White table header
- Dense `56px` resource rows
- Object Identity Pattern name cells
- Attached pagination

## References

- [OpenAI skills catalog](https://github.com/openai/skills)
- [Claude Code settings and custom instructions](https://code.claude.com/docs/en/settings)
- [OpenCode instructions](https://opencode.school/lessons/instructions/)
- [AGENTS.md](https://agents.md/)
