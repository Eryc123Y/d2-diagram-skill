# d2-diagram

[中文说明 / Chinese translation](./README_CN.md)

`d2-diagram` is a Claude Code skill for generating production-quality [D2](https://d2lang.com/) diagrams for system and process visualization.

It is built for people who want diagrams that are readable, stylish, and structurally correct without falling back to Mermaid, ASCII sketches, or ad hoc notation.

## Why this fork

This repository is based on the original work by **Altstaq-Apps**:

- Upstream repository: https://github.com/Altstaq-Apps/d2-diagram-skill

This fork is maintained by **Eryc123Y**. The current version keeps the original foundation, then extends it with:

- a cleaner public repo structure
- separate English and Chinese documentation
- ELK-first layout guidance
- bundled UML 2.5.1 relationship notation reference
- a more explicit installation path for the `skills` CLI ecosystem

## What the skill covers

Use it for:

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

## Built-in styles

The skill ships with a few strong defaults instead of one generic look:

- **Dark sketch** for engineering architecture diagrams
- **Light sketch** for flow and process diagrams in docs or presentations
- **Minimal formal** for UML-style and print-friendly diagrams
- **Strict UML mode** when relationship semantics matter

The skill also defaults to **ELK** for layouts, while treating `sequence_diagram` as the practical exception because D2 handles that mode differently.

## Example gallery

### Dark architecture

[Source](./examples/src/dark-architecture.d2) | [Rendered SVG](./examples/rendered/dark-architecture.svg)

![Dark architecture example](./examples/rendered/dark-architecture.svg)

### Light flowchart

[Source](./examples/src/light-flowchart.d2) | [Rendered SVG](./examples/rendered/light-flowchart.svg)

![Light flowchart example](./examples/rendered/light-flowchart.svg)

### Minimal UML

[Source](./examples/src/minimal-uml.d2) | [Rendered SVG](./examples/rendered/minimal-uml.svg)

![Minimal UML example](./examples/rendered/minimal-uml.svg)

### Sequence diagram

[Source](./examples/src/sequence-signin.d2) | [Rendered SVG](./examples/rendered/sequence-signin.svg)

![Sequence example](./examples/rendered/sequence-signin.svg)

## Install

### Install via skills.sh / skills CLI

Install from GitHub with the skills CLI:

```bash
npx skills add https://github.com/Eryc123Y/d2-diagram-skill --skill d2-diagram
```

Global install:

```bash
npx skills add https://github.com/Eryc123Y/d2-diagram-skill --skill d2-diagram -g
```

### Manual install

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

## Repository layout

```text
.
├── README.md
├── README_CN.md
├── examples/
│   ├── rendered/
│   └── src/
└── skill/
    ├── SKILL.md
    └── references/
        ├── d2-examples.md
        ├── d2-style-guide.md
        └── uml-2.5.1-notation.md
```

## Notes

- The install target must contain `SKILL.md` at its root.
- The `references/` directory is part of the skill package and should be copied together with `SKILL.md`.
- The repo includes a bundled UML 2.5.1 notation reference for relationship-accurate UML diagrams.
- The examples in this repo are intentionally small and presentation-friendly. The full working guidance lives in `skill/SKILL.md` and the bundled references.

## Render example

```bash
d2 --sketch --theme 200 --layout elk input.d2 output.svg
```
