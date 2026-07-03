# The Three Core Invariants

These are the non-negotiable pillars of Spex. Every design decision, every grammar rule, and every architectural choice flows from these three invariants.

---

## Invariant I: Absolute Separation of Logic and Physics

**The logical world and the physical world never touch.**

### The Logical World (The Business)

The logical world consists exclusively of the economic, operational, and legal rules of a system. Its properties:

- **Immortal** — business rules don't expire when technology changes
- **Ageless** — a spec written today is equally valid in 10 years
- **Infrastructure-agnostic** — no mention of servers, databases, containers, APIs, or any physical construct
- **Human-readable** — written in controlled English that non-programmers understand

### The Physical World (The Machine)

The physical world is a purely temporary transit vehicle. Its properties:

- **Disposable** — servers, IPs, Docker containers, SQL databases are implementation details
- **Invisible** — none of these concepts appear in the specification
- **Managed by the runtime** — the universal interpreter handles all physical concerns

### Why This Matters

Every time a spec mentions a database, an API endpoint, or a cloud service, it couples business logic to a technology that will become obsolete. Spex forbids this coupling at the grammar level.

---

## Invariant II: The Contract is the Code

**There is one artifact and one artifact only: the specification.**

### What Gets Eliminated

| Traditional Artifact | Spex Replacement |
|---------------------|------------------|
| Requirement documents | The spec itself |
| Jira tasks / user stories | The spec itself |
| Source code files | The spec itself |
| Test suites | The mathematical solver's completeness check |
| API documentation | The spec itself |
| Architecture diagrams | The spec itself |

### The Implication

If something goes wrong within the system, the specification was defined incorrectly. There is no "the spec was right but the code was wrong." There is no code.

---

## Invariant III: Purely Declarative Intent Governance

**The system computes what an actor can do; the UI merely reflects it.**

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

Crashes and runtime exceptions disappear. The system pre-computes the path to a user's goal. If a step is restricted, the UI displays the contract text explaining exactly which preconditions must be met.
