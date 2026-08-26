# SPEX — Executable Specifications

For a concise introduction of what SPEX is supposed to become, see
[What is SPEX supposed to become?](./description-en.md) or
[Was soll SPEX werden?](./description-de.md).

## The problem of specifying software behaviour

Some software has no formal specification at all; its “specification” is scattered across emails, handwritten meeting notes, and informal conversations. Even higher-quality business specifications may appear clear in ordinary language while remaining too imprecise to determine behaviour unambiguously. When they are translated into software, information is lost, gaps are filled, and assumptions are added. The resulting code is normally inaccessible to people without a programming background, while domain experts cannot be sure that the software actually does what they intended. This is not a new problem: the software industry has struggled with it since the earliest computers.

That makes two old questions more important than ever:

> What exactly should the software do?

and

> How do we know that the software actually does exactly that?

For any particular piece of behaviour, consider three simplified cases:

- **We do not care about the behaviour.** A team may deliberately leave it open to a human or AI implementer.
- **The specification is incomplete.** Whoever implements the missing behaviour has to guess. We cannot establish that software matches an intended business behaviour that was never defined.
- **The specification is precise.** There should be nothing left to interpret, and asking a probabilistic model to interpret it again creates an avoidable opportunity for semantic drift.

You cannot expect even the smartest AI to implement exactly what you intended if you never expressed exactly what you intended.

AI-assisted development does not remove this translation problem. It performs the same translation faster, more frequently, and at a larger scale. Tests, guardrails, and reviews may detect incorrect outcomes, but they cannot establish intent that was never defined. Nor does agreement between generated code and independently generated tests prove that either one preserves the meaning of the specification. More capable LLMs might guess better, but they do not remove the need for a precise, human-verifiable definition of behaviour that must not be guessed.

## Retaining human authority

Allowing an LLM to infer both the specification and its implementation would leave essential business decisions without a human-verifiable source of authority.

We must distinguish between the work AI may perform and the business meaning humans must retain, review, and control.

**Which decisions are we willing to let AI infer — and which must remain explicitly defined by humans?**

At some point, the question is no longer whether AI can implement our decisions. It is whether we still want the decisions to be ours.

> If we decide that we no longer need to know exactly what such software is supposed to do, we are not merely delegating programming — we are delegating the decisions themselves.

> **The specification, read and approved by humans, should be the authoritative business artefact.**

This repository prepares a research proposal investigating whether business logic can be expressed in a restricted, computable form of English: precise enough for formal analysis and deterministic compilation, yet readable enough to be discussed and approved by domain experts.

## The core idea of SPEX

