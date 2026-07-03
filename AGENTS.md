# AGENTS.md — Spex Knowledge Base

## What This Repo Is

This is a **documentation-only research knowledge base** for the Spex project: a brainstorming experiment about whether signed, compiler-checked specifications can become the authoritative business artifact. No source code lives here — only markdown documentation that captures concepts, architecture sketches, candidate grammar, comparisons, and prior art.

## Folder Structure

```
├── README.md              — Project front door: research framing + quick links
├── manifest.md            — Current working thesis. Core claims should stay aligned here.
├── architecture/          — Proposed design sketches
│   ├── design-loop.md     — LLM-assisted authoring cycle
│   ├── mathematical-bridge.md — Controlled English → Lambda Calculus pipeline
│   ├── persistence.md     — Data storage model (writing is infrastructure)
│   └── execution-target.md — Portable, distributable business engine target
├── concepts/              — Foundational ideas and principles
│   ├── overview.md        — Entry point for concepts section
│   ├── three-invariants.md — The three core invariants
│   ├── logical-vs-physical.md — Separation of business logic from infrastructure
│   ├── contract-is-code.md — Single source of truth principle
│   └── intent-driven-ui.md — Declarative UI from permissions
├── comparison/            — How Spex differs from existing approaches
│   └── state-of-the-art.md — Kimi/Devin, MDA, Cloud IaC
├── grammar/               — Candidate controlled-English grammar
│   ├── overview.md        — Grammar design goals and characteristics
│   └── keywords.md        — Formal keyword patterns with lambda-calculus mapping
├── examples/              — Placeholder for future examples and thought experiments
├── reference/             — Placeholder for prior art and external references
└── [future folders]       — See "Adding New Topics" below
```

### Folder Purposes

| Folder | Purpose | Examples |
|--------|---------|----------|
| `architecture/` | Proposed design sketches and execution model | Solver loop, persistence, execution target, boundary ports |
| `concepts/` | Foundational principles, philosophy, mental models | Invariants, separation of concerns, intent-driven UI |
| `comparison/` | Positioning against other tools, frameworks, paradigms | AI code gen, MDA, Terraform, ACE |
| `grammar/` | Candidate language patterns and boundaries | Keyword candidates, value constraints, state responsibility |
| `examples/` | Future examples and evaluation sketches | Sample specs, thought experiments |
| `reference/` | Prior art and source material | Papers, links, notes |

## Writing Conventions

### Tone and Voice

- **Clear but exploratory** — write for CEOs, lawyers, and engineers equally, without pretending the language or execution target already exists
- **Declarative when describing the thesis; explicit when describing uncertainty** — use target-model language such as "would," "should," and "design goal" where implementation is not settled
- **No false certainty** — strong claims are allowed, but avoid presenting unfinished grammar, solver, compiler, or execution target behavior as implemented fact
- **Different documents may have different heat** — `comparison/why-sdd-fails.md` is intentionally polemical; README, manifest, concepts, grammar, and architecture should be more careful

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
| spec | A Spex specification document | source code |
| signed business contract | The human-reviewed spec when legal/business accountability is emphasized | source code |
| solver | The mathematical verifier that checks completeness and contradictions | implementation engine |
| compiler | The component that lowers an accepted spec into portable, distributable business-engine parts | LLM, code generator |
| execution target | The portable, distributable business engine target | backend, server, infrastructure layer |
| boundary port | A declared input/output/state/authority boundary where infrastructure connects | API endpoint, route, socket |
| outside authority | A named external source of truth defined in the spec glossary | API, service, integration |
| actor | A role that performs actions | user, client, principal |
| entity | A business object defined in the spec | model, class, table, resource |
| action | A permitted operation (e.g., Cancel, Ship) | method, function, endpoint, API call |
| state | A declared business condition or responsibility of an entity | table row, cache entry, primitive value |

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

- "How might X work in the proposed design?" → `architecture/`
- "Why do we believe X?" → `concepts/`
- "How is X different from Y?" → `comparison/`
- "How might X be expressed in Spex?" → `grammar/`

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
- The `manifest.md` captures the current working thesis — if a new document changes the thesis, update the manifest too

## Session Memory Guidelines

When working in this repo across multiple conversations:

- Save **structural decisions** (new folders, naming changes) to `/memories/repo/`
- Save **in-progress document outlines** to `/memories/session/`
- Don't save content that's already in the markdown files — the repo _is_ the memory
