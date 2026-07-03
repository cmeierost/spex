# Grammar Keywords

## Formal Keyword Patterns

These are the building blocks of Spex specifications. Each keyword maps to a precise lambda-calculus construct.

---

## Entity Declaration

| Pattern | Meaning |
|---------|---------|
| `An <Entity> shall consist of ...` | Defines required composition of an entity |
| `An <Entity> may have ...` | Defines optional attributes of an entity |
| `<Entity> A shall be related to <Entity> B` | Declares a relationship between entities |

---

## State & Status

| Pattern | Meaning |
|---------|---------|
| `<Entity> shall be deemed <State> if and only if: ...` | Defines when an entity enters a state |
| `<Entity> shall transition from <A> to <B> when ...` | Defines a state transition rule |

---

## Permissions & Actions

| Pattern | Meaning |
|---------|---------|
| `<Actor> may <Action> an <Entity> if and only if: ...` | Declares a permission with preconditions |
| `<Actor> shall not <Action> an <Entity> if ...` | Declares a prohibition |

---

## Quantifiers

| Pattern | Meaning |
|---------|---------|
| `all <Entities>` | Universal quantification (∀) |
| `at least one <Entity>` | Existential quantification (∃) |
| `none of the <Entities>` | Negated existential (¬∃) |
| `exactly one <Entity>` | Unique existential (∃!) |

---

## Logical Connectors

| Pattern | Meaning |
|---------|---------|
| `and` | Conjunction (∧) |
| `or` | Disjunction (∨) |
| `not` | Negation (¬) |
| `if and only if` | Biconditional (↔) |

---

## Views (Declarative Queries)

| Pattern | Meaning |
|---------|---------|
| `<Actor> may view <Entity> if ...` | Defines a declarative filter for data visibility |

---

## Contract Points

| Pattern | Meaning |
|---------|---------|
| `<Entity> shall compute <Attribute> via <Contract Name>` | Declares a named contract point for computation |
| `<Action> shall require <Contract Name>` | Declares a named contract point for an action |

The spec does not reference implementations. It declares **named contract points** — ports that an algorithm must fill.

The binding flows upward: the algorithm declares which contract point it implements.

```
-- In the spec:
An Order shall compute its Discount via CalculateDiscount.

-- In the algorithm (Haskell):
-- Implements: CalculateDiscount
-- Input: Order
-- Output: Decimal
```

This is **inversion of control**. The spec defines the port. The algorithm is the adapter. The dependency arrow points inward: implementation → contract, never contract → implementation.

The compiler verifies:
1. Every contract point has exactly one implementation
2. The implementation's signature matches the contract (input types, output type)
3. The implementation satisfies the declared preconditions and postconditions

---

## Transactions

| Pattern | Meaning |
|---------|---------|
| `<Action> shall be atomic` | Declares that an action's steps must all succeed or all roll back |
| `If <Action> fails, then <Rollback Action> shall occur` | Declares a rollback rule for a failed action |

Transactions are specification logic, not persistence. The spec declares **what** must be atomic and **what** to undo. The runtime handles **how** to persist and roll back.

```
A Customer may Place an Order.
Placing an Order shall be atomic.

If Placing an Order fails, then:
  - the reserved Inventory shall be released; and
  - the pre-authorization shall be voided.
```

No database concepts. No savepoints. No isolation levels. The spec declares business-level atomicity: "these things happen together, or none of them do."

---

## Design Notes

- Keywords are **case-insensitive** for readability
- Semicolons (`;`) separate conditions in a list
- Indentation is significant for nested conditions
- The grammar rejects ambiguous natural language patterns at parse time

---

## The Computational Fragment

The grammar is **not** Turing-complete. It deliberately omits:

| Missing | Lambda Calculus Equivalent | Why It's Missing |
|---------|--------------------------|------------------|
| Recursion | Y combinator | Algorithms are physical, not logical |
| Let bindings | `(λx.M) N` | Intermediate computation belongs in the algorithm layer |
| Arithmetic | Church numerals | Numeric operations are algorithmic |
| Pattern matching | Church encodings | Case analysis is implementation detail |
| Collections | Church lists | Aggregation, sorting, grouping are algorithmic |

This is not a gap. It is a boundary.

The grammar covers the **logical fragment** of lambda calculus: propositional logic, predicate logic, quantification, and boolean combinators. That is sufficient for specification — rules, constraints, permissions, and state.

Computation lives in the algorithm layer. Algorithms are written in a general-purpose language (e.g., Haskell). The spec binds to them by reference, not by embedding. The compiler verifies the binding is correct.

| Concern | Where It Lives | Example |
|---------|---------------|---------||
| Specification logic | Spex grammar | "An order over $1000 gets a 10% discount" |
| Algorithm binding | Spex grammar | "An Order shall use CalculateDiscount" |
| Algorithm implementation | General-purpose language | The Haskell function that computes the discount |
| Infrastructure | Runtime configuration | The database that stores orders |

The spec declares *what* and *which implementation*. The algorithm implements *how*. The grammar is complete for *what* and *which*.
