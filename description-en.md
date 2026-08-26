# What is SPEX supposed to become?

SPEX is intended to become an executable specification language for business and behavioural UI logic.

Its central idea is that a specification, once read and approved by the responsible people, becomes the **authoritative source of business behaviour**. It should describe that behaviour precisely enough for a compiler to translate it deterministically into the production business core. At the same time, its controlled syntax should remain close enough to English that domain experts can discuss and review it without learning a conventional programming language.

SPEX is therefore not ordinary natural language and not merely a better prompt. It is a restricted language in which every valid construct must have defined semantics.

## The basic idea

In traditional software development, people interpret a specification and translate it into code. Current AI-oriented Spec-Driven Development largely accelerates the same sequence:

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

At every step, information can be lost, assumptions can be introduced, or requirements can acquire a meaning that nobody explicitly approved. Tests, reviews, and guardrails can detect some incorrect outcomes, but they create further representations that must also be shown to preserve the specification's meaning.

That leaves two questions:

> What exactly should the software do?

and

> How do we know that the software actually does exactly that?

The underlying problem depends on how important and complete the specification is:

- **We do not care about the behaviour.** A team can deliberately leave it open to a human or AI implementer.
- **The specification is incomplete.** Whoever implements the missing behaviour has to guess; software cannot be shown to match an intended behaviour that was never defined.
- **The specification is precise.** There should be nothing left to interpret, so interpreting it probabilistically again introduces avoidable semantic risk.

You cannot expect even the smartest AI to implement exactly what you intended if you never expressed exactly what you intended.

SPEX focuses on the third case while helping humans expose and resolve the second:

```text
Human Intent
     ↓
LLM and Formal-Tool Assistance
     ↓
Precise SPEX Specification
     ↓
Human Approval
     ↓
Deterministic Compiler
     ↓
Production Business Core
```

An LLM may interview domain experts, identify uncertainty, and propose SPEX formulations. The compiler and formal-analysis tools check what can be established from the language semantics, such as syntax, types, references, structural completeness, contradictions, unreachable cases, or counterexamples.

These tools cannot decide what the business intended. The LLM may propose the specification, but it does not decide the required behaviour or define the language semantics. The responsible human reviews the precise result and decides whether it expresses the intended meaning. Once accepted, that meaning is **no longer inferred** by AI; it is preserved through deterministic compilation.

## Precise, but still readable

SPEX is formal, but its constructs are designed to read like controlled English:

```
An Account has a Balance of Dollars.

When a Withdrawal Request occurs, the result is a Withdrawal:
  - provided that its Requested Amount does not exceed
    its Customer's Account's Balance:
    - the resulting Outcome is successful; and
    - the resulting Balance is its Customer's Account's Balance
      minus its Requested Amount;
  - otherwise:
    - the resulting Outcome is declined; and
    - the resulting Balance is its Customer's Account's Balance.
```

Precision here does not mean merely well-written English. A specification needs definitions, not impressions. Constructs such as types, relationships, conditions, alternatives, and state transitions must have a meaning defined by the language rather than inferred from context. Once a language has formally defined semantics, a compiler, not an LLM, can preserve them during translation.

The aim is to make unclear rules, missing cases, and competing interpretations visible before they disappear into an implementation.

## Human approval and traceability

Anything proposed by an LLM or developer remains a proposal until a responsible human accepts it. Parts of a specification may therefore have explicit states such as proposed, developer-reviewed, or domain-approved.

If approved behaviour changes, its approval is invalidated and the changed statement must be reviewed again. This keeps authority and traceability attached to the behaviour itself rather than to comments, meeting notes, or tests around it.

Formal validity and business approval answer different questions:

- The compiler establishes whether a statement is valid within the defined SPEX semantics.
- A human establishes whether that valid statement expresses the intended business meaning.

## Examples validate the specification

Concrete, typed examples complement the rules and can be discussed directly with domain experts. Their central question is not:

> Did the developer implement the specification correctly?

but:

> Did we specify the intended behaviour correctly?

