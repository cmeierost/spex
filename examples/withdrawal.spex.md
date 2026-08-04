------

withdrawal.spex.md

------


# Feature: Withdrawal

## Terms

- Money is a Decimal Number with at most 2 decimal places.
- Dollars is Money with prefix "$".

- A Customer has an Account.
- An Account has a Balance of Dollars.

- A Withdrawal Request has a Customer
- and a Requested Amount of Dollars greater than zero.

- An Outcome is either successful or declined.
- A Withdrawal has an Outcome and a Balance of Dollars.

## Rules

When a Withdrawal Request occurs, the result is a Withdrawal:

- Provided that
    its Requested Amount does not exceed
    its Customer's Account's Balance:

  - The resulting Outcome is successful.

  - The resulting Balance is its Customer's Account's Balance
    minus its Requested Amount.

- Otherwise:

  - The resulting Outcome is declined.

  - The resulting Balance is its Customer's Account's Balance.

## Examples

```gherkin
Feature: Withdrawing money

  Scenario: Successful withdrawal within balance
    Alice is a Customer
    Given Alice's Account's Balance is $234.56
    When a Withdrawal Request occurs
      whose Customer is Alice
      and whose Requested Amount is $200.00
    Then the Withdrawal's Outcome is successful
    And the Withdrawal's Balance is $34.56

  Scenario: Declined withdrawal exceeding balance
    Hamza is a Customer
    And Hamza's Account's Balance is $100.00
    When a Withdrawal Request occurs
      whose Customer is Hamza
      and whose Requested Amount is $150.00
    Then the Withdrawal's Outcome is declined
    And the Withdrawal's Balance is $100.00
```

------

End of the spex file.

------

# Concepts Demonstrated by the Withdrawal Example

The withdrawal example is intentionally small, but it already exercises a substantial part of the proposed SPEX language and compiler model.

## 1. Feature as a Compilation Boundary

```spex
# Feature: Withdrawal
```

The document defines one coherent business capability.

This introduces:

* a feature or module boundary;
* a local namespace;
* a grouping of terms, rules, and examples;
* a possible compiler input unit.

---

## 2. Primitive Type Refinement

```spex
Money is a Decimal Number with at most 2 decimal places.
```

`Money` refines the primitive type `Decimal Number`.

This introduces:

* primitive base types;
* named business types;
* constrained or refined types;
* decimal precision constraints;
* compile-time validation of values;
* independence from machine-level number representations.

The infrastructure may later choose an appropriate runtime representation, but it must preserve the declared precision semantics.

---

## 3. Transitive Type Refinement and Literal Syntax

```spex
Dollars is Money with prefix "$".
```

`Dollars` further refines `Money`.

```text
Decimal Number
  → Money
      → Dollars
```

This introduces:

* refinement of an already refined type;
* inherited constraints;
* domain-specific value types;
* typed literal syntax;
* parsing `$234.56` as a value of `Dollars`.

The `$` prefix currently acts as lexical information for SPEX literals. Whether such notation later belongs to the language, presentation metadata, or both remains a language-design question.

---

## 4. Domain Types

```spex
A Customer has an Account.
An Account has a Balance of Dollars.
```

This introduces named domain types:

* `Customer`;
* `Account`.

It also introduces:

* structured business values;
* typed properties;
* relationships between domain types;
* a domain vocabulary shared by rules and examples.

---

## 5. Typed Relationships

```spex
A Customer has an Account.
```

The compiler understands that a `Customer` provides access to an `Account`.

Likewise:

```spex
An Account has a Balance of Dollars.
```

establishes:

```text
Customer
  .Account → Account
  .Balance → Dollars
```

This makes chained expressions such as the following type-checkable:

```spex
its Customer's Account's Balance
```

---

## 6. Implicit Cardinality

The declarations currently imply scalar properties:

```spex
A Customer has an Account.
An Account has a Balance of Dollars.
```

This suggests that `has` currently means a mandatory single value.

The language must eventually define whether this means:

```text
exactly one
```

or whether cardinalities must be expressed separately.

This matters because chained access is only scalar and total when every relationship in the chain produces exactly one value.

---

## 7. Composite Input Type

```spex
A Withdrawal Request has a Customer
and a Requested Amount of Dollars greater than zero.
```

`Withdrawal Request` is a structured input type with two properties:

