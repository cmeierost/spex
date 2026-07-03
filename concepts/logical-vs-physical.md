# Logical vs Physical World

## The Separation

The Spex direction aims to enforce a strict boundary between two realms:

```
┌─────────────────────────────────────────────────────────┐
│                   LOGICAL WORLD                         │
│                                                         │
│  "An Order shall be deemed Shipped only if all          │
│   Items are available and Payment has Cleared."         │
│                                                         │
│  • Business rules                                      │
│  • Legal constraints                                   │
│  • Operational policies                                │
│  • Actor permissions                                   │
│  • State transitions                                   │
│                                                         │
│  IMMORTAL · AGNOSTIC · HUMAN-READABLE                  │
└─────────────────────────────────────────────────────────┘
                          │
                          │  (1:1 isomorphism)
                          ▼
┌─────────────────────────────────────────────────────────┐
│                   PHYSICAL WORLD                        │
│                                                         │
│  • SQL tables / NoSQL collections                       │
│  • REST endpoints / gRPC services                       │
│  • Docker containers / Kubernetes pods                  │
│  • Load balancers / CDNs                               │
│  • Message queues / event buses                        │
│                                                         │
│  DISPOSABLE · INVISIBLE · RUNTIME-MANAGED              │
└─────────────────────────────────────────────────────────┘
```

## What Lives Where

### Logical (In The Spec)

- Entities and their relationships
- Rules governing state transitions
- State responsibility, such as what the system must keep, may forget, or receives from another authority
- Named outside authorities and boundary concepts
- Actor roles and permissions
- User intents and allowed state modifications
- Business constraints and invariants
- Value constraints such as ranges, precision, and text length
- Declarative views (who can see what, when)

### Physical (Not In The Spec)

- Database schemas and indexes
- API routes and HTTP methods
- Server configurations and scaling policies
- Authentication token formats
- File storage backends
- UI rendering, layout, input devices, and human-computer interaction mechanics
- Primitive encodings such as integer widths, decimal storage, and string column types

These are implementations of nonfunctional requirements. They exist to satisfy performance, durability, security, integration, accessibility, usability, and operational constraints, not to define the business rules themselves.

## Infrastructure is outside the spec

The intended Spex grammar should have no keywords for infrastructure. You should not write `CREATE TABLE`, `POST /api/orders`, `spawn thread`, `open socket`, `use Redis`, or `scale to three replicas` in a Spex specification because the language should provide no syntax for those concerns.

There is no I/O in the business specification. There is no persistence mechanism, only requirements about long-lived state. There is no network, only requirements about communication, availability, latency, and trust boundaries. There is no UI rendering model, only requirements about user intent, permitted actions, observable business state, allowed state modifications, and guidance. There is no load balancing or parallel programming, only requirements about throughput, consistency, and performance.

The goal is for this to be more than a convention. It should become a **grammatical impossibility**.

## State responsibility is logical

State is a business concept before it is a storage concern. A specification may need to say what the system is responsible for keeping, what it may forget, and what another actor or system is authoritative for.

| Specification responsibility | Meaning | Physical implementation |
|------------------------------|---------|-------------------------|
| The system must keep this state | Losing it would violate the business contract | Database, event log, snapshot, replicated storage |
| The system may forget this state | Losing it is allowed because the state can expire, be recomputed, or be asked for again | Memory, cache, session storage, recomputation |
| Another authority provides this state | The system is not responsible for maintaining the truth of the state; it only uses the authoritative value under declared rules | API call, message subscription, file import, human input |

The specification can define the required lifetime, authority, consistency, visibility, and recovery meaning of state. It should not define the storage engine, protocol, queue, cache, or synchronization mechanism used to satisfy that meaning.

State that the system may forget still needs a clear lifetime. The spec should be able to say that state lives only during a process, a user session, or a transaction. Those are business-relevant boundaries because they define when losing the state is allowed.

If the responsibility is underspecified, the system should ask for clarification. A useful specification should say something like: "The Payment Provider is authoritative for Payment Status." It may also need to define how stale that information may be, whether the system may proceed without it, and what should happen when the Payment Provider is unavailable.

## Outside authorities are named concepts

Outside systems and actors should be named in the specification glossary before they are used. The point is not to specify their protocol, endpoint, or deployment. The point is to give the reader and compiler a stable concept name.

For example, the spec may define **Payment Provider** as an outside authority. Later clauses may then say that the Payment Provider is authoritative for Payment Status. A clause that mentions an undefined outside concept should fail to compile, just as an undefined entity or actor should fail.

This makes the spec easier to read. When a business rule mentions the Payment Provider, the reader can jump to the glossary and see what kind of outside concern is being referenced, without being forced into infrastructure details.

## Infrastructure binds through boundary ports

Infrastructure still has to connect to the specification. It does so through **boundary ports**: named boundary points where the outside world may provide input, observe output, store state, invoke actions, or satisfy nonfunctional requirements.

The spec defines the meaning of those ports. The physical layer decides how to implement them.

| Spec concern | Infrastructure binding |
|--------------|------------------------|
| An actor performs an action | API route, message handler, CLI command, UI event |
| An entity has long-lived state | Database row, document, event stream, cache entry |
| A view exposes permitted data | Query endpoint, projection, report, UI read model |
| A transaction must be atomic | Database transaction, saga, compensating workflow |
| A workflow must meet latency requirements | Parallel execution, queueing, scaling, locality |

## Nonfunctional requirements shape architecture

Nonfunctional requirements define the architectural pressure on boundary ports. If the spec requires low latency, high availability, auditability, or regional resilience, the infrastructure around the portable, distributable business engine can choose a network topology, storage strategy, replication model, or execution plan that satisfies those requirements without changing the business meaning.

Network is not specified as sockets, protocols, or routes. It is specified through requirements about communication, availability, latency, and trust boundaries. Persistence is not specified as tables or documents. It is specified through requirements about long-lived state, consistency, durability, auditability, and recovery.

## Parallelism is derived, not declared

Parallelism should not be written into the business spec as threads, workers, queues, or replicas. It should be derived from declared dependencies when nonfunctional requirements demand it.

Transactions and data dependencies are the key inputs. If the specification defines which actions are atomic, which inputs are read, which outputs are produced, and which state changes must happen together, the compiler and surrounding infrastructure can reason about what may run in parallel without changing the business contract.

## Primitive types are physical

The spec should not declare primitive implementation types such as `int`, `float`, `bool`, or `varchar(500)`. It should declare the semantic constraint instead: a number's range, an amount's precision, a text's maximum length, or an enum-like set of permitted values.

The compiler can then select the required value semantics, and infrastructure can choose a physical representation that satisfies them. If the constraint is not precise enough to choose safely, the system should ask for a more exact specification rather than silently guessing.
