# Logical vs Physical World

## The Separation

Spex enforces a strict boundary between two realms:

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
- Actor roles and permissions
- Business constraints and invariants
- Declarative views (who can see what, when)

### Physical (Not In The Spec)

- Database schemas and indexes
- API routes and HTTP methods
- Server configurations and scaling policies
- Authentication token formats
- File storage backends

## The Grammar Enforces This

The Spex grammar simply has no keywords for physical constructs. You cannot write `CREATE TABLE` or `POST /api/orders` in a Spex specification because the language provides no syntax for it.

This is not a convention — it is a **grammatical impossibility**.
