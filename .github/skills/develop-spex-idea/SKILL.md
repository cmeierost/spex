---
name: develop-spex-idea
description: Expand a Spex concept into a full documentation proposal. Use when the user has a raw idea, wants to flesh out a new topic, or says "develop this idea".
---

# Develop Spex Idea

Take a raw idea and expand it into a structured documentation proposal ready for review.

## Process

### 1. Classify the Idea

Determine which folder the idea belongs to:

| Question                           | Folder          |
| ---------------------------------- | --------------- |
| How does X work inside the system? | `architecture/` |
| Why do we believe X?               | `concepts/`     |
| How is X different from Y?         | `comparison/`   |
| How do I write X in Spex?          | `grammar/`      |

If none fit, propose a new folder (only if 3+ docs will go there).

### 2. Check for Overlap

Search existing docs for:

- Claims that already cover this idea (even partially)
- Terminology that should be reused
- Cross-references that should be established

### 3. Generate Outline

Produce a structured outline:

```markdown
# [Title]

## What This Document Answers

[One-sentence summary]

## Sections

1. [Section] — [what it covers]
2. [Section] — [what it covers]
   ...

## Cross-References

- Links from: [existing doc]
- Links to: [existing doc]

## Key Claims

- [Claim 1 — must trace to manifest]
- [Claim 2]
```

### 4. Flag Dependencies

- Does this idea require a concept that isn't documented yet?
- Does it contradict anything in the manifest?
- Does it introduce new terminology that needs to go in AGENTS.md?

### 5. Propose Next Step

End with: "Ready to write this as `.md` file, or refine the outline further?"

## Constraints

- Follow AGENTS.md conventions (tone, formatting, terminology)
- Every claim must be traceable to `manifest.md`
- No hedging language
- Suggest concrete Spex spec examples where relevant
