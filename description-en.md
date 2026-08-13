# What Is SPEX Supposed to Become?

SPEX is an executable specification language for business and UI logic.

The specification is the **Single Source of Truth**: it describes the intended behaviour precisely enough for a compiler to deterministically generate executable logic from it. At the same time, the syntax remains valid English so that business experts can read and review it without needing programming-language knowledge.

## The Basic Idea

In traditional software development, a specification is interpreted by people and translated into code. Agentic Spec-Driven Development with AI largely automates the same process:

```text
Specification
     ↓
Interpretation
     ↓
Plan / Tasks
     ↓
Code
     ↓
Tests / Reviews
```

AI can accelerate this traditional development process dramatically. But the fundamental risk remains: at every translation step, information can be lost, assumptions can be added, or requirements can be interpreted differently from what was originally intended.

Agentic SDD therefore does not eliminate this problem. It makes the translation faster — and misunderstandings and errors can propagate faster into code, tests, and other artefacts as well.

SPEX starts earlier:

```text
Natural Language
      ↓
LLM assists
      ↓
Precise SPEX Specification
      ↓
Compiler
      ↓
Executable Behaviour
```

The LLM helps transform an initially incomplete or vague description into precise SPEX syntax. The compiler then checks the specification for missing cases, contradictions, type errors, and other forms of incompleteness.

After that, the functional behaviour is **no longer interpreted**. It is compiled deterministically.

## Precise, but Still Readable

SPEX is formal, but it is designed to remain readable as correct English:

```text
An Account has a Balance of Dollars.

When a Withdrawal Request occurs:

- Provided that its Requested Amount does not exceed
  its Customer's Account's Balance:

- The resulting Outcome is successful.

- Otherwise:

- The resulting Outcome is declined.
```

The same specification can therefore be read both by the compiler and by a business expert.

The purpose of precision is not to make communication harder, but to improve it: unclear rules, missing cases, and different interpretations become visible before they disappear into the implementation.

## Examples Validate the Specification

Concrete examples complement the rules and can be discussed directly with business experts.

Their central question is not:

> Did the developer implement the specification correctly?

but:

> Did we specify the intended behaviour correctly?

Examples therefore help uncover misunderstandings in the specification itself.

## Business and UI Logic

SPEX describes both traditional business logic and UI logic, for example:

- states
- user actions
- validations
- state transitions
- business dependencies

The concrete presentation is deliberately excluded.

SPEX can define **when an action is allowed**, but not how a button looks or which UI framework is used.

## Functional Behaviour Is Compiled, Infrastructure Remains Flexible

SPEX focuses on behaviour that can be defined as objectively correct or incorrect.

Non-functional requirements such as performance, scalability, or availability are instead treated as measurable targets.

This creates a clear separation:

**Functional requirements are specified and compiled.**

**Non-functional requirements are measured and fulfilled by the technical implementation.**

The infrastructure around the business kernel can therefore still be developed and optimised conventionally or with AI assistance.

## Changes Start in SPEX

When a business rule changes, the specification is changed and compiled again:

```text
Business Change
      ↓
SPEX Change
      ↓
Compiler
      ↓
Updated Behaviour
```

There is no separate business implementation that has to be kept in sync with the specification afterwards.

The specification remains the authoritative description of the behaviour that is actually executed.

## The Difference in One Sentence

**Agentic SDD with AI accelerates the traditional translation from specification to software. SPEX eliminates that translation for functional behaviour.**

The philosophy behind it is:

> **Interpretation where freedom is useful. Determinism where correctness is required.**

The specification therefore does not merely describe what the software should do.

**It is the executable definition of what the software does.**
