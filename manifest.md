# The SPEX Manifest

> **The specification, read and approved by humans, should be the authoritative business artefact.**

## 1. Working Thesis

SPEX is a proposal for a research project. It does not yet exist as a finished language, compiler, solver, or runtime.

The proposal begins with a persistent problem in software engineering: business intent must pass through several representations before it becomes executable software. It may begin in conversations, meeting notes, requirement documents, examples, or tests before being translated into application code.

Each translation can lose information, fill gaps implicitly, and introduce assumptions that domain experts never reviewed.

AI-assisted development does not eliminate this problem. It accelerates the same translation and performs it probabilistically. More capable models may infer intent more successfully, but they cannot establish what the intended behaviour was when that intent was never expressed precisely.

SPEX investigates a different model:

1. Business behaviour is expressed in a restricted, computable form of English.
2. An LLM assists humans in discovering, clarifying, and reformulating that behaviour.
3. A compiler and formal-analysis tools identify invalid, incomplete, contradictory, or insufficiently precise statements.
4. Humans remain responsible for approving the meaning of the resulting specification.
5. The approved specification is compiled deterministically into executable business behaviour.
6. Infrastructure is designed independently against declared interfaces and measurable non-functional requirements.

Generated code may still exist, but it becomes a derived execution artefact rather than a separately maintained source of business meaning.

---

## 2. The Three Core Invariants

### I. Separation of Business Meaning and Technical Realisation

The business specification defines the logical world of the system:

* business concepts and values;
* permissions and responsibilities;
* decisions and invariants;
* valid state transitions;
* observable business state;
* user intents;
* external authorities;
* declared inputs, outputs, and boundary ports.

It must not depend on how these concepts are technically represented or operated.

The physical world provides the mechanisms required to run the system:

* storage engines and data encodings;
* APIs and network protocols;
* queues and communication mechanisms;
* deployment and scaling;
* replication and recovery;
* security mechanisms;
* widgets and rendering technologies;
* monitoring and operations.

The distinction does not mean that business logic cannot refer to state, time, people, external organisations, or physical events. These concepts may be part of the business domain. However, the specification declares their meaning and authority without prescribing the technical mechanisms through which they are provided.

The same principle applies to values. The specification defines meaning through ranges, precision, units, permitted states, and other business constraints. Infrastructure selects suitable machine representations and storage encodings without changing that meaning.

### II. The Specification Is the Source of Business Truth

The approved specification should be the authoritative definition of business behaviour.

Application code, tests, database schemas, API implementations, and user interfaces must not become competing sources of business rules. They are derived technical artefacts whose purpose is to execute, integrate, expose, or verify the behaviour defined by the specification.

Given the same valid SPEX specification, every conforming compiler must produce semantically equivalent observable business behaviour.

Concrete examples are discussed with domain experts to reveal misunderstandings and confirm that the formal rules express their intent. These examples do not replace or complete the specification. They validate shared understanding of it and can later be reused as integration tests against the complete application.

This addresses three structural weaknesses of LLM-based specification-to-code workflows:

* **Unverified interpretation:** Generated implementations may contain assumptions or decisions that nobody explicitly reviewed or approved.

* **Invisible specification in tests:** When the written specification is incomplete, teams encode missing business behaviour in tests. Those tests become an implicit, code-oriented part of the specification that domain experts may never read or approve.

* **Unverified verification:** When teams do not trust the transformation from specification to generated code, they express the expected behaviour again in tests and guardrails. This introduces another translation—from specification into test code—which must itself be reviewed to establish that it expresses the same meaning. Trust is shifted to another manually maintained artefact rather than established by the transformation.

### III. Intent-Driven Interaction

Business meaning should not be encoded through presentation-specific state such as `button.disabled`, page navigation flags, or widget visibility rules.

The specification should instead define:

* what an actor can observe;
* which intents are available;
* which preconditions apply;
* what the actor is permitted to do;
* which business outcomes an intent may produce;
* how observable business state changes.

A user interface becomes a replaceable projection of these capabilities. Different rendering technologies may present the same intents and observable values through different widgets, layouts, devices, or interaction mechanisms without redefining the underlying business rules.

---

## 3. Compiler-Guided Specification

The intended role of the LLM is primarily that of an interviewer, discussion partner, and drafting assistant.

The authoring process would form a controlled loop:

1. A human expresses business knowledge in ordinary, potentially imprecise language.
2. The LLM asks questions and proposes statements in the controlled SPEX language.
3. The compiler checks syntax, types, references, and structural completeness.
4. Solvers and other formal-analysis tools examine properties such as contradictions, violated invariants, unreachable cases, and counterexamples.
5. The LLM translates these diagnostics into understandable questions and possible reformulations.
6. The human confirms or rejects the proposed meaning.
7. The process continues until the specification passes the required formal checks and the responsible humans approve its meaning.

The compiler establishes formal validity within the declared scope. It cannot determine whether a formally valid statement expresses what the business actually intends. That authority remains with humans.

The LLM must not silently resolve missing business decisions. When the intended behaviour is unknown, its responsibility is to ask.

The same principle applies to value semantics. Terms such as “exact,” “short,” “large,” or “fast enough” must be replaced by explicit constraints such as ranges, precision, rounding behaviour, units, length, latency, or throughput.

