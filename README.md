# d2-diagram

[中文说明 / Chinese translation](./README_CN.md)

`d2-diagram` is a Claude Code skill for generating production-quality [D2](https://d2lang.com/) diagrams for system and process visualization.

## Attribution

This repository is based on the original work by **Altstaq-Apps**:

- Upstream repository: https://github.com/Altstaq-Apps/d2-diagram-skill

This fork is maintained by **Eryc123Y** and adapts the original skill with additional restructuring, documentation, ELK-by-default guidance, and UML 2.5.1 relationship notation support.

## Scope

This skill is designed for:

- system architecture
- flowcharts
- sequence diagrams
- ER and schema diagrams
- network topology
- CI/CD pipelines
- cloud infrastructure
- Kubernetes topology
- data-flow and state-machine diagrams
- org charts, class diagrams, and roadmaps

## Repository Layout

```text
.
├── README.md
├── README_CN.md
└── skill/
    ├── SKILL.md
    └── references/
        ├── d2-examples.md
        ├── d2-style-guide.md
        └── uml-2.5.1-notation.md
```

## Install

Personal install:

```bash
mkdir -p ~/.claude/skills/d2-diagram
cp -R skill/. ~/.claude/skills/d2-diagram/
```

Project install:

```bash
mkdir -p .claude/skills/d2-diagram
cp -R skill/. .claude/skills/d2-diagram/
```

## Notes

- The install target must contain `SKILL.md` at its root.
- The `references/` directory is part of the skill package and should be copied together with `SKILL.md`.
- The skill defaults to ELK for layouts. `sequence_diagram` remains the only practical exception because D2 handles it internally.
- A dedicated UML 2.5.1 notation reference is bundled for relationship-accurate UML diagrams.

## Render Example

```bash
d2 --sketch --theme 200 --layout elk input.d2 output.svg
```
