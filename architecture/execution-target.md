# Portable Execution Target

## Overview

The execution target is a proposed portable and distributable substrate for evaluated Spex specifications. It should be closer in spirit to WebAssembly than to a bespoke application framework: a compiled business engine can run in many physical environments, and can be distributed as communicating parts, while preserving the same business meaning.

The execution target is not responsible for everything around the business engine. It should not decide databases, networks, deployment topology, queues, caches, or scaling strategy. Those belong to the surrounding infrastructure.

The compiler should be able to lower the formal specification into portable executable parts with declared inputs, outputs, state responsibilities, dependencies, and boundary ports. Independent infrastructure can then connect to those inputs and outputs according to nonfunctional requirements.

Distribution is semantic, not arbitrary. The compiler should produce separate parts only where the specification's dependencies, transactions, state responsibilities, and boundary ports allow it. Infrastructure may deploy those parts separately, but it must not change the business meaning they preserve.

## Architecture Principles

### 1. The Spec Knows Nothing About Physics

The specification contains zero references to:
- Database engines
- Network protocols
- File systems
- Authentication mechanisms

### 2. The Compiler Defines Parts; Infrastructure Binds Them

Given a spec, the compiler would determine:
- Which executable parts are needed
- Which boundary ports each part exposes or consumes
- Which state must be persistent, may be forgotten, or comes from another authority
- Which parts may communicate with each other
- Which parts must remain together because of transaction or consistency requirements

Given those compiled parts, infrastructure would determine:
- How boundary ports are connected
- How persistent state is physically preserved
- How outside authorities are reached
- How parts are deployed, scaled, replicated, and monitored
- Which physical choices satisfy the nonfunctional requirements

These are not business decisions. They are implementations of nonfunctional requirements such as latency, durability, throughput, interoperability, and operational resilience.

### 3. The Mapping Is Disposable

If the physical binding swapped PostgreSQL for MongoDB, or REST for gRPC, the spec would not change. The mapping layer would be outside the business specification.

What changes in that swap is not the business meaning of the specification. What changes is how the surrounding infrastructure satisfies nonfunctional requirements in a particular environment.

## Execution Responsibilities

```
┌─────────────────────────────────────────────┐
│        PORTABLE EXECUTION TARGET            │
│                                             │
│  ┌─────────────────────────────────────┐   │
│  │  Evaluated Spec Module             │   │
│  │  - Receives compiled semantics     │   │
│  │  - Evaluates permissions & rules   │   │
│  │  - Preserves business meaning      │   │
│  └─────────────────────────────────────┘   │
│                                             │
│  ┌─────────────────────────────────────┐   │
│  │  State Boundary                     │   │
│  │  - Marks persistent state          │   │
│  │  - Marks forgettable state         │   │
│  │  - Marks outside authorities       │   │
│  └─────────────────────────────────────┘   │
│                                             │
│  ┌─────────────────────────────────────┐   │
│  │  Boundary Ports                     │   │
│  │  - Expose permitted actions        │   │
│  │  - Receive authoritative inputs    │   │
│  │  - Emit contract-shaped outputs    │   │
│  └─────────────────────────────────────┘   │
│                                             │
│  ┌─────────────────────────────────────┐   │
│  │  Part Communication                 │   │
│  │  - Links compiled parts            │   │
│  │  - Follows declared dependencies   │   │
│  │  - Supports derived parallelism    │   │
│  └─────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

The boxes above are responsibilities of the portable business engine. They are not a database, web server, message broker, deployment platform, or orchestration layer. Infrastructure connects to them.

## Proactive Guidance

When an action is blocked, the executable part would ideally return **guidance** instead of a generic error:

```
Action: Ship Order #42
Result: BLOCKED
Guidance: "To ship this order, the following must be true:
  ✓ Order is Confirmed
  ✗ Item 'Widget-X' is not Available (2 in stock, 5 ordered)
  ✓ You hold the Warehouse role"
```

This guidance would be derived directly from the spec's precondition list, with no manual error messages needed.
