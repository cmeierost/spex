# Grammar Overview

## Controlled English for Specifications

Spex explores the possibility of using a strict subset of English — conceptually evolving from [Attempto Controlled English (ACE)](https://attempto.informatik.uni-hamburg.de/) — that non-programmers could read while remaining precise enough for mathematical evaluation.

## Design Goals

1. **Readable by non-programmers** — CEOs, lawyers, business analysts can understand and sign the spec
2. **Unambiguous** — every sentence has exactly one interpretation
3. **1:1 mappable to lambda calculus** — every clause would translate to a pure mathematical expression
4. **Grammatically enforced boundaries** — physical concepts would be excluded because the grammar would have no words for them

## Grammar Characteristics

### What IS Allowed

- Declarative rules about entities and their relationships
- Declarative constraints on values such as ranges, precision, and length
- Declarative state responsibilities: what the system must keep, may forget, or receives from another authority
- Named outside authorities and boundary concepts that must be defined before use
- Conditional permissions ("may do X if Y")
- State transition constraints
- Quantified statements ("all", "at least one", "none")
- Logical connectors ("and", "or", "not", "if and only if")
- Transactional atomicity ("this action is atomic", "if this fails, undo that")

### What IS NOT Allowed

- Imperative commands ("do this", "call that")
- Physical constructs (databases, APIs, servers)
- Persistence mechanisms (tables, indexes, savepoints, isolation levels)
- Side effects (logging, network calls, file I/O)
- Ambiguous quantifiers ("some", "many", "usually")
- Temporal vagueness ("soon", "later", "eventually")
- **Algorithms** — computation, iteration, recursion, arithmetic

### Persistence Is Physical

Persistence mechanisms are infrastructure. In this design direction, the spec would not declare tables, indexes, savepoints, or isolation levels. Infrastructure around the portable business engine would handle how data is stored and retrieved.

State responsibility is different. The spec may declare what state the system must keep, what it may forget, and which actor or system is authoritative for a value. What remains physical is the mechanism used to satisfy that declaration.

Outside authorities should be named concepts, not ad hoc strings. If a rule mentions a Payment Provider, the Payment Provider should be defined in the specification glossary before it can be used.

Transactions are different. A transaction is a business rule: "these changes happen together, or none of them do." The spec would declare the atomicity boundary and the rollback behavior. The portable business engine and surrounding infrastructure would enforce it together.

| Concern | Where It Lives | Example |
|---------|---------------|---------|
| Persistent state | Spex grammar | "The Order History shall be persistent" |
| Forgettable state | Spex grammar | "The Cart Contents may be forgotten after the Session ends" |
| Process-scoped state | Spex grammar | "The Import Buffer may be forgotten when the Process ends" |
| Transaction-scoped state | Spex grammar | "The Reservation Hold may be forgotten when Placing an Order ends" |
| Outside authority | Spex grammar | "The Payment Provider shall be authoritative for Payment Status" |
| Transaction rule | Spex grammar | "Placing an Order shall be atomic" |
| Rollback rule | Spex grammar | "If Placing an Order fails, release the reserved Inventory" |
| Persistence mechanism | Infrastructure | How persistent state is physically preserved |
| Savepoints, isolation | Infrastructure | How rollback is physically executed |

### Primitive types are not the specification

The specification should constrain **values**, not hard-code machine-level primitive types.

For example, the spec may need to say:

- a number must be between 0 and 100;
- a monetary amount must support exactly 2 decimal places; or
- a text field must contain no more than 500 words.

Those are specification constraints. They describe business meaning. They do **not** require the spec to declare `int`, `float`, `decimal`, `bool`, or `varchar(500)`.

The compiler should select the required value semantics, and infrastructure should choose a physical representation that satisfies the declared constraint. If multiple representations are possible and the choice would affect meaning, portability, or correctness, the system should reject the underspecified clause and ask for a more exact specification.

| Concern | Where It Lives | Example |
|---------|---------------|---------|
| Value constraint | Spex grammar | "The Discount Rate shall be between 0 and 1" |
| Precision rule | Spex grammar | "The Settlement Amount shall use exactly 2 decimal places" |
| Physical primitive type | Compiler / runtime | `decimal(18,2)` vs scaled integer |
| Storage encoding | Runtime / infrastructure | SQL decimal, JSON number, binary fixed-point |

### Algorithms Are Physical

Algorithms are not specification logic. They are implementation detail — a physical concern, like infrastructure.

The grammar would not need to express "how to calculate a discount tier" or "how to sort a list." Those are computational problems, solved in a general-purpose language (e.g., Haskell). The spec would declare the rule ("an order over $1000 gets a 10% discount"); the algorithm would implement the calculation.

The binding is **inverted**: the spec would declare a named contract point, and the algorithm would declare that it implements that contract. This is hexagonal architecture applied to specifications — the spec defines the port, the algorithm is the adapter.

| Concern | Where It Lives | Example |
|---------|---------------|---------|
| **Specification logic** | Spex grammar | "An order over $1000 gets a 10% discount" |
| **Contract point** | Spex grammar | "An Order shall compute its Discount via CalculateDiscount" |
| **Algorithm implementation** | General-purpose language | Haskell function that declares `Implements: CalculateDiscount` |
| **State responsibility** | Spex grammar | "The Order History shall be persistent" |

The intended grammar would be complete for specification logic and deliberately incomplete for computation.

## Example Snippet

```
An Order shall consist of at least one Item.

An Order shall be deemed Shipped if and only if:
  - the Order is Confirmed; and
  - all Items of the Order are Available.

A Customer may Cancel an Order if and only if:
  - the Order belongs to the Customer; and
  - the Order is not Shipped.
```

## Next Steps

- [Candidate grammar keywords](./keywords.md) — The current keyword patterns and boundaries being explored
