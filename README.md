# SPEX — Executable Specifications

> **The specification, read and signed by humans, should be the business product.**

SPEX is a proposal for a research project exploring whether business logic can be written in a restricted, computable form of English that is precise enough to analyse and compile deterministically, yet readable enough to discuss and approve directly with domain experts.

Business specifications often appear clear in ordinary language while remaining too imprecise to determine behaviour unambiguously. When they are translated into software, information is lost, gaps are filled, and assumptions are added.

AI-assisted development does not remove this translation problem. It performs the same translation faster, more frequently, and at a larger scale.

SPEX explores a different approach: make the reviewed specification the single source of truth and compile the specified business behaviour deterministically. AI remains part of the engineering process, but it is not responsible for repeatedly reconstructing business logic from informal documents and existing code.

## The Core Idea

During specification, an LLM acts primarily as an interviewer and discussion partner. It helps engineers and domain experts express their knowledge, identify unclear assumptions, and formulate business rules.

The compiler and solver provide the formal foundation. They identify statements that are incomplete, ambiguous, inconsistent, syntactically invalid, or insufficiently precise.

Using these diagnostics, the LLM can:

* ask focused follow-up questions;
* help transform informal statements into valid SPEX statements;
* explain why a statement is incomplete or invalid;
* propose reformulations without silently deciding the intended meaning.

The human remains in control. Every reformulated statement must be reviewed and accepted before it becomes part of the specification.

Concrete examples are discussed with domain experts to confirm that the written rules reflect their intent. These examples resemble unit tests, but their primary purpose during specification is to uncover misunderstandings. The same examples can later be reused as integration tests against the complete application.

Once accepted, the specification is compiled without AI into portable and distributable business-engine parts. Engineers and LLMs can then design and implement the surrounding infrastructure against declared interfaces and measurable non-functional requirements.

```text
Human intent
  ↔ LLM as interviewer and drafting partner
  ↔ controlled specification text
  ↔ compiler and solver feedback
      gaps, contradictions, invalid statements, counterexamples
  ↔ human review and confirmation

When the specification is accepted:

Signed business contract
  → deterministic compiler
  → portable business-engine parts
  → infrastructure designed by engineers and LLMs
  → complete application
```

## Why This Matters

Modern [spec-driven development](./comparison/why-sdd-fails.md) asks a non-deterministic system to generate business-critical code and then surrounds that code with tests, reviews, guardrails, sandboxes, and human oversight until it appears trustworthy.

This reproduces the traditional software problem at higher speed: translating an insufficiently precise specification into code while silently adding assumptions.

SPEX explores the opposite direction:

> What if the signed specification were the authoritative business artefact, and generated application code no longer had to be continuously reviewed for business correctness?

A precise specification and deterministic compiler could reduce:

* repeated, token-intensive analysis of specifications and codebases;
* dependence on increasingly powerful and expensive frontier models;
* unnecessary energy and hardware consumption;
* exposure of sensitive business knowledge to external model providers;
* the review burden created by untrusted generated business logic.

Smaller or locally operated models could still assist with specification and infrastructure work because the compiler provides precise, structured feedback instead of requiring the model to rediscover the entire system repeatedly.

## The Architectural Boundary

The central boundary is between **specified business meaning** and the **technical mechanisms used to realise it**.

Functional requirements define what the system means and which behaviour is allowed. Non-functional requirements shape how that behaviour is stored, distributed, rendered, integrated, secured, and operated.

| Specified business logic                                    | Derived non-functional implementation                         |
| ----------------------------------------------------------- | ------------------------------------------------------------- |
| User intents, permissions, and business decisions           | Databases, APIs, and integration adapters                     |
| Observable business state and allowed state changes         | Machine types, encodings, and persistence formats             |
| State ownership, responsibilities, and external authorities | Queues, deployment topology, and scaling                      |
| Value constraints, invariants, and transactions             | Availability, latency, durability, and security mechanisms    |
| Declared inputs, outputs, and boundary ports                | Network protocols and communication technologies              |
| Intent-driven UI state                                      | Widgets, rendering engines, and platform-specific interaction |

