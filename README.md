# Spex — Research Notes on Executable Specifications

> **The specification, read and signed by a human, should be the business product.**

Spex is a brainstorming experiment about a contradiction in modern [spec-driven development](./comparison/why-sdd-fails.md): teams ask nondeterministic systems to generate business-critical code, then surround that code with tests, reviews, guardrails, sandboxes, and human oversight until it appears trustworthy.

In that sense, [SDD](./comparison/sdd-rebuttal.md) copies the traditional human software development workflow, including its weakest part: the lossy, nondeterministic translation from specification to code. Software engineering has always suffered from drift between intent, implementation, tests, and infrastructure. AI does not remove that drift. It accelerates it, making the old translation problem faster, larger, and harder to audit.

Spex explores the opposite direction. What if the signed specification were the authoritative business artifact, and generated application code stopped being the thing teams must continuously review for business correctness?

> There is no Spex language yet, no finished grammar, no solver, and no implemented execution target. This repository collects the research notes, comparisons, candidate grammar patterns, and architecture sketches for forming that idea.

The current target model is not a straight pipeline. It has an authoring loop and an execution handoff:

```text
Human intent
  ↔ LLM as interviewer and drafting partner
  ↔ controlled specification text
  ↔ solver feedback: gaps, contradictions, counterexamples

When the solver accepts the spec and the human signs it:

Signed business contract
  → compiler lowers it into portable, distributable business-engine parts
  → software engineer and LLM design infrastructure to satisfy nonfunctional requirements
  → infrastructure architecture determines how business-engine parts are distributed through declared ports
```

The key boundary is this: **functional requirements define the specified business logic; nonfunctional requirements shape the infrastructure and interaction mechanisms that support it.** Many teams call this "business logic vs infrastructure" without noticing that they are really separating business meaning from durability, latency, availability, deployment, storage, integration, I/O, and human-computer interaction. Those concerns may derive from the specification, but they must remain separated from the specified logic, both in the design and in the running application.

| Specified business logic | Derived nonfunctional implementation |
|--------------------------|--------------------------------------|
| User intents and permissions | Databases and APIs |
| Observable business state and allowed state modifications | Primitive machine types and storage encodings |
| State responsibilities and outside authorities | Queues, deployment topology, and scaling strategy |
| Value constraints, transactions, and boundary ports | Network protocols, widget state, and UI rendering |

This is not presented as a wholly new idea. Spex builds on older attempts to separate functional requirements from nonfunctional requirements: business rules, permissions, and state meaning on one side; durability, latency, availability, deployment, storage, and integration concerns on the other. Many software practices talk about "business logic vs infrastructure" without noticing that this is the same boundary. The closest aligning ideas include [model-driven development](./comparison/related-work.md#model-driven-architecture-mda), [controlled natural language](./comparison/related-work.md#controlled-english--natural-language) such as [Attempto Controlled English](https://www.ace-editor.org/) and [Logical English](https://logicalcontracts.com/logical-english/), [formal specification](./comparison/related-work.md#specification-languages--tools), [Rules as Code / computable law](./comparison/related-work.md#rules-as-code--computable-law), and architecture boundaries such as [Clean Architecture](./comparison/related-work.md#robert-c-martin--clean-architecture) and [Hexagonal Architecture](./comparison/related-work.md#alistair-cockburn--hexagonal-architecture). See [Related Work](./comparison/related-work.md) and [Further Reading](./reference/further-reading.md) for the full map.

The open question is whether LLMs can now help with the part that historically failed: making precise specifications writable by humans without letting probabilistic code generation become the source of truth.

## Start Here

- Read the [manifest](./manifest.md) for the current thesis.
- Read [Logical vs Physical World](./concepts/logical-vs-physical.md) for the functional/nonfunctional boundary.
- Read [Related Work](./comparison/related-work.md) to see the older attempts Spex builds on and departs from.

## Knowledge Base Structure

### [Manifest](./manifest.md)
The current working thesis and research posture.

### [Concepts](./concepts/)
Core ideas and principles:
- [Overview](./concepts/overview.md) — What problem does Spex solve?
- [Three Core Invariants](./concepts/three-invariants.md) — The foundational pillars
- [Logical vs Physical World](./concepts/logical-vs-physical.md) — Absolute separation
- [Contract is Code](./concepts/contract-is-code.md) — Single source of truth
- [Intent-Driven UI](./concepts/intent-driven-ui.md) — Declarative intent governance

### [Grammar](./grammar/)
Candidate grammar and controlled-English patterns:
- [Grammar Overview](./grammar/overview.md) — Controlled English syntax
- [Keywords](./grammar/keywords.md) — Formal keywords and patterns

### [Architecture](./architecture/)
Design sketches for solver-guided authoring, formal semantics, portable execution, and state responsibility:
- [Mathematical Bridge](./architecture/mathematical-bridge.md) — Lambda calculus + legal English fusion
- [Interactive Design Loop](./architecture/design-loop.md) — LLM as cognitive interface
- [Portable Execution Target](./architecture/execution-target.md) — Compiler-produced, distributable business-engine parts with declared inputs and outputs
- [Persistence Model](./architecture/persistence.md) — Writing is infrastructure

### [Examples](./examples/overview.md)
Sample specifications, thought experiments, and possible evaluations once the design is concrete enough.

### [Comparison](./comparison/)
- [State of the Art](./comparison/state-of-the-art.md) — Why Kimi, MDA, and modern cloud frameworks fall short

### [Reference](./reference/overview.md)
Source documents, prior art, and external materials collected during the research.
