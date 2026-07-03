# AGENTS.md — Spex Knowledge Base

## What This Repo Is

This is a **documentation-only knowledge base** for the Spex project: a paradigm where executable specification contracts replace traditional programming. No source code lives here — only markdown documentation that captures concepts, architecture, grammar, and comparisons.

## Folder Structure

```
├── README.md              — Project front door: elevator pitch + quick links
├── manifest.md            — The single authoritative manifest. All core claims originate here.
├── architecture/          — How the system works internally
│   ├── design-loop.md     — LLM-assisted authoring cycle
│   ├── mathematical-bridge.md — Controlled English → Lambda Calculus pipeline
│   ├── persistence.md     — Data storage model (writing is infrastructure)
│   └── runtime.md         — Universal runtime interpreter
├── concepts/              — Foundational ideas and principles
│   ├── overview.md        — Entry point for concepts section
│   ├── three-invariants.md — The three core invariants
│   ├── logical-vs-physical.md — Separation of business logic from infrastructure
│   ├── contract-is-code.md — Single source of truth principle
│   └── intent-driven-ui.md — Declarative UI from permissions
├── comparison/            — How Spex differs from existing approaches
│   └── state-of-the-art.md — Kimi/Devin, MDA, Cloud IaC
├── grammar/               — The Spex specification language
│   ├── overview.md        — Grammar design goals and characteristics
│   └── keywords.md        — Formal keyword patterns with lambda-calculus mapping
└── [future folders]       — See "Adding New Topics" below
```

### Folder Purposes

| Folder | Purpose | Examples |
|--------|---------|----------|
| `architecture/` | Internal system design, components, data flow | Runtime, solver, persistence, action router |
| `concepts/` | Foundational principles, philosophy, mental models | Invariants, separation of concerns, intent-driven UI |
| `comparison/` | Positioning against other tools, frameworks, paradigms | AI code gen, MDA, Terraform, ACE |
| `grammar/` | The Spex language itself: keywords, syntax, patterns | Keyword reference, BNF, example specs |

## Writing Conventions

### Tone and Voice

- **Authoritative but accessible** — write for CEOs, lawyers, and engineers equally
- **Declarative, not imperative** — describe what _is_, not what _to do_
- **No hedging** — avoid "might," "could," "perhaps." Spex makes strong claims.
- **First-person plural for the project** — "Spex eliminates programming" not "we think it might"

### Formatting Rules

- **Headings:** ATX style (`#`, `##`, `###`), sentence case
- **Code blocks:** Use fenced blocks with language hint (`markdown`, `text`) for Spex spec examples
- **Tables:** Prefer tables over bullet lists for comparisons and keyword references
- **ASCII diagrams:** Use box-drawing characters (`┌─┐│└┘`) for architecture diagrams; keep them narrow (<80 chars)
- **Cross-references:** Use relative markdown links (`[link](./file.md)`)
- **Bold** for key terms on first introduction; _italics_ for emphasis in running text

### Spex-Specific Terminology

| Term | Use | Don't Use |
|------|-----|-----------|
| Spex | The project name | Holy Grail, the system, this thing |
| spec | A Spex specification document | contract, requirements doc, source code |
| solver | The mathematical compiler/verifier | compiler, type checker, engine |
| runtime | The universal runtime interpreter | backend, server, infrastructure layer |
| actor | A role that performs actions | user, client, principal |
| entity | A business object defined in the spec | model, class, table, resource |
| action | A permitted operation (e.g., Cancel, Ship) | method, function, endpoint, API call |
| state | A declared status of an entity (e.g., Shipped) | status, phase, mode |

### Spex Spec Example Style

When showing Spex specification text in documentation, use this format:

````
```
An Order shall consist of:
  - an Order Number;
  - a Customer;
  - at least one Item; and
  - a Status.

A Customer may Cancel an Order if and only if:
  - the Order belongs to the Customer; and
  - the Order is not Shipped.
```
````

- No language hint on the fence (bare ```` ``` ````) to distinguish Spex spec from code
- Indent list items with 2 spaces
- Semicolons separate conditions; "and" before the last item
- Entity names are PascalCase

## Adding New Topics

### Deciding Where a Document Belongs

Ask: **What question does this document answer?**

- "How does X work inside the system?" → `architecture/`
- "Why do we believe X?" → `concepts/`
- "How is X different from Y?" → `comparison/`
- "How do I write X in Spex?" → `grammar/`

### Naming Files

- **Lowercase, hyphen-separated:** `persistence-model.md`, `design-loop.md`
- **Noun-oriented:** `three-invariants.md` not `defining-three-invariants.md`
- **No numbers in filenames** unless ordering is critical (avoid `01-intro.md`)

### New Folder Criteria

Create a new top-level folder only when a topic area has **3+ documents** that don't fit existing folders. Suggested future folders:

- `patterns/` — Common specification patterns and anti-patterns
- `tutorials/` — Step-by-step walkthroughs of authoring a spec
- `glossary/` — Term definitions if the vocabulary grows large enough
- `adr/` — Architecture Decision Records for the Spex project itself

### Cross-Referencing

- Every new document should link back to at least one existing document
- Section `overview.md` files should list all documents in their section with brief descriptions
- The `manifest.md` is the canonical source — if a new document contradicts it, update the manifest first

## Session Memory Guidelines

When working in this repo across multiple conversations:

- Save **structural decisions** (new folders, naming changes) to `/memories/repo/`
- Save **in-progress document outlines** to `/memories/session/`
- Don't save content that's already in the markdown files — the repo _is_ the memory
