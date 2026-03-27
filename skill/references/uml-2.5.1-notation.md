# UML 2.5.1 Notation Reference

Use this file when the user asks for UML specifically, or when relationship correctness matters more than freeform architecture styling.

Primary references:

- OMG UML 2.5.1 specification: https://www.omg.org/spec/UML/2.5.1/PDF
- OMG UML overview page: https://www.omg.org/spec/UML/
- D2 UML classes: https://d2lang.com/tour/uml-classes/
- D2 connections and arrowheads: https://d2lang.com/tour/connections

## Scope

This reference is most relevant for:

- class diagrams
- object diagrams
- package diagrams
- component diagrams
- deployment diagrams when UML notation is explicitly requested

For cloud architecture, infra topology, and product/system landscape diagrams, the main `SKILL.md` defaults are usually more appropriate than strict UML.

## Core Relationship Semantics

### Association

- Meaning: structural relationship between classifiers.
- Notation: solid line.
- Navigability: optional arrowhead on the navigable end.
- Multiplicity, role names, and end labels belong on association ends, not in the middle of the line.

Use this when objects know about each other, collaborate, or hold references.

Minimal D2 pattern:

```d2
Order -- Customer
```

Directed association:

```d2
Order -> Customer
```

### Aggregation

- Meaning: shared whole-part relationship.
- Notation: hollow diamond at the aggregate end.
- Use sparingly. If the lifecycle semantics are weak or ambiguous, plain association is often better.

D2 pattern:

```d2
Team -> Player: has members {
  source-arrowhead.shape: diamond
  source-arrowhead.style.filled: false
}
```

If the aggregate is on the right side, put the diamond on `target-arrowhead` instead.

### Composition

- Meaning: strong whole-part relationship with owned lifecycle.
- Notation: filled diamond at the composite end.
- Use when the part conceptually belongs to exactly one whole and usually does not outlive it independently.

D2 pattern:

```d2
Order -> OrderLine: contains {
  source-arrowhead.shape: diamond
  source-arrowhead.style.filled: true
}
```

### Generalization

- Meaning: inheritance / "is-a" specialization.
- Notation: solid line with hollow triangle pointing to the general classifier.

D2 pattern:

```d2
AdminUser -> User {
  target-arrowhead.shape: triangle
  target-arrowhead.style.filled: false
}
```

### Realization

- Meaning: implementation of an interface or contract.
- Notation: dashed line with hollow triangle pointing to the specification.

D2 pattern:

```d2
PaymentService -> PaymentProvider {
  style.stroke-dash: 5
  target-arrowhead.shape: triangle
  target-arrowhead.style.filled: false
}
```

### Dependency

- Meaning: usage or reliance without structural ownership.
- Notation: dashed line with open arrow.

D2 pattern:

```d2
ReportController -> PdfRenderer {
  style.stroke-dash: 5
}
```

Use dependency for temporary use, import/use, or "calls into" relations when association would overstate structural coupling.

## Practical Guidance

### Prefer the simplest correct relation

If you are unsure between aggregation and association, prefer association.

If you are unsure between dependency and association:

- choose **association** for stable structural links or held references
- choose **dependency** for temporary use, method-call reliance, or imports

If you are unsure between aggregation and composition:

- choose **composition** only when lifecycle ownership is clearly strong
- otherwise choose **association** or, less often, **aggregation**

### Keep UML diagrams semantic, not decorative

- Avoid random icons and infra-specific shapes unless they help rather than dilute the notation.
- Use consistent multiplicity and role-end placement.
- Do not mix sequence-diagram semantics into class/object diagrams.

### Use D2's UML support where it fits

For class diagrams, prefer `shape: class` and UML-style member visibility:

```d2
User: {
  shape: class
  +id: UUID
  -passwordHash: string
  +login(email: string, password: string): boolean
}
```

## Relationship Checklist

Before finalizing a UML diagram, verify:

- Is each relationship type semantically justified?
- Are diamonds on the correct whole side?
- Are hollow vs filled diamonds correct?
- Is inheritance using a hollow triangle, not a plain arrow?
- Is realization dashed while generalization is solid?
- Are multiplicities and role names attached to the ends they describe?