* `Customer`;
* `Requested Amount`.

This introduces:

* composite types;
* multiline type declarations;
* multiple typed properties;
* relationships to existing domain types.

---

## 8. Property-Level Constraints

```spex
a Requested Amount of Dollars greater than zero
```

The requested amount is not merely `Dollars`. It is a contextually refined value:

```text
Dollars where value > 0
```

This introduces:

* constraints attached to properties;
* declarative invariants;
* positive-value restrictions;
* rejection of invalid requests before rule execution.

A request for zero or a negative amount cannot satisfy the declared type.

---

## 9. Closed Union Type

```spex
An Outcome is either successful or declined.
```

This introduces:

* a closed union type;
* enumeration-like values;
* a finite set of valid alternatives;
* exhaustiveness checking;
* typed symbolic literals.

Only the following values belong to `Outcome`:

```text
successful
declined
```

---

## 10. Structured Result Type

```spex
A Withdrawal has an Outcome and a Balance of Dollars.
```

`Withdrawal` is the return type of the business decision.

It contains:

* an `Outcome`;
* a resulting `Balance`.

This introduces:

* structured return values;
* named result types;
* typed output properties;
* separation between the request and the result.

---

## 11. Event Declaration Through Use

```spex
When a Withdrawal Request occurs
```

The keyword `occurs` establishes that a `Withdrawal Request` is being handled as an event.

This introduces:

* events;
* event payloads;
* active inputs;
* reactive business rules;
* event handlers.

A separate declaration such as the following is unnecessary:

```spex
Withdrawal Request is an Event.
```

The event role is determined by its grammatical use.

---

## 12. Implicit Function Declaration

```spex
When a Withdrawal Request occurs, the result is a Withdrawal:
```

This defines a typed transformation.

Conceptually:

```typescript
function decideWithdrawal(
  request: WithdrawalRequest
): Withdrawal
```

This introduces:

* an input type;
* a return type;
* a function or transformation boundary;
* deterministic event-to-result mapping;
* a scoped rule body.

---

## 13. Current-Input Binding

Inside the rule, `its` refers to the triggering `Withdrawal Request`.

```spex
its Requested Amount
```

Conceptually:

```typescript
request.requestedAmount
```

This introduces:

* contextual pronouns;
* implicit input bindings;
* scoped property resolution;
* type-safe references to the current input.

---

## 14. Current-Result Binding

Inside the rule, `resulting` refers to the `Withdrawal` being constructed and returned.

```spex
The resulting Outcome
The resulting Balance
```

Conceptually:

```typescript
result.outcome
result.balance
```

The word `resulting` is not part of the property name. It identifies the current return value.

```text
its       → current input
resulting → current output
```

---

## 15. Nested Possessive Navigation

```spex
its Customer's Account's Balance
```

This introduces:

* natural-language property access;
* chained relationship traversal;
* possessive syntax;
* static type-checking of nested access.

Conceptually:

```typescript
request.customer.account.balance
```

The compiler resolves each step independently:

```text
Withdrawal Request
  .Customer → Customer
  .Account  → Account
  .Balance  → Dollars
```

---

## 16. Typed Comparison

```spex
its Requested Amount does not exceed
its Customer's Account's Balance
```

This corresponds to:

```typescript
request.requestedAmount <= request.customer.account.balance
```

This introduces:

* comparison operators expressed in English;
* ordering relations;
* type compatibility checking;
* comparison of values with the same business unit.

The compiler can reject comparisons between incompatible types.

---

## 17. Guarded Business Rule

```spex
Provided that ...
```

This introduces a guarded branch.

The successful outcome applies only when the stated condition is true.

This introduces:

* preconditions;
* guards;
* conditional execution;
* business decision branches.

---

## 18. Exhaustive Fallback

```spex
Otherwise:
```

`Otherwise` defines the complementary branch.

This introduces:

* fallback behaviour;
* exhaustive branching;
* total decision functions;
* prevention of unhandled valid inputs.

Every valid `Withdrawal Request` therefore produces a `Withdrawal`.

---

## 19. Typed Result Construction

```spex
The resulting Outcome is successful.
```

This assigns a value to a property of the return value.

The compiler can verify that:

* `Outcome` exists on `Withdrawal`;
* `successful` is a valid `Outcome`;
* the assigned value has the correct type;
* the property is assigned on every execution path.

