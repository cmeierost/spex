# Grammar Overview

## Controlled English for Specifications

Spex uses a strict subset of English — conceptually evolving from [Attempto Controlled English (ACE)](https://attempto.informatik.uni-hamburg.de/) — that any non-programmer can read while remaining precise enough for mathematical evaluation.

## Design Goals

1. **Readable by non-programmers** — CEOs, lawyers, business analysts can understand and sign the spec
2. **Unambiguous** — every sentence has exactly one interpretation
3. **1:1 mappable to lambda calculus** — every clause translates to a pure mathematical expression
4. **Grammatically enforced boundaries** — you cannot express physical concepts because the grammar has no words for them

## Grammar Characteristics

### What IS Allowed

- Declarative rules about entities and their relationships
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

Persistence is infrastructure. The spec does not declare tables, indexes, savepoints, or isolation levels. The runtime handles how data is stored and retrieved.

Transactions are different. A transaction is a business rule: "these changes happen together, or none of them do." The spec declares the atomicity boundary and the rollback behavior. The runtime enforces it.

| Concern | Where It Lives | Example |
|---------|---------------|---------||
| Transaction rule | Spex grammar | "Placing an Order shall be atomic" |
| Rollback rule | Spex grammar | "If Placing an Order fails, release the reserved Inventory" |
| Persistence | Runtime | The database that stores orders |
| Savepoints, isolation | Runtime | How the rollback is physically executed |

### Algorithms Are Physical

Algorithms are not specification logic. They are implementation detail — a physical concern, like infrastructure.

The grammar does not need to express "how to calculate a discount tier" or "how to sort a list." Those are computational problems, solved in a general-purpose language (e.g., Haskell). The spec declares the rule ("an order over $1000 gets a 10% discount"); the algorithm implements the calculation.

The binding is **inverted**: the spec declares a named contract point, and the algorithm declares that it implements that contract. This is hexagonal architecture applied to specifications — the spec defines the port, the algorithm is the adapter.

| Concern | Where It Lives | Example |
|---------|---------------|---------||
| **Specification logic** | Spex grammar | "An order over $1000 gets a 10% discount" |
| **Contract point** | Spex grammar | "An Order shall compute its Discount via CalculateDiscount" |
| **Algorithm implementation** | General-purpose language | Haskell function that declares `Implements: CalculateDiscount` |
| **Infrastructure** | Runtime configuration | The database that stores orders |

The grammar is complete for specification logic. It is deliberately incomplete for computation.

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

- [Formal Keywords](./keywords.md) — The complete set of Spex keywords and their meanings
