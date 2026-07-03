# Contract is Code

## Single Source of Truth

In the Spex direction, the specification is the **authoritative business artifact**. The goal is not that every technical file disappears. The goal is that business meaning, permissions, state rules, and invariants stop being scattered across requirements documents, application code, test suites, and infrastructure declarations.

## The Elimination Matrix

| Traditional World | Spex World |
|------------------|------------|
| `requirements.docx` | The spec |
| `user-stories/` (Jira) | The spec |
| `src/**/*.ts` | Business intent moves toward the spec; technical implementation becomes subordinate |
| `tests/**/*.spec.ts` | Business-rule verification moves toward the solver's completeness check |
| `docs/api.md` | The spec |
| `ARCHITECTURE.md` | The spec defines the business contract; architecture explains implementation strategy |
| `Terraform/` or `cloudformation/` | Infrastructure around the execution target implements nonfunctional requirements |
| `docker-compose.yml` | Infrastructure around the execution target implements nonfunctional requirements |

## How It Works

### 1. The Spec Defines Everything Logical

A Spex specification would declare the logical core:
- What entities exist
- What rules govern them
- Who can do what, when
- What views are available

### 2. The Solver Verifies Completeness

The mathematical compiler would check:
- Are all state transitions covered?
- Are there contradictions between rules?
- Are there unreachable states?
- Are there ambiguous conditions?

### 3. The Compiler Produces Business-Engine Parts

In the target model, the compiler would lower the accepted specification into portable, distributable business-engine parts. Those parts expose declared inputs, outputs, state responsibilities, and boundary ports. Infrastructure connects to those ports and satisfies nonfunctional requirements without redefining business meaning.

## The implication: "Wrong spec" before "wrong code"

When something goes wrong:

- **Traditional:** "The code has a bug" → find the function, fix the logic, re-deploy
- **Spex:** first ask whether the spec was incomplete, contradictory, or underspecified before treating the issue as an implementation defect

This does not mean there can never be implementation defects. It means the specification should become the primary place where business correctness is expressed, reviewed, and verified. Algorithmic and infrastructure layers should implement that contract rather than silently redefining it.
