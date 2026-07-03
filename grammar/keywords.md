# Candidate grammar keywords

## Formal Keyword Patterns

These are candidate building blocks for Spex-style specifications. They describe the current direction of the grammar design, not a finished language definition. The aim is for each accepted pattern to map to a precise formal construct.

The list is intentionally provisional. Some patterns may split, merge, or disappear as the grammar becomes more exact.

---

## Entity Declaration

| Pattern | Meaning |
|---------|---------|
| `An <Entity> shall consist of ...` | Defines required composition of an entity |
| `An <Entity> may have ...` | Defines optional attributes of an entity |
| `<Entity> A shall be related to <Entity> B` | Declares a relationship between entities |

---

## Glossary and Outside Authorities

| Pattern | Meaning |
|---------|---------|
| `<Name> shall be an outside authority` | Declares a named outside system, actor, or source of truth |
| `<Authority> shall be authoritative for <Value>` | Declares that the system uses another authority for the truth of a value |

Outside authorities are specification concepts, not infrastructure bindings. Their names must be declared before use so the compiler can reject undefined outside concepts.

The spec may say that **Payment Provider** is authoritative for Payment Status. It should not say whether that authority is reached through REST, a message queue, a file import, or human input.

---

## State Responsibility

| Pattern | Meaning |
|---------|---------|
| `<State> shall be persistent` | Declares that losing the state would violate the contract |
| `<State> may be forgotten when <Boundary> ends` | Declares state whose loss is allowed after a process, session, transaction, or other boundary |
| `<State> shall be provided by <Authority>` | Declares that another named authority provides the value |

These patterns describe responsibility for state, not storage. The compiler should require the boundary or authority to be defined before it is used.

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

## Value Constraints

| Pattern | Meaning |
|---------|---------|
| `<Value> shall be between <A> and <B>` | Declares a closed range constraint |
| `<Value> shall be greater than <A>` | Declares a lower bound |
| `<Value> shall be less than <B>` | Declares an upper bound |
| `<Value> shall use exactly <N> decimal places` | Declares a precision requirement |
| `<Text> shall contain no more than <N> words` | Declares a length constraint on text |

These patterns describe semantic constraints on values, not implementation-level primitive types.

The goal is to let the specification state what must be true of a value while leaving infrastructure free to choose an appropriate physical representation that satisfies compiler-checked semantics.

If the constraint does not determine a safe representation unambiguously, the compiler should ask for a more exact specification rather than silently guessing.

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

The spec would not reference implementations directly. It would declare **named contract points** — ports that an algorithm would fill.

The binding would flow upward: the algorithm would declare which contract point it implements.

```
-- In the spec:
An Order shall compute its Discount via CalculateDiscount.

-- In the algorithm (Haskell):
-- Implements: CalculateDiscount
-- Input: Order
-- Output: Decimal
```

This is **inversion of control**. The spec defines the port. The algorithm is the adapter. The dependency arrow points inward: implementation → contract, never contract → implementation.

In a stronger version of the design, the compiler would verify:
1. Every contract point has exactly one implementation
2. The implementation's signature matches the contract (input types, output type)
3. The implementation satisfies the declared preconditions and postconditions

---

## Transactions

| Pattern | Meaning |
|---------|---------|
| `<Action> shall be atomic` | Declares that an action's steps must all succeed or all roll back |
| `If <Action> fails, then <Rollback Action> shall occur` | Declares a rollback rule for a failed action |

Transactions are specification logic, not persistence. The spec would declare **what** must be atomic and **what** to undo. The portable business engine and surrounding infrastructure would handle **how** to persist and roll back.

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

- Keywords would likely be **case-insensitive** for readability
- Semicolons (`;`) would separate conditions in a list
- Indentation may be significant for nested conditions
- The grammar should reject ambiguous natural language patterns at parse time
- Primitive datatypes such as `int`, `float`, `bool`, or `varchar` should remain out of the grammar

---

## The Computational Fragment

The intended grammar is **not** Turing-complete. It deliberately omits:

| Missing | Lambda Calculus Equivalent | Why It's Missing |
|---------|--------------------------|------------------|
| Recursion | Y combinator | Algorithms are physical, not logical |
| Let bindings | `(λx.M) N` | Intermediate computation belongs in the algorithm layer |
| Arithmetic | Church numerals | Numeric operations are algorithmic |
| Pattern matching | Church encodings | Case analysis is implementation detail |
| Collections | Church lists | Aggregation, sorting, grouping are algorithmic |

This is not intended as a gap. It is a boundary.

The intended grammar would cover the **logical fragment** of lambda calculus: propositional logic, predicate logic, quantification, and boolean combinators. That is sufficient for specification — rules, constraints, permissions, and state.

Computation would live in the algorithm layer. Algorithms could be written in a general-purpose language (e.g., Haskell). The spec would bind to them by reference, not by embedding. The compiler would verify that the binding is correct.

| Concern | Where It Lives | Example |
|---------|---------------|---------|
| Specification logic | Spex grammar | "An order over $1000 gets a 10% discount" |
| Algorithm binding | Spex grammar | "An Order shall use CalculateDiscount" |
| Algorithm implementation | General-purpose language | The Haskell function that computes the discount |
| State responsibility | Spex grammar | "The Order History shall be persistent" |

The spec would declare *what* and *which implementation*. The algorithm would implement *how*. The goal is for the grammar to be complete for *what* and *which*.
