# The Interactive Design Loop

## Authoring Without Coding

Humans do not type SPEX specifications directly. Instead, authoring is a closed-loop cycle between three participants:

```
   ┌─────────┐     free language      ┌─────────┐
   │  HUMAN  │ ──────────────────────► │   LLM   │
   │         │                         │         │
   │         │ ◄────────────────────── │         │
   └─────────┘   formal SPEX text      └────┬────┘
                                            │
                                            │ translate
                                            ▼
                                     ┌───────────────┐
                                     │   SOLVER      │
                                     │ (Mathematical │
                                     │  Compiler)    │
                                     └──────┬────────┘
                                            │
                                            │ counterexample
                                            ▼
                                     ┌───────────────┐
                                     │  CLARIFYING   │
                                     │   QUESTION    │
                                     │ (back to LLM) │
                                     └───────────────┘
```

## The Three Steps

### 1. Human Describes Intent

The human describes what they want in everyday, imprecise language:

> "I want customers to be able to cancel orders, but only if we haven't shipped them yet."

### 2. LLM Translates to Formal Spec

The LLM casts this into strict SPEX grammar:

```
A Customer may Cancel an Order if and only if:
  - the Order belongs to the Customer; and
  - the Order is not Shipped.
```

### 3. Solver Verifies Completeness

The mathematical compiler checks the formal spec and may find a blind spot:

> **Counterexample:** What happens if a Customer tries to Cancel an Order that belongs to a *different* Customer?

The LLM translates this into a clarifying question:

> "Should a Customer be able to cancel an Order that doesn't belong to them?"

The human answers, the LLM updates the spec, and the cycle repeats until the solver reports **zero logical errors**.

The same loop would apply to value constraints. If a specification says an amount must be precise, a text must be short, or a number must stay within a range, the system should prefer semantic constraints over guessed primitive types.

For example, if the spec says:

> "The Settlement Amount must be exact."

that may still be underspecified. The system should ask a clarifying question such as:

> "How many decimal places must the Settlement Amount support?"

or:

> "Is rounding permitted, and if so, by which rule?"

The goal is for the accepted specification to determine the required meaning of the value. The compiler can check that meaning, and infrastructure can choose a physical representation that satisfies it.

## Sign-Off

Once the solver confirms completeness, the human reviews the final spec text and signs off. This signed text becomes the authoritative business artifact.

Compiler output, boundary bindings, deployment choices, and migration mechanisms may still exist, but they are subordinate artifacts. They should satisfy the signed specification and its nonfunctional requirements rather than redefining the business meaning.
