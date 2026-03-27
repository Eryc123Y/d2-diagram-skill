# Sequence Diagram Checklist

Use this file before generating production-grade sequence diagrams.

Primary references:

- D2 official sequence diagram docs: https://d2lang.com/tour/sequence-diagrams/
- UML 2.5.1 spec: https://www.omg.org/spec/UML/2.5.1/PDF

This reference is not a full UML interaction manual. Its purpose is to make sure the generated diagram actually uses D2 sequence-diagram features correctly and reflects the most important UML-style interaction semantics.

## Required Checks

### 1. Use `shape: sequence_diagram`

Do not fake a sequence diagram with ordinary nodes and arrows.

```d2
Checkout flow: {
  shape: sequence_diagram
}
```

### 2. Predeclare actors in the intended left-to-right order

In D2 sequence diagrams, ordering matters. Declare participants explicitly before groups.

```d2
customer
web
api
payments
orders
```

Do this even if the first messages appear later inside groups.

### 3. Use groups for meaningful fragments

Groups are the closest thing to labeled interaction fragments in D2.

Use them for:

- happy path
- authorization flow
- webhook reconciliation
- failure / compensation branch
- retry path

Example:

```d2
authorize_payment: Authorize payment {
  api -> payments: authorize
  payments -> api: approved
}
```

### 4. Use spans for activation-like behavior

When one actor is actively processing work over multiple messages, use nested objects to show spans.

Example:

```d2
api.request -> payments.auth: authorize
payments.auth -> threeds.challenge: create challenge
threeds.challenge -> payments.auth: challenge url
payments.auth -> api.request: action required
```

Use spans when they clarify:

- in-flight work
- nested interactions
- callback lifetimes
- reconciliation / compensation scopes

### 5. Use notes for operational context

Notes are useful for production diagrams because they carry assumptions that do not belong in message labels.

Examples:

- idempotency key reuse
- reservation TTL
- webhook ordering caveats
- timeout windows
- retry semantics

Example:

```d2
checkout."Idempotency key is reused across retries for the same checkout attempt."
```

### 6. Respect definition order

D2 sequence diagrams render in the order you define things.

This means:

- write the messages in the order you want people to read them
- do not reorder later unless you want the visual sequence to change
- keep happy path and error path separated into labeled groups

### 7. Distinguish synchronous vs asynchronous messages

If a message is asynchronous or event-like, style it accordingly instead of pretending everything is a request/response pair.

Example:

```d2
payments.webhook -> api.reconcile: payment.authorized {
  style.stroke-dash: 5
}
```

Good candidates:

- webhooks
- event bus publications
- background notifications
- delayed callbacks

### 8. Prefer semantic clarity over message count

A production-grade sequence diagram is not just "more arrows".

It should make clear:

- who initiates the flow
- what the authoritative services are
- where decisions happen
- which parts are async
- where state is committed
- how compensation or rollback works

### 9. Keep failure handling explicit

For real production examples, include at least one clearly labeled exception or compensation path when it materially changes behavior.

Typical cases:

- payment authorization failure
- persistence failure after successful authorization
- stock release on compensation
- void / refund after downstream failure

### 10. Final review checklist

Before finalizing, verify:

- Is this truly a `sequence_diagram`, not a generic flow chart?
- Are participants explicitly declared in the intended order?
- Are major phases separated into groups?
- Are spans used where activation-like behavior matters?
- Are notes carrying operational assumptions?
- Are async callbacks styled distinctly?
- Is the failure / compensation path visible and readable?