SPEX is also intended to support intent-driven UI specifications. The specification can define observable values, permitted user intents, and valid state changes. These can be bound to different rendering technologies without placing widgets or presentation mechanics inside the deterministic business core.

## Current Status

> There is no finished SPEX language yet, no stable grammar, and no complete solver or execution target.

An early compiler already exists. It has an extensive type system, can compile simple problems, and is expected to be released as open source in the near future.

This repository contains research notes, comparisons, candidate grammar patterns, examples, and architecture sketches from which the language and its tooling may emerge.

The current model is deliberately not a simple specification-to-code pipeline. It combines a human-controlled authoring loop with a deterministic execution handoff.

## Foundations and Related Work

SPEX is not presented as an entirely new idea. It builds on earlier attempts to separate business meaning from technical implementation and to make requirements formally analysable.

Related areas include:

* [model-driven development](./comparison/related-work.md#model-driven-architecture-mda);
* [controlled natural language](./comparison/related-work.md#controlled-english--natural-language), including [Attempto Controlled English](https://www.ace-editor.org/) and [Logical English](https://logicalcontracts.com/logical-english/);
* [formal specification languages and tools](./comparison/related-work.md#specification-languages--tools);
* [Rules as Code and computable law](./comparison/related-work.md#rules-as-code--computable-law);
* architectural boundaries such as [Clean Architecture](./comparison/related-work.md#robert-c-martin--clean-architecture) and [Hexagonal Architecture](./comparison/related-work.md#alistair-cockburn--hexagonal-architecture).

See [Related Work](./comparison/related-work.md) and [Further Reading](./reference/further-reading.md) for the broader research map.

The central open question is whether LLMs can now help solve the part that historically failed: making precise specifications practical for humans to write without allowing probabilistic code generation to become the source of truth.

## Start Here

1. Read the [Manifest](./manifest.md) for the current thesis and research posture.
2. Read [Logical vs Physical World](./concepts/logical-vs-physical.md) for the functional and non-functional boundary.
3. Read [Three Core Invariants](./concepts/three-invariants.md) for the foundational principles.
4. Read [Related Work](./comparison/related-work.md) for the ideas and systems on which SPEX builds.

## Knowledge Base

### [Manifest](./manifest.md)

The current working thesis, motivation, and research posture.

### [Concepts](./concepts/)

Core ideas and principles:

* [Overview](./concepts/overview.md) — The problem SPEX addresses
* [Three Core Invariants](./concepts/three-invariants.md) — The foundational principles
* [Logical vs Physical World](./concepts/logical-vs-physical.md) — Separation of business meaning and implementation
* [Contract Is Code](./concepts/contract-is-code.md) — The specification as the single source of truth
* [Intent-Driven UI](./concepts/intent-driven-ui.md) — Declarative intent and observable state

### [Grammar](./grammar/)

Candidate controlled-English syntax and language patterns:

* [Grammar Overview](./grammar/overview.md) — Candidate language structure
* [Keywords](./grammar/keywords.md) — Formal keywords and grammatical patterns

### [Architecture](./architecture/)

Design sketches for solver-guided authoring, formal semantics, portable execution, and state responsibility:

* [Mathematical Bridge](./architecture/mathematical-bridge.md) — Connecting formal semantics and controlled English
* [Interactive Design Loop](./architecture/design-loop.md) — The LLM as a cognitive interface
* [Portable Execution Target](./architecture/execution-target.md) — Distributable business-engine parts with declared inputs and outputs
* [Persistence Model](./architecture/persistence.md) — Why writing state is an infrastructure concern

### [Examples](./examples/overview.md)

Candidate specifications, thought experiments, and possible evaluation scenarios.

### [Comparison](./comparison/)

* [Why Spec-Driven Development Fails](./comparison/why-sdd-fails.md) — The unresolved translation problem
* [State of the Art](./comparison/state-of-the-art.md) — Comparison with current approaches
* [Related Work](./comparison/related-work.md) — Prior art and adjacent research

### [Reference](./reference/overview.md)

Source documents, prior work, and external material collected during the research.