Examples are evaluated using the same SPEX semantics as the rules. They must not silently complete missing behaviour or become a second, hidden specification. The same approved examples can later run against the integrated application to check whether persistence, communication, rendering, and other infrastructure preserve the specified behaviour.

Tests still matter, but they no longer need to compensate for a probabilistic translation of already-defined business behaviour. Otherwise, the original trust problem merely moves to another question: who verifies that the tests themselves express what the specification says?

## Business and behavioural UI logic

SPEX is intended to describe behaviour whose outcome can be judged correct or incorrect, including:

- typed domain concepts and values;
- business decisions and validations;
- permissions and user intents;
- events, state, and state transitions;
- observable UI state and permitted interactions; and
- external authorities and declared boundary ports.

The concrete presentation is deliberately excluded. SPEX may define **when an action is allowed** and how application state changes in response, but not how a button looks or which UI framework renders it.

## Functional behaviour is compiled, infrastructure remains flexible

SPEX defines what the system must guarantee without prescribing every technical mechanism used to provide that guarantee.

For example, SPEX may state that an accepted withdrawal reduces an account balance. That state transition belongs to the specified behaviour. Whether the new balance is stored in PostgreSQL, an event store, or a remote banking system remains an infrastructure decision. If the balance must survive a restart, durability must be declared as a measurable requirement and fulfilled by the infrastructure.

The same distinction applies elsewhere. A rule that only a certain role may approve a payment is business behaviour; the identity provider, signed claims, or access-control mechanism used to enforce it is technical realisation.

This creates a deliberate boundary:

> *what behaviour the system must guarantee*

and

> *how that behaviour is implemented.*

Required behaviour is specified semantically and compiled. Open implementation choices remain available to engineers and AI, constrained by declared interfaces and measurable requirements.

Those requirements may cover performance, availability, durability, technical security properties, deployment, and cost. AI can remain highly useful around this boundary: before approval, where interpretation helps formulate the specification, and outside the compiled core, where it can find good ways to implement guarantees that have already been defined.

## Changes start in SPEX

When approved business behaviour changes, its SPEX source changes, its approval is renewed, and it is compiled again:

```text
Business Change
      ↓
SPEX Change
      ↓
Renewed Approval
      ↓
Compiler
      ↓
Updated Business Core
```

There is no separately maintained business implementation to synchronise with the specification. For behaviour covered by SPEX, the accepted specification is intended to be the source language of the production business core — not a model against which another implementation is independently written.

This does not remove every verification problem. The compiler and runtime must themselves be trusted to preserve the semantics defined by SPEX, and the complete application still requires integration and technical validation. The aim is to replace repeated, application-specific probabilistic interpretation with a reusable language and toolchain trust boundary.

## Current status

SPEX is a research direction supported by an exploratory compiler prototype, not a finished language or production toolchain. The prototype implements part of the proposed type system and compiles a limited set of specifications into TypeScript intended to serve as the application's business core.

There is not yet a stable grammar, complete formal semantics, general solver, conformance suite, or finished execution target. The challenge is not inventing executable specifications; they have existed for decades. It is making them practical enough for everyday software development: precise enough to compile, natural enough to read, and easy enough to write with AI assistance.

The central research questions are whether realistic business behaviour can be expressed without hidden implementation assumptions, whether domain experts can efficiently review it, and whether the approach reduces ambiguity, development effort, and LLM inference in practice.

## The difference in one sentence

**Current AI-oriented SDD accelerates the traditional interpretation of specifications into software. SPEX investigates whether that interpretation can end once functional behaviour has been precisely defined and approved.**

The principle is simple:

> **Let AI interpret what is still open.**
>
> **Compile what has already been decided.**

The specification would not merely describe what the software should do.

**It would be the executable, human-approved definition of what the software does.**

## Further reading

- [Spec-Driven Development Gets Your Spec Wrong — Part 1](./articles/spec-driven-development-gets-your-spec-wrong.md)
- [SPEX: Executable Specifications for AI-Assisted Development](./articles/spex-executable-specifications-for-ai-assisted-development.md)
- [The SPEX Manifest](./manifest.md)
