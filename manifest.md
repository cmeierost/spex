# The Spex Manifest

## 1. Executive Summary

This document captures the current working thesis behind Spex. It is not a claim that Spex already exists as a finished language or runtime. It is a research direction shaped by older attempts to separate business logic from infrastructure and to make specifications more exact.

The starting critique is that three existing practices remain structurally unsatisfying:

- **Traditional programming** — fragile lines of imperative code, maintained by humans, drifting from intent
- **Probabilistic AI code generation** — Kimi-style agents that automate the *writing* of code without solving the *verification* of code. Each patch compounds technical debt at superhuman speed. The result is a system no one can audit, no one can trust, and no one can legally sign off on
- **Manual cloud infrastructure configuration** — mixing physical deployment details with business logic

**The fundamental thesis:** The specification, read and signed by a human, should become the final product. The long-term goal is to reduce the gap between business meaning and execution so that generated application code is no longer the central artifact teams must review and trust.

The current target model is: a human signs a controlled specification, a solver checks it, a compiler lowers it into portable and distributable business-engine parts, and independent infrastructure binds to declared inputs, outputs, state responsibilities, outside authorities, and boundary ports. Infrastructure may decide deployment, storage, networking, and scaling, but it must not redefine the signed business meaning.

---

## 2. The Three Core Invariants

### I. Absolute Separation of Logic and Physics

**The Logical World (The Business):** Consists exclusively of the economic, operational, and legal rules of a system. It is immortal, does not age, and remains completely agnostic toward computers, databases, cloud architecture, or specific operating systems.

**The Physical World (The Machine):** Is a purely temporary transit vehicle. Servers, IP addresses, Docker containers, and SQL databases are infrastructure details around the portable business engine. They do not exist within the specification.

The deeper distinction is between **functional requirements** and **nonfunctional requirements**. Business rules, permissions, invariants, and state transitions belong to the logical world. Storage engines, transport protocols, replication strategies, scaling policies, and deployment topology belong to the physical world because they are implementations of nonfunctional requirements.

The same distinction applies to primitive datatypes. The specification should constrain value meaning such as ranges, precision, permitted states, and text length. The physical layer should choose machine-level representations such as integer width, decimal encoding, boolean storage, or database column types.

### II. The Contract is the Code (Single Source of Truth)

The specification should become the authoritative business artifact. The design goal is to reduce the need for separate requirement documents, duplicated business logic in application code, and large suites of tests that restate business rules indirectly. If something goes wrong, the first question should be whether the specification was incomplete, contradictory, or unclear.

### III. Purely Declarative Intent Governance (Intent-Driven UI)

The system should not need to manage business meaning through UI flags such as `button.disabled`. The design goal is for the interface to become a replaceable mirror of user capabilities, with the underlying model computing what an actor can and is allowed to do in a given state.

---

## 3. The technical fusion (The Mathematical Bridge)

The core design direction is to merge two historical extremes of computer science and the humanities into a single system:

```
[ HUMAN INTERFACE ]
High-Precision, Legal English (Controlled English)
                    │
                    ▼ (1:1 Isomorphism)
[ MATHEMATICAL FOUNDATION ]
Pure, Typed Lambda Calculus
```

**The Interface (Legal English):** Real-world legal contracts are high-precision, declarative rulebooks. A Spex-like specification would likely use a strict, controlled English grammar (conceptually evolving from Attempto Controlled English) that non-programmers (CEOs, business analysts, lawyers) can read, understand, and sign.

**The Foundation (Lambda Calculus):** The design goal is for every legal clause in the specification to map 1:1 to expressions in the Lambda Calculus. Because the Lambda Calculus is purely mathematical, stateless, and free of side effects, it offers the kind of deterministic semantics this direction depends on (for example, via Church-Rosser style confluence).

---

## 4. Operationalization: The System in Action

### The Interactive Design Loop (LLM as a Cognitive Interface)

Humans would not type application code directly. Authoring the specification would follow a three-part, closed-loop cycle:

1. **The Human** describes their business intent in free-form, imprecise everyday language.
2. **The LLM** acts as a precise translator, casting that messy intent into the strict, formal legal English of the specification.
3. **The Mathematical Compiler (Solver)** would verify the text for logical completeness. If it finds a blind spot or contradiction, it would feed the counterexample back to the LLM. The LLM would translate the mathematical failure into a simple clarifying question for the human (*"What should happen if X occurs while Y is still blocked?"*).

That clarification loop would also apply to value semantics. If the human says a field must be "exact," "short," or "large enough," the system should ask what exact range, precision, rounding rule, or length constraint is required rather than silently guessing a primitive type.

Once the mathematical solver reports "zero logical errors," the human would sign off on the text.

### State Responsibility and Portable Execution

**Writing is Infrastructure:** In the target model, executing an action only changes the state of the mathematical model. The specification may declare that state is persistent, forgettable, or provided by another named authority. The physical mechanism that preserves, discards, or observes that state belongs to the infrastructure around the portable business engine.

Those infrastructure choices would exist to satisfy nonfunctional requirements such as latency, durability, auditability, and resilience. They would not alter the business meaning of the specification.

The compiled business engine should also be **distributable**. The compiler may divide it into communicating parts when dependencies, transactions, state responsibilities, and boundary ports allow it. Infrastructure may deploy those parts in different places, but it must preserve the signed specification's meaning.

**Reading is Business Logic:** The specification would define exactly who can search and see what data fragments under which conditions using declarative filters (Views).

**Proactive Guidance over Reactive Error Messages:** Because the system would deeply understand the rules, the design aims to replace many reactive errors with proactive guidance. If a step is restricted, the UI could use the contract text to display which preconditions must be met to unlock it.

---

## 5. Contrast to the State of the Art (Why Others Fail)

| Approach | Why It Fails |
|----------|-------------|
| **Kimi AI / Devin / Agent Swarms** | Try to copy the craft of manual programming. Waste immense compute on probabilistic trial-and-error, generating unmaintainable AI legacy code. Spex instead asks whether business logic can move out of probabilistic code generation entirely. |
| **Model-Driven Architecture (MDA) of the 2000s** | Failed due to visual "spaghetti diagrams" and the roundtrip engineering trap, where generated code had to be manually edited. Spex explores a text-first approach where business meaning remains in the specification and technical implementation stays subordinate. |
| **Modern Cloud Frameworks** | Force developers to mix infrastructure declarations into code. In Spex, the business specification does not know which physical infrastructure surrounds the portable business engine. |

---

## 6. Closing statement

The ambition behind Spex is to push software engineering closer to direct execution of human agreements, without forcing business meaning to survive a long chain of lossy translations into application code. Whether that ambition can be realized remains the open question this repository explores.
