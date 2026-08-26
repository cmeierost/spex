# SPEX: Executable Specifications for AI-Assisted Development

*SPEX: A Specification Language, a Compiler, and a New AI Development Workflow*

At OST — Eastern Switzerland University of Applied Sciences — we are working on a research project that explores an alternative approach to AI-assisted software development.

We call it SPEX — executable specifications.

The problem SPEX addresses is this: current AI-oriented spec-driven development can improve requirements elicitation, structure and traceability, but it still relies on probabilistic translation between specification and implementation.

SPEX explores whether the functional core of an application can instead be expressed in a readable language with formally defined semantics and compiled directly into production behaviour.

AI may help turn ambiguous human intent into a precise specification.

**But here is the key idea:**

> Once the behaviour has been defined precisely, there is no reason to ask AI to guess it again.

This applies to the functional business core covered by SPEX. Infrastructure, persistence, integration, rendering, and other implementation choices remain outside that deterministic boundary and can still be implemented by AI or conventional software engineering.

Read Part 1 for the reasoning behind that approach:

**Spec-Driven Development Gets Your Spec Wrong — Part 1**

## A DSL in Controlled Natural English

SPEX is a domain-specific language designed to read like natural English.

Its purpose is to provide a common language for domain experts and software developers: readable enough for people without a programming background, but restricted enough that every valid construct has defined semantics.

A valid SPEX specification can therefore be translated deterministically into executable business behaviour.

SPEX is embedded in Markdown, so specifications can be stored, reviewed and versioned using ordinary documentation tools and source repositories.

## Compiler and Specification Tooling

The SPEX compiler translates a valid specification into an executable business core.

The compiler toolchain is designed to detect syntax and type errors, invalid references, incomplete constructs, and classes of missing or contradictory cases that can be determined from the language semantics. It can also suggest examples or questions that may help uncover missing business requirements.

Where deterministic tooling cannot safely derive a valid formulation, an LLM may be used to propose a reformulation in SPEX.

The important boundary is:

> An LLM may help create or refine a specification, but it does not determine the meaning of a domain-approved SPEX statement. That meaning is defined by the language semantics.

## Specification Loop

SPEX supports an iterative specification workflow.

An author may begin with ordinary, potentially ambiguous natural language. Where possible, deterministic tooling can transform or suggest valid SPEX directly. Where more interpretation is required, an LLM can propose one or more SPEX formulations.

The developer or domain expert then reviews the resulting SPEX statement and decides whether it expresses the intended behaviour.

The LLM may also act as a discussion partner by asking clarification questions, identifying possible missing cases, suggesting examples or drafting larger parts of a specification.

However, anything inferred or invented by the LLM remains unapproved until a human accepts it.

This creates a deliberate boundary between probabilistic interpretation and deterministic semantics:

> AI may assist before the specification is accepted. Once accepted, the meaning of the specification is no longer inferred by AI.

The accepted SPEX statement remains in the source repository as the authoritative representation of that behaviour; it is not merely an intermediate prompt or generation artifact.

## Domain Approval and Traceability

Individual rules or other parts of a SPEX specification can be assigned an approval state.

Once the responsible domain expert confirms that a rule expresses the intended business behaviour, that rule can be marked as domain-approved.

If domain-approved behaviour is changed, its approval is invalidated and the changed behaviour must be reviewed again.

Unapproved parts may remain editable by developers or AI agents during development. This allows SPEX to distinguish explicitly between:

- behaviour proposed by an AI or developer;
- behaviour reviewed by a software developer;
- behaviour explicitly accepted by the domain expert responsible for defining it.

These states are part of the development workflow rather than comments on the side. They make it explicit which behaviour is still being proposed or explored and which behaviour has become an authoritative requirement that should not be changed without renewed domain approval.

For high-assurance systems, the goal is that all behaviour considered business-critical ultimately reaches the appropriate domain-approved state.

## Infrastructure Agnosticism

SPEX specifies required system behaviour, not the technical mechanisms used to implement that behaviour.

SPEX therefore does not directly open files, access databases, make HTTP calls, create threads or invoke framework-specific APIs.

This restriction is deliberate.

A SPEX specification defines what the system must guarantee. It does not prescribe which database, framework, communication protocol or infrastructure technology must be used to provide that guarantee.

## Infrastructure Implementation

Infrastructure is connected to the SPEX-generated business core from the outside.

Human developers or AI agents can implement persistence, communication, user-interface rendering, and other technical mechanisms against interfaces and contracts derived from the specified behaviour.

For example, SPEX may define that the system remembers an Account. This means that the Account forms part of system state and must remain available to later behaviour for the lifetime defined by the specification. SPEX does not prescribe how that state is stored.

If state must survive a restart or failure, that durability requirement must be specified explicitly and satisfied by the infrastructure.

The same principle applies to other technical concerns. Requirements such as latency, availability, technical security properties, deployment constraints or resource limits can be stated and measured separately without becoming part of SPEX's executable business semantics.

This produces two complementary forms of automated system validation:

