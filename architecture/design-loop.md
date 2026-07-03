# The Interactive Design Loop

## Authoring Without Coding

Humans do not type Spex specifications directly. Instead, authoring is a closed-loop cycle between three participants:

```
   ┌─────────┐     free language      ┌─────────┐
   │  HUMAN  │ ──────────────────────► │   LLM   │
   │         │                         │         │
   │         │ ◄────────────────────── │         │
   └─────────┘   formal Spex text      └────┬────┘
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

The LLM casts this into strict Spex grammar:

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

## Sign-Off

Once the solver confirms completeness, the human reviews the final spec text and signs off. This signed text is the product — no code, no deployment, no migration.
