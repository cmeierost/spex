# Contract is Code

## Single Source of Truth

In Spex, the specification is the **only** artifact. Everything that traditionally exists as separate files, documents, or configurations is subsumed into one text document.

## The Elimination Matrix

| Traditional World | Spex World |
|------------------|------------|
| `requirements.docx` | The spec |
| `user-stories/` (Jira) | The spec |
| `src/**/*.ts` | The spec |
| `tests/**/*.spec.ts` | The solver's completeness check |
| `docs/api.md` | The spec |
| `ARCHITECTURE.md` | The spec |
| `Terraform/` or `cloudformation/` | The runtime decides |
| `docker-compose.yml` | The runtime decides |

## How It Works

### 1. The Spec Defines Everything Logical

A single Spex document declares:
- What entities exist
- What rules govern them
- Who can do what, when
- What views are available

### 2. The Solver Verifies Completeness

The mathematical compiler checks:
- Are all state transitions covered?
- Are there contradictions between rules?
- Are there unreachable states?
- Are there ambiguous conditions?

### 3. The Runtime Executes Directly

No code generation step. The spec is evaluated directly by the universal interpreter, which maps logical operations to physical execution.

## The Implication: "Wrong Spec" Not "Wrong Code"

When something goes wrong:

- **Traditional:** "The code has a bug" → find the function, fix the logic, re-deploy
- **Spex:** "The spec was incomplete" → clarify the rule, re-verify, done

There is no layer where the translation can go wrong because there is no translation.
