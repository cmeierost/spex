---
name: grill-spex
description: Stress-test Spex concepts, architecture, and grammar against internal contradictions, blind spots, and paradigm violations. Use when the user wants to challenge an idea, find gaps in the docs, or says "grill this".
---

# Grill Spex

Relentlessly challenge Spex documentation against the manifest, the three invariants, and internal consistency.

## Rules

1. **Read the manifest first** — `manifest.md` is the single source of truth. Every claim in every doc must trace back to it.
2. **Check the three invariants** — does the doc violate any of:
   - Absolute separation of logic and physics?
   - Contract is the code (single source of truth)?
   - Purely declarative intent governance?
3. **Check terminology** — does the doc use banned terms from AGENTS.md (e.g., "Holy Grail", "compiler", "backend")?
4. **Check cross-references** — do links work? Does the doc link back to at least one existing doc?
5. **Check for hedging** — "might", "could", "perhaps" are forbidden in Spex docs.

## Grill Questions

For each document or idea, ask:

- **Contradiction:** Does this claim conflict with anything in the manifest or another doc?
- **Blind spot:** What scenario does this doc not cover that it should?
- **Physics leak:** Does this doc accidentally reference physical concepts (databases, APIs, servers) in the logical world?
- **Ambiguity:** Is there a sentence with more than one interpretation?
- **Terminology drift:** Are terms used inconsistently with AGENTS.md?
- **Completeness:** If this is a grammar doc, are all keyword patterns accounted for? If architecture, are all components covered?

## Output Format

```
## Contradictions Found
- [claim] conflicts with [source]: [explanation]

## Blind Spots
- [missing scenario or case]

## Terminology Issues
- "[wrong term]" → should be "[correct term]"

## Questions to Resolve
1. [clarifying question]
```

Be direct. Don't soften findings. The goal is to make the docs unbreakable.
