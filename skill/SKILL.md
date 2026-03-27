---
name: d2-diagram
description: "Generate production-quality D2 diagrams for system and process visualization. Use this skill whenever the user asks to diagram, draw, sketch, visualize, or map out system architecture, flowcharts, sequence diagrams, ER/database schemas, network topology, CI/CD pipelines, cloud infrastructure (AWS/GCP/Azure), Kubernetes, data flows, state machines, org charts, class diagrams, or roadmaps/timelines. Also activate when the user says 'show me how X connects to Y', 'make this visual', or 'create a diagram' for a structured system or workflow."
---

# D2 Diagram Skill

## Design Philosophy

1. **Semantic shapes first** — cylinders for databases, clouds for managed services, queues for message buses, hexagons for microservices, person for actors
2. **Palette matches context** — dark/navy for engineering audiences; light/minimal for presentations and docs; infer from context or ask
3. **Sketch by default** — hand-drawn aesthetic for exploratory and engineering diagrams; disable for formal print-ready output
4. **ELK by default** — use `layout-engine: elk` for every diagram unless D2 forces `sequence_diagram` to use dagre internally
5. **Typed connectors** — every edge communicates meaning (sync vs async, data-flow vs monitoring, boundary-crossing)
6. **Classes for consistency** — define style classes once, apply everywhere; never repeat inline styles
7. **Structure before connections** — declare all containers and nodes first, then draw all edges at the bottom

## Diagram Type Selection

| Type                     | D2 Approach                                 | Key Config                        |
| ------------------------ | ------------------------------------------- | --------------------------------- |
| **System Architecture**  | Nested containers + typed edges             | `direction: right`, ELK           |
| **Sequence Diagram**     | `shape: sequence_diagram`                   | D2 falls back to dagre internally |
| **ER / Schema**          | `shape: sql_table` with constraints         | `direction: right`, ELK           |
| **Flowchart / Decision** | `diamond` + `step` + `oval` shapes          | `direction: down`, ELK            |
| **State Machine**        | Nodes as states, labeled transition edges   | `circle` for start/end terminals  |
| **Org Chart**            | `person` shapes + containers                | `direction: down`, ELK            |
| **Class Diagram**        | `sql_table` approximation                   | Fields + methods as rows          |
| **CI/CD Pipeline**       | `step` shapes + grid layout                 | `direction: right`                |
| **Data Flow / ETL**      | Animated edges + pipeline-step class        | `animated: true`                  |
| **Cloud Infra**          | Account/VPC/subnet containers, vendor icons | 5-level nesting                   |
| **K8s Topology**         | Cluster → namespace → deployment            | `multiple: true` for replicas     |
| **Network / Security**   | Trust zones + boundary-crossing edges       | `security-boundary` class         |
| **Roadmap / Timeline**   | `grid-rows` + `grid-columns` per quarter    | `step` shapes per item            |

For complete renderable examples of every type, see `references/d2-examples.md`.

## UML Mode

When the user explicitly asks for UML, class diagrams, object diagrams, package diagrams, component diagrams, or relationship-accurate notation, switch into **strict UML mode**:

- Follow UML 2.5.1 notation concepts and naming.
- Prefer D2's native UML support where available, especially `shape: class`.
- Use correct relationship markers instead of generic arrows whenever D2 can express them.
- Do not mix informal architecture notation with UML notation on the same diagram unless the user asks for a hybrid.

Relationship rules in UML mode:

- **Association**: plain solid line. Add navigability only if the user specifies direction.
- **Directed association**: solid line with arrowhead on the navigable end.
- **Aggregation**: hollow diamond on the whole/aggregate side.
- **Composition**: filled diamond on the whole/composite side.
- **Generalization**: solid line with hollow triangle pointing to the parent.
- **Realization**: dashed line with hollow triangle pointing to the interface/specification.
- **Dependency**: dashed line with open arrow.

In D2, use `source-arrowhead` and `target-arrowhead` overrides to model these precisely. See `references/uml-2.5.1-notation.md` for concrete patterns.

## Palette Options

Choose based on the audience and output context. Default to **dark** for engineering diagrams.

### Dark (engineering / technical audiences)

```d2
vars: {
  d2-config: { sketch: true; theme-id: 200; layout-engine: elk }
  bg:      "#1C2F4A"   surface:  "#162646"   text:      "#D6D6DA"
  primary: "#385FAF"   accent:   "#9CADD7"   highlight: "#F5871F"
  muted:   "#739BCF"   success:  "#4CAF80"   danger:    "#E24B4A"
  text-light: "#FFFFFF"
}
```

