# The Three Core Invariants

These are the current design invariants behind SPEX. They are not presented as properties of a finished system, but as the constraints the research direction is trying to preserve.

---

## Invariant I: Absolute Separation of Logic and Physics

**The logical world and the physical world should remain strictly separated.**

### The Logical World (The Business)

The logical world consists exclusively of the economic, operational, and legal rules of a system. Its properties:

- **Immortal** — business rules don't expire when technology changes
- **Ageless** — a spec written today is equally valid in 10 years
- **Infrastructure-agnostic** — no mention of servers, databases, containers, APIs, or any physical construct
- **Human-readable** — ideally written in a controlled form that non-programmers understand

### The Physical World (The Machine)

The physical world is the implementation layer that carries nonfunctional requirements. Its properties:

- **Disposable** — servers, IPs, Docker containers, SQL databases are implementation details
- **Invisible** — none of these concepts appear in the specification
- **Runtime-managed** — a runtime or infrastructure layer would handle the physical concerns

### Why This Matters

Every time a spec mentions a database, an API endpoint, or a cloud service, it couples business logic to an implementation of nonfunctional requirements. The SPEX direction tries to forbid this coupling at the grammar level.

---

## Invariant II: The Contract is the Code

**The specification should be the authoritative business artifact.**

### What Gets Eliminated

| Traditional Artifact | SPEX Replacement |
|---------------------|------------------|
| Requirement documents | The spec itself |
| Jira tasks / user stories | The spec itself |
| Source code files | The spec itself |
| Test suites | The mathematical solver's completeness check |
| API documentation | The spec itself |
| Architecture diagrams | The spec itself |

### The Implication

If something goes wrong within the system, the first question should be whether the specification was incomplete, contradictory, or unclear. The goal is not to pretend implementation defects cannot exist. The goal is to stop business correctness from being defined primarily in application code.

---

## Invariant III: Purely Declarative Intent Governance

**The system should compute what an actor can do; the UI should merely reflect it.**

### What This Is NOT

- Managing pixels or visual states
- Setting `button.disabled = true`
- Defining UI components or layouts
- Writing event handlers for clicks

### What This IS

- Declaring what actions an actor is permitted to perform in each state
- Letting the system proactively compute available actions
- Rendering a "dumb" UI that is a replaceable mirror of capability

### The Result

In the target model, many reactive UI errors would be replaced by proactive guidance. If a step is restricted, the UI would display contract-derived explanations of which preconditions must be met.