---

## 4. Formal Semantics

SPEX aims to establish a deterministic, semantics-preserving mapping from accepted controlled-English statements into a formal intermediate representation.

A pure, typed computational core inspired by typed lambda calculus is one candidate foundation. Business decisions and state transitions could be represented as pure transformations from declared inputs and previous state to outcomes, resulting state, and declared outputs.

The research must determine which additional formalisms are required for constraints, relations, temporal behaviour, transactions, completeness analysis, and solver integration.

The essential requirement is not allegiance to one mathematical formalism. It is that every accepted language construct has explicit formal semantics and cannot depend on an LLM’s interpretation during compilation.

---

## 5. Execution and Infrastructure

### State Changes and Persistence

The specification defines the meaning of a state transition. Infrastructure determines how the resulting state is preserved, replicated, discarded, or supplied by an external authority.

For example, SPEX may define that an accepted withdrawal reduces an account balance. It does not define whether that balance is stored in PostgreSQL, an event log, an in-memory process, or a remote banking system.

Persistence is therefore a technical mechanism around a business-defined state transition.

### Reading and Visibility

Rules determining who may observe which business information belong to the specification.

The mechanism used to retrieve, index, cache, transmit, and render that information belongs to infrastructure.

### Portable Business-Engine Parts

The compiler should be able to produce portable business-engine parts with declared:

* inputs and outputs;
* state responsibilities;
* invariants and transactions;
* external authorities;
* observable values;
* boundary ports.

The proposal also investigates whether these parts can be distributed when their dependencies and transactional boundaries permit it.

Infrastructure may decide where and how the parts run, but it must preserve the behaviour defined by the approved specification.

### Measurable Infrastructure

Infrastructure does not need to be trusted as an interpretation of business intent. It can instead be evaluated against:

* declared interfaces;
* integration tests derived from approved examples;
* security constraints;
* latency and throughput targets;
* availability and durability requirements;
* resource and deployment constraints.

Engineers and LLMs may implement this infrastructure using different technologies. The implementation is acceptable when it preserves the business-engine contract and fulfils the measurable requirements.

---

## 6. Research Questions

The proposed research should investigate whether:

* controlled English can express realistic business domains without hidden implementation assumptions;
* domain experts can understand, discuss, and approve SPEX specifications;
* compiler diagnostics can expose missing cases, contradictions, invalid statements, and insufficiently precise rules;
* compiler-guided LLM assistance reduces the effort required to produce valid specifications;
* humans can reliably determine whether LLM-proposed reformulations preserve their intended meaning;
* executable examples effectively uncover misunderstandings during specification;
* smaller or less capable language models can achieve results comparable to frontier models when guided by precise compiler diagnostics;
* the separation between business meaning and technical realisation remains practical for stateful, distributed, and interactive applications.

Models could be compared through compiler acceptance, domain-expert acceptance, unsupported assumptions, clarification and correction steps, human effort, token usage, execution time, and computational cost.

---

## 7. Relationship to Existing Approaches

SPEX is not presented as an entirely new idea. It combines and extends ideas from controlled natural language, formal specification, functional programming, model-driven development, Rules as Code, domain-driven design, and architecture patterns that separate business policy from technical mechanisms.

| Approach                                       | Unresolved issue addressed by SPEX                                                                                                                |
| ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Conventional programming**                   | Business meaning is encoded in implementation languages that domain experts usually cannot review directly.                                       |
| **LLM-based specification-to-code**            | The transformation remains probabilistic and may introduce unreviewed assumptions.                                                                |
| **Specification by example**                   | Examples can clarify intended behaviour but may become an incomplete or implicit replacement for formal rules.                                    |
| **Test-driven verification of generated code** | Tests restate expected behaviour in another technical artefact, producing unverified verification.                                                |
| **Model-driven development**                   | Models may remain inaccessible to domain experts, depend on technical notation, or require generated code to be edited and maintained separately. |
| **Application and cloud frameworks**           | Business policy is frequently coupled to persistence, communication, deployment, and framework-specific abstractions.                             |

SPEX investigates whether compiler-guided LLM assistance can make formal specification practical without making probabilistic systems the authority over business meaning.

---

## 8. Current Status

SPEX currently exists as a proposed research direction supported by an exploratory compiler prototype.

The prototype implements parts of the proposed type system and can compile a limited set of example specifications. It demonstrates that selected language ideas are implementable, but it does not validate the complete research hypothesis.

There is no stable language grammar, complete formal semantics, production compiler, general solver, conformance suite, or finished execution target.

This repository collects the concepts, candidate grammar, comparisons, experiments, examples, and architectural sketches needed to evaluate whether the proposed direction is viable.

---

## 9. Closing Statement

The ambition behind SPEX is to move software engineering closer to the direct execution of human-approved agreements.

Business meaning should not have to survive a chain of lossy translations through informal requirements, generated code, manually restated tests, and infrastructure-specific implementations.

Whether humans can write sufficiently precise specifications with the assistance of compiler-guided LLMs—and whether those specifications can support realistic software systems—remains the central question this proposed research project is intended to explore.