- **Behavioural validation:** examples derived from or written alongside the SPEX specification can be executed against the integrated system to check that infrastructure preserves the behaviour defined by SPEX.
- **Technical validation:** measurable implementation requirements such as performance, availability or security constraints can be tested directly against the running system.

## The Key Architectural Principle of SPEX

SPEX defines the functional behaviour whose meaning must be precise and compiles that behaviour deterministically into the production business core. AI and conventional software engineering remain useful before this semantic boundary and for implementation choices outside it, as long as the surrounding infrastructure preserves the behaviour and guarantees defined by SPEX.

This does not eliminate validation, integration testing or the need to establish separately that the compiler and runtime preserve the defined SPEX semantics. It aims to remove one specific source of semantic drift: probabilistic reinterpretation of business behaviour that has already been defined precisely.

SPEX is therefore not intended primarily as a separate model against which another business implementation is checked. For the behaviour covered by SPEX, the accepted specification is intended to be the source language of the production business core itself.

This includes not only decisions and calculations, but also typed domain concepts, events, state and state transitions. The generated core exposes contracts to the surrounding infrastructure, while persistence, communication, rendering and other technical mechanisms remain outside it.

The SPEX source remains the authoritative representation of that behaviour. AI may help humans author and refine it, but accepted statements are retained, versioned and compiled rather than regenerated from natural-language requirements whenever the software changes.

---

## Language Features

SPEX is still under development. The current prototype implements only part of the language, but the following examples illustrate some of the concepts we are exploring.

### Terms

Terms define domain concepts, their structure and their constraints. They effectively form SPEX's type system from the specification perspective.

Primitive values can carry semantic constraints and define readable typed literals:

```spex
## Terms

- Money is a Decimal Number with at most 2 decimal places.
- Dollars is Money with literal prefix "$".
```

This makes `$234.56` a value of type `Dollars`, not merely a number with display formatting. Prefixes and suffixes can turn ordinary literals into strongly typed values while keeping the specification readable.

The same mechanism can be used for units. For example, `5s` and `8min` can both be typed as durations, with their conversion relationship defined by the language.

Domain concepts can be composed from other terms, constrained, and expressed as alternatives:

```spex
## Terms

- A Customer has an Account.
- An Account has a Balance of Dollars.

- A Withdrawal Request has a Customer
  and a Requested Amount of Dollars greater than zero.

- An Outcome is either successful or declined.
- A Withdrawal has an Outcome and a Balance of Dollars.
```

The language is intended to support strongly typed domain concepts, relationships, constraints and alternatives while keeping their representation readable to domain experts. Relationship words such as `has` have precisely defined cardinality semantics in SPEX rather than relying on ordinary-English interpretation.

### Rules

Rules define behaviour.

Structured input is evaluated according to explicit business rules and produces a defined result.

```spex
## Rules

When a Withdrawal Request occurs, the result is a Withdrawal:

- Provided that
    its Requested Amount does not exceed
    the Customer's Account's Balance:

  - The resulting Outcome is successful.
  - The resulting Balance is the Customer's Account's Balance minus its Requested Amount.

- Otherwise:

  - The resulting Outcome is declined.
  - The resulting Balance is the Customer's Account's Balance.

- The Customer's Account's Balance becomes the resulting Balance.
```

Conditions such as `Provided that` and `Otherwise` define explicit branches whose meaning is determined by the language rather than inferred by an LLM.

The planned keyword `becomes` expresses a state transition: it defines how system state changes as a consequence of a rule without defining how that state change is persisted technically.

### State

State is part of observable system behaviour and can therefore also be specified explicitly:

```spex
- The System remembers an Account.
```

This means that an Account forms part of the system state and can be used by later behaviour.

The same model can apply to both long-lived state, such as persistent business data, and short-lived state, such as the state of a user interface.

The keyword `becomes` defines an atomic state transition: either all resulting state changes take effect or none do. For the current runtime model, the compiler generates the locking and concurrency control required to evaluate such transitions against a consistent current state. The infrastructure only needs to commit the resulting transition and report whether that commit succeeded.

### Examples

Examples serve a different purpose from tests whose purpose is to check whether an LLM correctly translated a specification.

Because SPEX business behaviour is compiled deterministically, examples are primarily used to check whether the specification itself expresses the intended behaviour.

Inspired by Gherkin and Cucumber, SPEX uses its own typed example syntax. An example may look like this:

```spex
Scenario: Successful withdrawal within balance
  Alice is a Customer
  Given Alice's Account's Balance is $234.56
  When a Withdrawal Request occurs
    whose Customer is Alice
    and whose Requested Amount is $200.00
  Then the Withdrawal's Outcome is successful
  And the Withdrawal's Balance is $34.56
  And the Account's Balance is $34.56

Scenario: Declined withdrawal exceeding balance
  Hamza is a Customer
  Given Hamza's Account's Balance is $100.00
  When a Withdrawal Request occurs
    whose Customer is Hamza
    and whose Requested Amount is $150.00
  Then the Withdrawal's Outcome is declined
  And the Withdrawal's Balance is $100.00
  And the Account's Balance is $100.00
```

