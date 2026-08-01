# State of the Art: Why Others Fail

## Kimi AI / Devin / Agent Swarms

### The Approach

These systems attempt to automate traditional programming by having AI agents write, review, and debug source code. They are fast, impressive demos, and dangerously misleading.

### Why It Fails

| Problem | Impact |
|---------|--------|
| **Probabilistic, not deterministic** | Same prompt → different code. You cannot build safety on a system that produces different output each time |
| **Generates un-auditable AI legacy** | No human reads 50k lines of AI-generated code. You cannot security-audit what you cannot read |
| **Drift compounds at superhuman speed** | Each AI patch introduces subtle regressions that only more AI can "fix" — a feedback loop of accumulating technical debt |
| **Copies a broken paradigm** | Still requires intent → code → tests → deploy. Kimi automates the *writing* of code; it does not solve the *verification* of code |
| **No legal sign-off possible** | Who signs the contract when the code was written by a probabilistic model? No one. That's a compliance dead end |

### The Chain No One States

Kimi generates code → code is probabilistic → probabilistic means non-deterministic → non-deterministic means unverifiable → unverifiable means insecure.

Kimi is a faster hammer on the same broken roof.

### SPEX's Answer

Reduce generated application code as the central artifact. The long-term aim is a deterministic, human-readable, signable specification, with LLMs assisting authoring rather than deciding business logic.

---

## Model-Driven Architecture (MDA) — 2000s

### The Approach

MDA attempted to separate business logic from implementation using visual modeling languages (UML, BPMN) and code generation.

### Why It Fails

| Problem | Impact |
|---------|--------|
| **Visual spaghetti diagrams** | Unreadable at scale, impossible to version control |
| **Roundtrip engineering trap** | Generated code had to be manually edited, breaking sync with the model |
| **Tooling lock-in** | Proprietary model editors, no standard interchange |

### SPEX's Answer

- **Text-based specs** — version control friendly, diffable, reviewable
- **No generated code to edit** — the physical layer is completely disposable
- **Open grammar** — any text editor + LLM can author specs

---

## Modern Cloud Frameworks (Terraform, Pulumi, CDK)

### The Approach

Infrastructure-as-Code tools let developers declare cloud resources in code (HCL, TypeScript, Python).

They are useful because infrastructure is real work. The problem is that they encode implementations of nonfunctional requirements in the same development flow as business behavior.

### Why It Fails

| Problem | Impact |
|---------|--------|
| **Mixes business logic with infrastructure** | "To add a feature, you also need to update the VPC config" |
| **Vendor lock-in** | Terraform modules are AWS/Azure/GCP specific |
| **Still requires programming** | Loops, conditionals, and state management in IaC |

### SPEX's Answer

The business logic does not know computers exist. Infrastructure is a concern around the portable, distributable business engine, not a spec concern.

More precisely: business logic carries functional requirements. Infrastructure code implements nonfunctional requirements such as latency, durability, security, compliance, and scaling. SPEX tries to keep those concerns separate even when both must exist.

---

## Summary Comparison

| Dimension | Traditional Code | AI Code Gen | MDA | Cloud IaC | **SPEX** |
|-----------|-----------------|-------------|-----|-----------|----------|
| Human-readable spec | ❌ | ❌ | ⚠️ (diagrams) | ⚠️ (HCL/TS) | ✅ |
| Single artifact | ❌ (5+ layers) | ❌ (code + prompts) | ❌ (model + code) | ❌ (IaC + app) | ✅ |
| Infrastructure-agnostic | ❌ | ❌ | ✅ | ❌ | ✅ |
| Self-verifying | ❌ (tests) | ❌ (tests) | ❌ | ❌ | ✅ (solver) |
| No code generation | ❌ | ❌ | ❌ | ❌ | ✅ |
