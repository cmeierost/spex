# Concepts Overview

Spex addresses a fundamental problem: **software engineering has been built on a broken abstraction.**

## The Problem

For decades, building software has required translating human intent through multiple lossy layers:

1. **Business requirements** (natural language, ambiguous)
2. **Technical specifications** (still ambiguous, but more structured)
3. **Source code** (precise but only to machines, not humans)
4. **Tests** (a second language to verify the first)
5. **Infrastructure configs** (yet another language for deployment)

Each layer introduces drift, misinterpretation, and technical debt. The gap between what a human *intends* and what a machine *executes* is where bugs, security vulnerabilities, and project failures live.

## The Spex Answer

Spex collapses all these layers into **one**: a mathematically precise specification written in controlled English that:

- **A human can read and sign** — no programming knowledge required
- **A machine can evaluate directly** — no code generation step
- **Is its own documentation** — no secondary artifacts
- **Is its own tests** — logical completeness is verified by the solver
- **Knows nothing about infrastructure** — the physical world is an implementation detail

## Key Questions This Section Answers

- [What are the three core invariants?](./three-invariants.md)
- [How does Spex separate logic from physics?](./logical-vs-physical.md)
- [What does "contract is code" mean?](./contract-is-code.md)
- [How does intent-driven UI work?](./intent-driven-ui.md)