Modern [spec-driven development](https://arxiv.org/pdf/2602.00180) commonly uses a non-deterministic system to transform specifications into plans, tasks, business-critical code, and tests, then relies on reviews, guardrails, and human oversight to establish trust. Each transformation interprets the previous representation again. SPEX investigates whether an approved specification can instead become precise, authoritative, and deterministically executable.

The two-part article develops the argument and the proposed response: [Spec-Driven Development Gets Your Spec Wrong](./articles/spec-driven-development-gets-your-spec-wrong.md) and [SPEX: Executable Specifications for AI-Assisted Development](./articles/spex-executable-specifications-for-ai-assisted-development.md). The earlier research analysis remains available in [Why Spec-Driven Development Fails](./sdd/why-sdd-fails.md).

**SPEX explores a different approach**:

> What if the approved specification were the authoritative business artefact, while generated code became a derived execution artefact rather than a separately maintained source of business logic?

If the specification already defines the intended behaviour precisely, there should be nothing left to interpret. So why ask an LLM to reinterpret it into code — and then build tests, reviews, and guardrails to check whether that interpretation was correct?

> At that point, interpretation is no longer a strength.
>
> It is the problem.

This requires more than well-written natural language. A specification needs definitions, not impressions, and every accepted language construct needs defined semantics. To translate a language with formally defined semantics, software engineering already has a technology: a compiler. Given the same accepted specification, every conforming compiler must produce semantically equivalent executable business behaviour.

AI remains part of the engineering process and accelerates it, but it is not responsible for repeatedly reconstructing business logic from informal documents and existing code.
During specification, an LLM acts primarily as an interviewer and discussion partner. It helps engineers and domain experts express their knowledge, identify unclear assumptions, and formulate business rules.

The compiler would check syntax, types, references, and forms of structural completeness defined by the language. Solvers and other formal-analysis tools could identify contradictions, violated invariants, unreachable cases, and counterexamples within the declared model. These diagnostics expose where the specification cannot yet be accepted; they cannot decide what the business intended.

Using these diagnostics, the LLM can:

* ask focused follow-up questions;
* help transform informal statements into valid SPEX statements;
* explain why a statement is incomplete or invalid;
* propose reformulations without silently deciding the intended meaning.

The LLM may propose the specification. It does not decide the required behaviour, and it does not define the language semantics. Every proposed reformulation must remain readable to the responsible reviewer and must be reviewed and accepted before becoming authoritative. The compiler can establish validity within the language semantics; only a human can confirm that a statement expresses the intended business meaning. Parts of a specification may therefore remain explicitly proposed, developer-reviewed, or domain-approved. Changing approved behaviour invalidates that approval and requires renewed review.

Once that precise form has been accepted, probabilistic interpretation should stop for the behaviour it defines. The specification becomes the authoritative source for that business behaviour — a shared language between the people who define it and the people who implement the system.

Concrete, executable examples are discussed with domain experts to reveal misunderstandings and confirm that the formal rules express their intent. The examples do not replace or complete the specification; they validate shared understanding of it. The same examples can later be reused as integration tests against the completed application.

### Figure 1: AI-assisted specification

```mermaid
flowchart LR
    H["Human<br/>intent, answers, approval"]
    L["LLM<br/>interviewer and drafting partner"]
    S["Controlled<br/>SPEX specification"]
    C["Compiler and solver<br/>analysis"]
    F["Diagnostics<br/>gaps · contradictions · invalid statements · counterexamples"]
    V{"Valid and<br/>human-approved?"}
    A["Approved business specification"]

    H -->|"Expresses intent"| L
    L -->|"Drafts or reformulates"| S
    S -->|"Analysed deterministically"| C
    C --> F
    F -->|"Precise feedback"| L
    L -->|"Questions and proposals"| H

    S --> V
    H -.->|"Approves meaning"| V
    V -->|"Yes"| A
    V -->|"No"| L
```

Once accepted, the specification would be compiled without AI into portable business-engine parts with declared inputs, outputs, state responsibilities, and boundary ports.

The infrastructure would not need to be reviewed for its interpretation of business behaviour. Instead, it would be evaluated against declared interfaces, integration tests, and measurable non-functional requirements such as latency, availability, durability, security, and throughput.

This does not eliminate verification. The compiler and runtime must themselves be tested — and, where assurance demands it, verified — to establish that they preserve the semantics defined by SPEX. It replaces repeated, application-by-application probabilistic interpretation with a reusable language and toolchain trust boundary.

### Figure 2: Deterministic execution and infrastructure

```mermaid
flowchart LR
    A["Approved business specification"]
    C["Deterministic compiler"]
    B["Portable business-engine parts<br/>with declared ports"]
    I["Infrastructure designed by<br/>engineers and LLMs"]
    APP["Complete application"]

    A --> C
    C --> B
    B -->|"Declares required ports"| I
    B --> APP
    I --> APP
```

## Why this matters

Compared with LLM-based specification-to-code workflows, a precise specification and deterministic compiler could reduce:

- repeated, token-intensive analysis of specifications and codebases;
- the need to keep separately maintained specifications, implementations, and tests semantically synchronised;
- unnecessary energy and hardware consumption;
- exposure of sensitive business knowledge to external model providers;
- review effort caused by untrusted generated business logic;
- dependence on increasingly powerful and expensive frontier models.

Smaller or locally operated models could still assist with specification and infrastructure work because the compiler provides precise, structured feedback instead of requiring the model to rediscover the entire system repeatedly.

SPEX can be understood as a rigorous form of spec-driven development: it takes the specification seriously enough to require it to be human-authoritative, formally analysable, and deterministically compilable. If those conditions can be achieved, transferring the specification into executable business behaviour should not require an LLM.

The proposed SPEX model is intended to address three structural weaknesses of LLM-based specification-to-code workflows:

- **Unverified interpretation**: The generated implementation may contain assumptions, decisions, or interpretations that nobody explicitly reviewed or approved.
- **Invisible specification in tests**: When the written specification is incomplete, teams encode missing business behaviour in tests. Those tests become an implicit, code-oriented part of the specification that domain experts may never read or approve. SPEX instead uses reviewed examples to expose misunderstandings and confirm that the formal specification expresses the intended meaning, without allowing the examples to define missing rules.
- **Unverified verification**: Because teams do not trust an LLM to translate the specification correctly, they encode the expected behaviour again in tests and guardrails. This creates a second transformation — from the specification into test code — which must itself be reviewed to ensure that it expresses the same meaning. The tests may detect some incorrect implementations, but ordinary testing cannot establish that the generated code is semantically equivalent to the complete specification. Trust is therefore shifted to another manually maintained artefact rather than established by the transformation itself.

This leaves a question that test generation alone cannot answer:

> Who or what verifies that the tests themselves express what the specification says?

If accepted business behaviour is compiled deterministically, tests no longer need to compensate for a probabilistic translation of that already-defined behaviour. They remain valuable for validating examples, integration, regressions, and measurable technical requirements.

## The architectural boundary

The central boundary is between **specified business meaning** and the **technical mechanisms used to realise it**. It is not a boundary between software that AI may and may not write; it separates decisions that humans have already made from implementation choices that remain open.

There is a fundamental difference between:

> *what behaviour the system must guarantee*

and

> *how that behaviour is implemented.*

Functional requirements define what the system means and which behaviour is allowed. Non-functional requirements shape how that behaviour is stored, distributed, rendered, integrated, secured, and operated.

| Authoritative business specification                        | Derived technical realisation                                 |
| ----------------------------------------------------------- | ------------------------------------------------------------- |
| User intents, permissions, and business decisions           | Databases, APIs, and integration adapters                     |
| Observable business state and allowed state changes         | Machine types, encodings, and persistence formats             |
| State ownership, responsibilities, and external authorities | Queues, deployment topology, and scaling                      |
| Value constraints, invariants, and transactions             | Availability, latency, durability, and security mechanisms    |
| Declared inputs, outputs, and boundary ports                | Network protocols and communication technologies              |
| Intent-driven UI state                                      | Widgets, rendering engines, and platform-specific interaction |

SPEX is also intended to support intent-driven UI specifications. The specification can define observable values, permitted user intents and valid state changes. These can be bound to different rendering technologies without placing widgets or presentation mechanics inside the deterministic business core.

Source code itself can be the specification, but then it becomes the only source of truth for that behaviour. That can work when the people responsible for the business behaviour can reliably review the complete implementation. When they cannot, they need another representation they can understand — and the organisation is again maintaining specification and code while asking how it knows that both still mean the same thing.

SPEX explores whether that second representation can instead be the source language of a protected business core. Required behaviour should not be reinterpreted, while the surrounding technical implementation remains flexible within its defined constraints. AI can be extremely useful for finding good ways to implement guarantees that have already been defined; those guarantees themselves do not have to be left for AI to decide.

## Current status

> There is no finished SPEX language yet, no stable grammar, and no complete solver or execution target.

An exploratory compiler prototype already implements parts of the proposed type system and can compile a limited set of example specifications into TypeScript intended to serve as the production business core. It demonstrates that selected ideas are implementable, but it does not yet validate the complete research hypothesis. Its open-source release depends on the resources available to the project.

This repository contains research notes, comparisons, grammar ideas, examples, and architectural sketches from which the language and its tooling may emerge.

The current model is deliberately not a simple specification-to-code pipeline. It combines a human-controlled, compiler-driven and AI-assisted authoring loop with a deterministic execution handoff.

The challenge is not inventing executable specifications; they have existed for decades. The challenge is making them practical enough for everyday software development: precise enough to compile, natural enough to read, and easy enough to write with AI assistance.

## Proposed evaluation

The research should investigate whether:

- the language can express realistic business rules without relying on hidden implementation assumptions;
- compiler and solver diagnostics expose the missing cases, contradictions, invalid statements, and insufficiently precise rules that can be derived from the defined language semantics;
- domain experts can understand, discuss, and approve SPEX specifications;
- LLM-assisted interviewing and reformulation reduce the effort required to produce valid specifications;
- humans can reliably verify that LLM-proposed reformulations preserve their intended meaning;
- explicit approval states and traceability remain usable as specifications evolve;
- executable examples help uncover misunderstandings during specification;
- the compiler and runtime can be shown to preserve the defined SPEX semantics;
- smaller or less capable language models can achieve specification results comparable to frontier models when guided by precise compiler diagnostics.

Models would be compared by compiler acceptance, domain-expert acceptance, unsupported assumptions introduced, clarification and correction steps, human effort, token usage, execution time, and computational cost.

## Foundations and related work

SPEX is not presented as an entirely new idea. It builds on earlier attempts to separate business meaning from technical implementation and to make requirements formally analysable. It also builds on earlier attempts to make controlled or simplified forms of English formally interpretable and executable.

Related areas include:

* [model-driven development](./comparison/related-work.md#model-driven-architecture-mda);
* [controlled natural language](./comparison/related-work.md#controlled-english--natural-language), including [Attempto Controlled English](https://www.ace-editor.org/) and [Logical English](https://logicalcontracts.com/logical-english/);
* [formal specification languages and tools](./comparison/related-work.md#specification-languages--tools);
* [Rules as Code and computable law](./comparison/related-work.md#rules-as-code--computable-law);
* architectural boundaries such as [Clean Architecture](./comparison/related-work.md#robert-c-martin--clean-architecture) and [Hexagonal Architecture](./comparison/related-work.md#alistair-cockburn--hexagonal-architecture).

See [Related Work](./comparison/related-work.md) and [Further Reading](./reference/further-reading.md) for the broader research map.

The central challenge is whether compiler-guided LLM assistance can make precise specifications practical for humans to write without allowing probabilistic systems to become the authority over business meaning.

The principle is simple:

> *Let AI interpret what is still open.*
>
> *Compile what has already been decided.*

## Start here

1. Read [Spec-Driven Development Gets Your Spec Wrong](./articles/spec-driven-development-gets-your-spec-wrong.md) for the problem SPEX is intended to address.
2. Read [SPEX: Executable Specifications for AI-Assisted Development](./articles/spex-executable-specifications-for-ai-assisted-development.md) for the proposed language and workflow.
3. Read the [Manifest](./manifest.md) for the current thesis and research posture.
4. Read [Logical vs Physical World](./concepts/logical-vs-physical.md) and [Three Core Invariants](./concepts/three-invariants.md) for the architectural boundary and foundational principles.
5. Read [Related Work](./comparison/related-work.md) for the ideas and systems on which SPEX builds.

## Knowledge base

### [Articles](./articles/)

The clearest current presentation of the motivation and proposed approach:

* [Spec-Driven Development Gets Your Spec Wrong](./articles/spec-driven-development-gets-your-spec-wrong.md) — Why current AI-oriented SDD retains a semantic gap
* [SPEX: Executable Specifications for AI-Assisted Development](./articles/spex-executable-specifications-for-ai-assisted-development.md) — The proposed language, workflow, and execution boundary

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

* [Why Spec-Driven Development Fails](./sdd/why-sdd-fails.md) — The unresolved translation problem
* [State of the Art](./comparison/state-of-the-art.md) — Comparison with current approaches
* [Related Work](./comparison/related-work.md) — Prior art and adjacent research

### [Reference](./reference/overview.md)

Source documents, prior work, and external material collected during the research.
