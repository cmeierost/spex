# Universal Runtime Interpreter

## Overview

The universal runtime interpreter is the physical engine that evaluates Spex specifications. It is the only component that knows about the physical world — servers, databases, networks, and containers.

## Architecture Principles

### 1. The Spec Knows Nothing About Physics

The specification contains zero references to:
- Database engines
- Network protocols
- File systems
- Authentication mechanisms

### 2. The Runtime Decides Everything Physical

Given a spec, the runtime autonomously determines:
- How to store entity state
- How to expose actions to actors
- How to scale under load
- How to handle failures

### 3. The Mapping Is Disposable

If the runtime swaps PostgreSQL for MongoDB, or REST for gRPC, the spec does not change. The mapping layer is entirely internal to the runtime.

## Runtime Responsibilities

```
┌─────────────────────────────────────────────┐
│            UNIVERSAL RUNTIME                │
│                                             │
│  ┌─────────────────────────────────────┐   │
│  │  Spec Evaluator                     │   │
│  │  - Parses Spex text                │   │
│  │  - Produces lambda expressions     │   │
│  │  - Evaluates permissions & rules   │   │
│  └─────────────────────────────────────┘   │
│                                             │
│  ┌─────────────────────────────────────┐   │
│  │  Persistence Manager                │   │
│  │  - Maps entities to storage        │   │
│  │  - Handles transactions            │   │
│  │  - Manages backups & replication   │   │
│  └─────────────────────────────────────┘   │
│                                             │
│  ┌─────────────────────────────────────┐   │
│  │  Action Router                      │   │
│  │  - Exposes permitted actions       │   │
│  │  - Validates preconditions         │   │
│  │  - Returns guidance on failures    │   │
│  └─────────────────────────────────────┘   │
│                                             │
│  ┌─────────────────────────────────────┐   │
│  │  View Engine                        │   │
│  │  - Evaluates declarative filters   │   │
│  │  - Returns filtered data           │   │
│  │  - Respects actor permissions      │   │
│  └─────────────────────────────────────┘   │
└─────────────────────────────────────────────┘
```

## Proactive Guidance

When an action is blocked, the runtime doesn't return an error. It returns **guidance**:

```
Action: Ship Order #42
Result: BLOCKED
Guidance: "To ship this order, the following must be true:
  ✓ Order is Confirmed
  ✗ Item 'Widget-X' is not Available (2 in stock, 5 ordered)
  ✓ You hold the Warehouse role"
```

This guidance is derived directly from the spec's precondition list — no manual error messages needed.
