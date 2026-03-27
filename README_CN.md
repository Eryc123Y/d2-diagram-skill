# d2-diagram

[English documentation / 英文文档](./README.md)

`d2-diagram` 是一个 Claude Code skill，用来生成高质量的 [D2](https://d2lang.com/) 图，适合系统和流程类可视化。

## 致谢与来源

这个仓库基于 **Altstaq-Apps** 的原始工作修改而来：

- 上游仓库：https://github.com/Altstaq-Apps/d2-diagram-skill

当前 fork 由 **Eryc123Y** 维护。这里的工作是在原作者基础上进行整理和扩展，包括目录重构、文档更新、默认 ELK 布局，以及 UML 2.5.1 关系标记支持。

## 适用范围

这个 skill 适合用于：

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

## 仓库结构

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

## 安装方式

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

## 说明

- 安装目标目录的根层必须直接包含 `SKILL.md`。
- `references/` 是 skill 的一部分，复制时要和 `SKILL.md` 一起带上。
- 这个 skill 默认使用 ELK 布局；`sequence_diagram` 仍然是唯一的例外，因为 D2 会内部处理它。
- 仓库内置了 UML 2.5.1 关系标记参考，适合需要严格关系语义的 UML 图。

## 渲染示例

```bash
d2 --sketch --theme 200 --layout elk input.d2 output.svg
```
