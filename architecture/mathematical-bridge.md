# The Mathematical Bridge

## The Fusion Architecture

Spex's core innovation is a 1:1 isomorphism between controlled English and typed lambda calculus:

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

## Why Lambda Calculus?

### Church-Rosser Theorem

The Church-Rosser theorem guarantees that regardless of the order of evaluation, a lambda expression always reduces to the same normal form. This means:

> **The spec evaluates identically on any computer in the universe.**

No race conditions, no non-determinism, no platform-specific behavior.

### Pure Functions

Lambda calculus is:
- **Stateless** — no mutable state, no hidden side effects
- **Referentially transparent** — same input always yields same output
- **Composable** — complex rules are built from simple, verifiable pieces

## The Solver

The mathematical solver performs:

1. **Completeness checking** — are all state transitions covered?
2. **Contradiction detection** — do any rules conflict?
3. **Reachability analysis** — are there unreachable states?
4. **Counterexample generation** — if a rule is incomplete, produce a concrete scenario that exposes the gap