---

## 20. Arithmetic on Refined Values

```spex
The resulting Balance is
its Customer's Account's Balance
minus its Requested Amount.
```

This introduces:

* arithmetic operators expressed in English;
* subtraction of refined decimal values;
* preservation of units or business types;
* typed arithmetic results.

Because both operands are `Dollars`, the result is also `Dollars`.

Conceptually:

```typescript
result.balance =
  request.customer.account.balance -
  request.requestedAmount
```

---

## 21. Constraint Propagation

The successful branch establishes:

```text
Requested Amount <= Balance
```

and then computes:

```text
Balance - Requested Amount
```

The compiler or solver may therefore infer:

```text
resulting Balance >= 0
```

This introduces:

* branch-sensitive reasoning;
* refinement propagation;
* range inference;
* proofs based on preconditions;
* prevention of invalid arithmetic results.

---

## 22. Explicit Unchanged-State Behaviour

In the declined branch:

```spex
The resulting Balance is its Customer's Account's Balance.
```

The unchanged value is stated explicitly.

This introduces:

* explicit preservation of state;
* deterministic fallback behaviour;
* avoidance of implicit defaults;
* complete result construction.

SPEX does not need to assume that an omitted assignment means “unchanged.”

---

## 23. Definite Assignment

Both branches assign:

* `Outcome`;
* `Balance`.

This allows the compiler to verify that every returned `Withdrawal` is complete.

This introduces:

* definite assignment;
* branch completeness;
* total result construction;
* compile-time checking of all return properties.

---

## 24. Pure Business Transformation

The rule reads the existing account balance and returns a new business result. It does not directly update a database or invoke infrastructure.

Conceptually:

```text
Withdrawal Request + current Balance
  → Withdrawal containing Outcome and resulting Balance
```

This introduces:

* pure transformations;
* previous state as input;
* resulting state as output;
* absence of side effects in business logic;
* separation of decision from persistence.

---

## 25. Infrastructure Independence

The specification contains no:

* database;
* storage command;
* API;
* network protocol;
* queue;
* framework;
* deployment instruction;
* UI widget;
* persistence implementation.

This demonstrates:

* separation of business meaning from infrastructure;
* portable business logic;
* technology-independent specification;
* deterministic compilation into a business core.

---

## 26. Executable Examples

```spex
## Examples
```

The examples are part of the specification process.

They introduce:

* shared examples for discussion with domain experts;
* executable acceptance scenarios;
* validation of shared understanding;
* later reuse as integration tests.

The examples do not define missing business rules. The rules remain authoritative.

---

## 27. Gherkin-Like Scenario Structure

```gherkin
Scenario: Successful withdrawal within balance
```

Each scenario contains:

* an initial context;
* an occurring event;
* an expected business result.

This makes the example readable to domain experts while remaining executable by the SPEX compiler.

---

## 28. Explicitly Typed Example Values

```gherkin
Alice is a Customer
```

This introduces a named value with a known type:

```text
Alice : Customer
```

Unlike ordinary Gherkin, SPEX cannot rely on hidden step definitions or fixture code to create or infer Alice.

The declaration is required so that the compiler can type-check expressions such as:

```spex
Alice's Account's Balance
```

This introduces:

* example-local variable declarations;
* explicit type construction;
* scenario-local symbol tables;
* independently compilable examples;
* elimination of hidden fixture semantics.

---

## 29. Example State Setup

```gherkin
Given Alice's Account's Balance is $234.56
```

This defines the initial state for the scenario.

It introduces:

* fixture construction;
* nested property assignment;
* typed literal parsing;
* pre-event state;
* type-checked scenario setup.

The compiler knows that `$234.56` is a value of `Dollars`.

---

## 30. Event Construction in Examples

```gherkin
When a Withdrawal Request occurs
  whose Customer is Alice
  and whose Requested Amount is $200.00
```

This constructs and raises a typed event.

Conceptually:

```typescript
{
  customer: Alice,
  requestedAmount: dollars("200.00")
}
```

This introduces:

* event instantiation;
* named property binding;
* multiline object construction;
* type-checked event payloads;
* occurrence of a business event.

---

## 31. Relative Binding Through `whose`

```spex
whose Customer is Alice
```