### Light (presentations / mixed audiences)

```d2
vars: {
  d2-config: { sketch: true; theme-id: 0; layout-engine: elk }
  bg:      "#FFFFFF"   surface:  "#F3F4F6"   text:      "#111827"
  primary: "#2563EB"   accent:   "#7C3AED"   highlight: "#F59E0B"
  muted:   "#6B7280"   success:  "#16A34A"   danger:    "#DC2626"
  text-light: "#FFFFFF"
}
```

### Minimal (formal docs / print)

```d2
vars: {
  d2-config: { sketch: false; theme-id: 0; layout-engine: elk }
  bg:      "#FFFFFF"   surface:  "#F9FAFB"   text:      "#1F2937"
  primary: "#374151"   accent:   "#6B7280"   highlight: "#EF4444"
  muted:   "#9CA3AF"   success:  "#059669"   danger:    "#DC2626"
  text-light: "#FFFFFF"
}
```

For additional palettes and full color semantics, see `references/d2-style-guide.md` → Palette Options.

## Core Template

Every diagram uses this structure order — never deviate from it:

```d2
# DIAGRAM TYPE: <Architecture | Sequence | ER | Flowchart | State Machine | Org Chart | ...>
# AUDIENCE: <who reads this>
# Shows: <one sentence>

direction: right   # or: down

vars: {
  d2-config: {
    sketch: true           # false for formal/print output
    theme-id: 200          # 200 = dark terminal, 0 = light
    layout-engine: elk     # elk for most diagrams; sequence_diagram uses dagre internally
  }
  # palette vars here (copy from Palette Options above)
}

classes: {
  # All style classes defined here
  # See references/d2-style-guide.md > "Default Class Library" for the full reusable set
}

# 1. Top-level standalone nodes (actors, external systems)
# 2. Container hierarchy (outermost → innermost)
# 3. All connections grouped by semantic type
```

## Render Commands

```bash
# Dark + sketch (default)
d2 --sketch --theme 200 --layout elk input.d2 output.svg

# Light mode
d2 --theme 0 --layout elk input.d2 output.svg

# Watch mode (live preview while editing)
d2 --layout elk -w input.d2 output.svg

# Multi-board animation
d2 --layout elk --animate-interval 1200 input.d2 output.svg

# Formal / print (no sketch, PDF)
d2 --theme 0 --layout elk input.d2 output.pdf
```

Always output the render command after the code block.

## Key Rules

- **Connections always last** — mixing edge declarations with node declarations confuses ELK routing
- **Default to ELK** — always pass `layout-engine: elk` or `--layout elk` unless `sequence_diagram` is in use
- **`sequence_diagram` is the only exception** — D2 uses dagre internally for sequences regardless of config
- **Use strict UML notation when requested** — class/object/package/component diagrams should follow UML 2.5.1 semantics instead of generic architecture conventions
- **Classes over inline styles** — never repeat `fill`/`stroke`/`font-color` on individual nodes; define a class
- **One `class` per node** — D2 supports only one; use `class` for shape/dimensions, a `style` block for color overrides
- **Integer `stroke-width` only** — D2 v0.7.x rejects decimal values (use 1, 2, 3 — not 1.5)
- **Icons on rectangular shapes only** — hexagon, diamond, person, circle distort icons; use text-only labels
- **`label.near: outside-top-center`** on containers with `direction: right` to prevent title overlap with children
- **`multiple: true`** to represent N replicated instances without drawing each individually

## References

- **`references/d2-style-guide.md`** — Complete D2 syntax: all style properties, shape catalog, palette options and color semantics, icon library (40+ verified URLs), ELK config, grid layouts, animation patterns, composition/multi-board (layers/scenarios/steps), glob patterns. Read when you need syntax details or icon URLs.
- **`references/d2-examples.md`** — Full renderable examples for every diagram type: Architecture (L1–L3), Sequence, ER, Flowchart, Data Flow, Network/Security, State Machine, Org Chart, Class Diagram, Roadmap/Timeline. Read when starting any specific diagram type.
- **`references/uml-2.5.1-notation.md`** — UML 2.5.1 relationship notation quick reference, including association, aggregation, composition, generalization, realization, and dependency mapping into D2 arrowheads.