Type declarations such as `Alice is a Customer` are part of SPEX and are required so that examples can be type-checked like the rest of the specification.

Examples are compiled and evaluated using the same SPEX semantics as the specification. Their purpose is to validate the intended behaviour, not to establish correctness of the compiler itself.

If an example does not produce its expected result, either the specification or the example must be corrected.

The same examples can later be executed against the integrated application to check that persistence, communication, user-interface code and other infrastructure still preserve the behaviour defined by SPEX.

### More Concepts

The broader SPEX design is intended to include additional typed domain constructs such as relationships and cardinalities, constrained numbers and units, dates and time, alternatives, derived values, preconditions and postconditions, workflows, events and state transitions.

The same approach can also be applied to behavioural user-interface state.

These constructs follow the same principle: their syntax remains business-readable while their semantics are defined by the language rather than inferred by an LLM.

The current prototype compiles SPEX business behaviour into TypeScript that is intended to serve directly as the application's production business core, together with executable examples. Other target languages or representations could be supported by replacing only the final code-generation stage of the compiler. The architectural principle remains the same: the specified behaviour is compiled rather than independently reimplemented.

## Potential Benefits of SPEX

### Better Specifications

If SPEX can become a common language between developers and domain experts, it could reduce misunderstandings and make specified behaviour directly reviewable by the people responsible for it.

Whether domain experts can actually read and review SPEX efficiently is one of the key questions we still need to validate.

### Faster and Cheaper Development

Once behaviour is expressed as valid SPEX, generating the executable business core becomes a compiler task.

There is no need to repeatedly regenerate the same business logic with an LLM. And provided that the compiler toolchain can be trusted to preserve SPEX semantics, application-level tests no longer need to duplicate business rules merely to check whether generated business code still matches the specification.

This could reduce development time, maintenance effort and inference cost.

### Less LLM Inference

Domain-approved behaviour no longer needs to be repeatedly interpreted by a large language model.

The compiler performs that translation directly, while the LLM can concentrate on tasks where interpretation is useful: helping formulate SPEX, resolving unclear input and implementing infrastructure around the compiled business core.

This should require substantially less computation and may allow smaller models to handle a larger part of the development process. The actual energy and cost savings still need to be measured.

### Privacy and Digital Sovereignty

If specification assistance and infrastructure generation can increasingly be handled by local or on-premise models, both source code and sensitive business rules can remain within the organisation.

That could improve confidentiality and reduce dependence on frontier-model providers such as Anthropic or OpenAI.

## What's the Future of SPEX?

SPEX is still a small research project. We have a working language concept and compiler prototype, but there is a large difference between proving that an idea works and building a language and toolchain that companies can trust for real software.

SPEX explores whether a domain-expert-readable, domain-approved specification can serve directly as the source language for the production business core — eliminating a separately interpreted or generated implementation for precisely specified behaviour, while leaving the surrounding technical implementation open to AI.

The next step is to test SPEX on realistic projects: whether domain experts can efficiently review and approve the specifications, and whether SPEX actually reduces ambiguity, development effort and LLM usage.

If the results are positive, our intention is to develop SPEX as an open-source language and ecosystem.

If this idea resonates with you, challenge it, try it, contribute to it, help us test it on real software — or help fund the next step.

Leave a comment or send me a message.

The principle is simple:

> **Let AI interpret what is still open.**  
> **Compile what has already been decided.**

And if we can specify exactly how the software should behave:

> **Why should we ever ask an AI to guess again?**

## References

- SPEX — GitHub Repository (Brainstorming about SPEX)
- Cucumber — Gherkin Reference
- Attempto Controlled English — Project Resources

---

## Related Work and Similar Approaches

SPEX is not the first approach to making software behaviour precise, executable or model-driven. Related ideas exist in business-rule systems, executable process models, formal methods, model-driven engineering, domain-specific languages and language workbenches. SPEX explores a particular combination of these ideas for AI-assisted development: a domain-readable source language with defined semantics that compiles directly into the production business core.

### Robert Englander — *Conversational Software Engineering: Compiling Intent* — 2026

Conversational Software Engineering explores a closely related workflow in which human intent is progressively transformed into structured specifications, validated artifacts, tests and implementations. It also draws a clear boundary between probabilistic AI assistance and deterministic validation.

### Quint — Executable Specifications

Quint is a formal executable specification language for modelling and verifying system behaviour. Specifications can be simulated, model-checked and used for model-based testing, while the production implementation remains separate.

### NL2ASP — Natural Language to Answer Set Programs through Controlled Natural Language

NL2ASP uses probabilistic language translation to transform natural language into a controlled intermediate language, followed by deterministic translation into Answer Set Programs. This is closely related to SPEX's idea of using AI before a formal boundary and deterministic tooling after it.

### FSL AI-Native Formal Specification Language

FSL combines AI-assisted specification authoring with deterministic formal verification, including techniques such as SMT solving, bounded model checking and refinement. Humans confirm that the generated specifications capture the intended business rules, while implementation conformance can be checked separately.