`whose` refers to the `Withdrawal Request` introduced immediately before it.

This introduces:

* relative clauses;
* contextual object construction;
* property binding to a newly introduced value;
* natural-language record initialisation.

---

## 32. Contextual Result Reference

```gherkin
Then the Withdrawal's Outcome is successful
```

`the Withdrawal` refers to the result produced by the preceding `Withdrawal Request`.

This introduces:

* scenario-local result bindings;
* definite-article resolution;
* contextual name lookup;
* references to the latest result of a known type.

---

## 33. Typed Result Assertions

```gherkin
Then the Withdrawal's Outcome is successful
And the Withdrawal's Balance is $34.56
```

This introduces:

* observable result assertions;
* typed equality;
* comparison with expected business values;
* verification of business outcomes;
* executable documentation.

The compiler can verify that each asserted property exists and that the expected value has a compatible type.

---

## 34. Exact Decimal Equality

```spex
the Withdrawal's Balance is $34.56
```

This introduces:

* exact comparison of decimal values;
* typed literals with domain notation;
* precision-aware equality;
* avoidance of accidental binary floating-point semantics.

---

## 35. Positive and Negative Scenarios

The examples cover both branches:

* a successful withdrawal;
* a declined withdrawal.

This introduces:

* examples for multiple outcomes;
* branch-oriented validation;
* discussion of fallback behaviour;
* confirmation of shared understanding across decision paths.

The examples do not prove the rules, but they help humans detect misunderstandings.

---

## 36. Deterministic Semantics

For every valid request and input state, the rule produces exactly one result.

This introduces:

* deterministic execution;
* total business decisions;
* absence of probabilistic interpretation at runtime;
* equivalent observable behaviour from every conforming compiler.

---

## 37. Potential Transaction Boundary

The example implicitly contains a sequence that may need atomic execution:

```text
read current balance
check requested amount
calculate resulting balance
preserve the accepted result
```

This exposes future concepts such as:

* transaction boundaries;
* atomic state changes;
* concurrent modification;
* state versioning;
* consistency requirements.

The business specification defines the valid decision. Infrastructure must preserve that decision under concurrency and failure.

---

## 38. State Responsibility

The account balance is accessed through:

```spex
its Customer's Account's Balance
```

A larger SPEX specification may need to define:

* which part of the business engine owns the balance;
* whether the balance is supplied by an external authority;
* who is permitted to read or modify it;
* which boundary port provides it.

This introduces future concepts around:

* state ownership;
* state authority;
* external data sources;
* boundary ports;
* capabilities.

---

## 39. Compiler-Guided Language Design

The example itself demonstrates the intended SPEX authoring process.

The compiler establishes that:

* `Alice` must be introduced as a `Customer`;
* property chains must resolve through declared types;
* `its` refers to the request;
* `resulting` refers to the return value;
* every result property must be assigned;
* literals must satisfy their declared constraints;
* comparisons and arithmetic must use compatible types.

The compiler therefore does more than translate syntax. It actively prevents apparently readable wording from drifting away from the language's formal semantics.

---

## Concept Summary

| Area              | Concepts demonstrated                                                |
| ----------------- | -------------------------------------------------------------------- |
| Feature structure | Compilation unit, namespace, terms, rules, examples                  |
| Types             | Primitive types, refinements, inherited constraints, unions          |
| Values            | Precision, positivity, typed literals, domain notation               |
| Domain model      | Entities, properties, relationships, cardinality                     |
| Events            | Event occurrence, typed payloads, reactive rules                     |
| Functions         | Typed input, typed result, deterministic transformation              |
| Scope             | `its`, `resulting`, `whose`, contextual result references            |
| Logic             | Guards, comparisons, fallback, exhaustive branching                  |
| Arithmetic        | Typed subtraction, constraint propagation                            |
| Results           | Construction, property assignment, definite assignment               |
| Examples          | Typed fixtures, state setup, events, assertions                      |
| Verification      | Type checking, structural completeness, solver opportunities         |
| Architecture      | Pure business logic, state responsibility, infrastructure separation |
| Authoring         | Compiler-guided clarification and language design                    |

Even though the withdrawal feature is small, it already exercises much of the conceptual foundation required for SPEX: a controlled domain language, a strong type system, deterministic business transformations, executable examples, and a clear boundary between specified business meaning and technical implementation.
