# d2-diagram

English | 中文

## English

`d2-diagram` is a Claude Code skill for generating production-quality [D2](https://d2lang.com/) diagrams for system and process visualization.

It is designed for:

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

### Repository Layout

```text
.
├── README.md
└── skill/
    ├── SKILL.md
    └── references/
        ├── d2-examples.md
        └── d2-style-guide.md
```

### Install

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

### Notes

- The install target must contain `SKILL.md` at its root.
- The `references/` directory is part of the skill package and should be copied together with `SKILL.md`.
- The skill defaults to ELK for layouts. `sequence_diagram` remains the only practical exception because D2 handles it internally.

### Render Example

```bash
d2 --sketch --theme 200 --layout elk input.d2 output.svg
```

## 中文

`d2-diagram` 是一个 Claude Code skill，用来生成高质量的 [D2](https://d2lang.com/) 图，适合系统和流程类可视化。

适用场景包括：

- 系统架构图
- 流程图
- 时序图
- ER 图和数据库 schema 图
- 网络拓扑图
- CI/CD 流水线
- 云基础设施图
- Kubernetes 拓扑图
- 数据流和状态机图
- 组织结构图、类图、路线图

### 仓库结构

```text
.
├── README.md
└── skill/
    ├── SKILL.md
    └── references/
        ├── d2-examples.md
        └── d2-style-guide.md
```

### 安装方式

安装到个人全局目录：

```bash
mkdir -p ~/.claude/skills/d2-diagram
cp -R skill/. ~/.claude/skills/d2-diagram/
```

安装到当前项目：

```bash
mkdir -p .claude/skills/d2-diagram
cp -R skill/. .claude/skills/d2-diagram/
```

### 说明

- 安装目标目录的根层必须直接包含 `SKILL.md`。
- `references/` 是 skill 的一部分，复制时要和 `SKILL.md` 一起带上。
- 这个 skill 默认使用 ELK 布局；`sequence_diagram` 仍然是唯一的例外，因为 D2 会内部处理它。

### 渲染示例

```bash
d2 --sketch --theme 200 --layout elk input.d2 output.svg
```
