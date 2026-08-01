# The Mathematical Bridge

## The Fusion Architecture

The core design ambition is a 1:1 isomorphism between controlled English and typed lambda calculus:

```
┌─────────────────────────────────────────────────┐
│           HUMAN INTERFACE                       │
│  "A Customer may Cancel an Order if and only   │
│   if the Order belongs to the Customer and      │
│   the Order is not Shipped."                    │
│                                                 │
│  (Controlled English / Legal Grammar)           │
└──────────────────┬──────────────────────────────┘
                   │  Parser
                   ▼
┌─────────────────────────────────────────────────┐
│           AST (Abstract Syntax Tree)            │
│                                                 │
│  Permission(Cancel, Customer, Order)            │
│    ∧ BelongsTo(Order, Customer)                 │
│    ∧ ¬Shipped(Order)                            │
└──────────────────┬──────────────────────────────┘
                   │  Type Checker
                   ▼
┌─────────────────────────────────────────────────┐
│         TYPED LAMBDA CALCULUS                   │
│                                                 │
│  λorder.λcustomer.                              │
│    And (BelongsTo order customer)               │
│         (Not (Shipped order))                   │
│                                                 │
│  Pure · Stateless · No Side Effects             │
└──────────────────┬──────────────────────────────┘
                   │  Solver / Evaluator
                   ▼
┌─────────────────────────────────────────────────┐
│           BOOLEAN RESULT                        │
│                                                 │
│  true  → action permitted                       │
│  false → action blocked + guidance shown        │
└─────────────────────────────────────────────────┘
```

## Why lambda calculus?

### Church-Rosser theorem

The Church-Rosser theorem is relevant because it describes a form of confluence: when reduction reaches a normal form, the order of evaluation does not change the final result. In this research direction, that matters because SPEX needs deterministic semantics at the business-logic level.

The intended consequence is not a magical implementation guarantee. It is a semantic target: the meaning of the specification should not depend on prompt phrasing, runtime mood, or platform-specific interpretation.

That does not eliminate all systems concerns by itself. Concurrency, I/O, distribution, and storage are still physical concerns. The point is narrower: the logical meaning of the accepted specification should be deterministic.

### Pure functions

Lambda calculus is attractive here because it is:
- **Stateless** — no mutable state, no hidden side effects
- **Referentially transparent** — same input always yields same output
- **Composable** — complex rules are built from simple, verifiable pieces

## The solver

In a stronger version of the design, the mathematical solver would perform:

1. **Completeness checking** — are all state transitions covered?
2. **Contradiction detection** — do any rules conflict?
3. **Reachability analysis** — are there unreachable states?
4. **Counterexample generation** — if a rule is incomplete, produce a concrete scenario that exposes the gap
