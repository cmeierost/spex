# Spex — Research Notes on Executable Specifications

> **The specification, read and signed by a human, should be the business product.**

Spex is a brainstorming experiment about a contradiction in modern spec-driven development: teams ask nondeterministic systems to generate business-critical code, then surround that code with tests, reviews, guardrails, sandboxes, and human oversight until it appears trustworthy.

In that sense, SDD copies the old human workflow, including its weakest part: the lossy, nondeterministic translation from specification to code. Software engineering has always suffered from drift between intent, implementation, tests, and infrastructure. AI does not remove that drift. It accelerates it.

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

The key boundary is this: **functional requirements belong in the spec; nonfunctional requirements shape the infrastructure around it.** Many teams call this "business logic vs infrastructure" without noticing that they are really separating business meaning from concerns such as durability, latency, availability, deployment, storage, integration, I/O, and human-computer interaction. The spec may define user intents, permissions, observable business state, allowed state modifications, state responsibilities, outside authorities, value constraints, transactions, and boundary ports. It should not define databases, APIs, primitive machine types, queues, deployment topology, scaling strategy, network protocols, widget state, or how a UI is rendered.

This is not presented as a wholly new idea. Spex builds on older attempts to separate functional requirements from nonfunctional requirements: business rules, permissions, and state meaning on one side; durability, latency, availability, deployment, storage, and integration concerns on the other. Many software practices talk about "business logic vs infrastructure" without noticing that this is the same boundary. Model-driven development, controlled natural language efforts such as Attempto, formal methods, and architectural traditions such as Clean Architecture all circle that problem from different directions.

The open question is whether LLMs can now help with the part that historically failed: making precise specifications writable by humans without letting probabilistic code generation become the source of truth.

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
