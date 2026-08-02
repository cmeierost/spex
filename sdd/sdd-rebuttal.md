# Rebuttal: How SDD Tries To Defend Itself

This document pairs strong defenses of Spec-Driven Development with SPEX responses.

It originated as an exploratory dialogue with Qwen 3.6 35B challenging the LLM to criticize the idea of SPEX from the view of SDD and was improved with GPT 5.6 Sol. It has been edited into a research note.

---

## 1. "SDD Is Pragmatic. SPEX Is Theoretical."

**SDD Defense:**

SDD does not claim mathematical perfection. It claims to be better than vibe coding for teams that need to ship software now. It is an engineering practice, not a theoretical manifesto. The point is simple: stop prompting AI with vague intent, write specs first, and use those specs to guide implementation.

The practical challenge to SPEX is obvious: SDD is usable today. SPEX has no widely adopted parser, solver, runtime, or production ecosystem. The fair objection is: show a SPEX specification that actually runs.

**SPEX Response:**

This is a fair challenge, but it attacks implementation maturity, not conceptual validity.

SPEX does not depend on inventing new mathematics. It explores how existing foundations could be combined: typed functional evaluation for pure algorithms, contract logic for preconditions and results, event/state transition systems for business behavior, and workflow logic for executable events. The missing piece is not the mathematics alone. The missing piece is an integrated language, solver, authoring loop, and runtime.

SDD improves the prompt-to-code pipeline. SPEX asks whether that pipeline should remain the central artifact. The question is whether the industry wants to keep maintaining probabilistically generated application code, or move toward deterministic executable contracts.

---

## 2. "Disposable Code Does Not Mean Unreviewed Code."

**SDD Defense:**

When SDD says code is disposable, it does not mean code should be shipped without review. It means teams should stop emotionally attaching themselves to generated code. Review, sandboxing, tests, and guardrails are still necessary. They are standard engineering practice.

**SPEX Response:**

That is exactly the contradiction.

If generated code still needs review, tests, sandboxing, security checks, and human inspection, then it is not truly disposable. It is still a liability. The fact that the code was generated does not remove the cost of understanding it. It may increase that cost because reviewers must now inspect code they did not design and may not trust.

SPEX would change the unit of liability. The reviewed artifact would not be generated application code. It would be the specification contract. The solver, compiler, and execution target would be platform assets audited separately from application behavior. Application behavior would change by changing the signed specification, not by regenerating and re-reviewing disposable code.

SPEX does not eliminate all engineering. It tries to eliminate generated application code as the thing teams must continuously inspect for business correctness.

---

## 3. "Token Optimization Is Responsible Engineering."

**SDD Defense:**

Token optimization is not a confession of failure. It reduces cost, latency, and energy consumption. If YAML works better than JSON for a given model, using YAML is responsible engineering.

**SPEX Response:**

Token optimization is reasonable inside an LLM-driven implementation loop. But it does not solve the structural problem.

SDD still performs a repeated cycle:

```
spec → prompt → generated code → generated tests → review → correction → regeneration
```

Every cycle consumes tokens. Every generated artifact must be checked against the original intent. Optimizing the format of prompts may reduce waste, but it does not remove the loop.

SPEX uses the LLM differently. The LLM is an authoring assistant, not the implementation engine. It translates imprecise human intent into controlled specification text. In the target design, the compiler either accepts the text or produces a counterexample. Once accepted, the specification can be lowered into portable, distributable business-engine parts.

The difference is not "fewer tokens per prompt." The difference is bounded translation versus continuous regeneration.

---

## 4. "Instruction Fragmentation Is Separation of Concerns."

**SDD Defense:**

Having instructions in chat, specs, skills, system prompts, and project files is not fragmentation. It is separation of concerns. Different levels of instruction have different lifetimes and scopes.

**SPEX Response:**

Separation of concerns is good. Fragmentation of authority is not.

In SDD, important behavior may be distributed across chat history, agent instructions, project conventions, generated plans, skills, tests, and code. That means the true source of behavior is not one artifact. It is an emergent result of many artifacts interpreted by a probabilistic model.

SPEX separates concerns differently. Business logic, algorithms, UI state, rendering, and infrastructure may live in separate files, but the goal is for them to relate through one authoritative formal model. The specification system would define the dependency boundaries and the meaning of each file.

Physical concerns such as programming language versions, framework choices, database products, and deployment mechanisms belong to infrastructure around the portable, distributable business engine. They are not part of the business contract. They are quantified, measurable goals. And there we can let AI find a way to fulfill them.

The goal is not one giant document. The goal is one authoritative formal model that is proven to not be broken.

---

## 5. "Hallucination Is Known and Contained."

**SDD Defense:**

Hallucination is a known property of LLMs. SDD does not deny it. It contains the risk through review, tests, sandboxing, and guardrails.

**SPEX Response:**

Guessing is intrinsic to how LLMs work; hallucination is what that guessing looks like when it is wrong. It is not a defect that guardrails can eliminate.

Containment is not the same as correctness.

The central weakness of AI-generated code is that the model is allowed to guess. A hallucination does not merely produce a wrong sentence. It can produce a coherent implementation that looks plausible, passes shallow tests, and still violates the intended business rule.

SDD adds safety layers around a probabilistic foundation. SPEX aims to remove probabilistic implementation from the correctness path. The LLM may propose specification text to help the human develop the specification faster, but it must not decide what that text means. The compiler must.

A SPEX clause should be accepted only if it maps to the formal model. Preconditions, transformations, transactions, workflow rules, and results should be checked deterministically. If the model is incomplete or contradictory, the solver should produce a counterexample. The LLM can explain that counterexample to the human, but it should not overrule it.

This is the core distinction:

- **SDD asks the model to implement an unfinished specification.**
- **SPEX asks the model to help author and correct the specification; the compiler then deterministically produces the business core. AI may implement the infrastructure where we know what outcome we need, but do not need to prescribe how it is achieved.**

---

## 6. "The LLM Is a Compiler — Structurally."

**SDD Defense:**

An LLM is a compiler in the structural sense: it translates a source you own (the spec) into a derived artifact (the app) that you don't hand-maintain. The source is canonical, the artifact is derived, and you maintain the source rather than the artifact. That relationship is what matters.

**SPEX Response:**

We already stated:

> If a system translates a human-written specification into code without guessing, it is a compiler by definition, not an LLM. Do not expect deterministic results from a probabilistic system. A compiler requires a restricted, clearly defined language.

Setting up guardrails to make "compiling" deterministic is absurd.

Birgitta Böckeler makes the same distinction in [I still care about the code](https://martinfowler.com/articles/exploring-gen-ai/i-still-care-about-the-code.html) on Martin Fowlers Website: LLMs are inferential systems, not compilers, interpreters, transpilers, or assemblers of natural language. A compiler works from structured input to repeatable, predictable output. An LLM does not. 

A real compiler has a pipeline: lexer, parser, type checker, optimizer, code generator. Each stage is deterministic. Each stage can be debugged. Each stage preserves the meaning of the input. The compiler community spent decades solving this problem.

The LLM-as-compiler people are proposing something worse than what already exists, and calling it equivalent.

SPEX does not use the LLM as a compiler. It uses the LLM as a translator — from free language to restricted controlled English. The actual compilation is done by the solver, which must be deterministic, debuggable, and mathematically verifiable.

---

## 7. "The Review Bottleneck Is Still the Bottleneck."

The review bottleneck exists because SDD continues to produce application code as the central artifact. If AI writes code faster than humans can understand it, delivery speed becomes an illusion. The organization has not removed work. It has moved work from writing to reviewing, debugging, and trusting.

SPEX targets that specific bottleneck. In the intended model, there is no generated application source code to review after every change. The human reviews the specification. The solver verifies the formal model. The compiler produces portable execution parts that bind to infrastructure through declared boundary ports. Examples make the intended business outcome concrete and help demonstrate that the logic satisfies it or may help to find contradictions in the specification.

This does not remove all review. It moves review to the correct level: business meaning, not generated implementation detail.

---

## 8. "Formal Specification Languages Failed Because They Were Too Hard."

**SDD Defense:**

Formal specification languages such as TLA+, Alloy, Z, and Event-B never became mainstream because they were too hard for normal engineers and business stakeholders. SDD works because it fits existing engineering practice.

**SPEX Response:**

This is the strongest SDD defense, and SPEX accepts the diagnosis.

Formal methods did not fail because the mathematics was useless. They failed because the authoring experience was too far away from how humans express business rules.

SPEX changes the authoring interface. Humans describe intent in ordinary language. The LLM translates that intent into controlled English. The compiler would check the controlled English against the formal model. If the model is incomplete, ambiguous, or contradictory, the solver would return a counterexample. The LLM would turn the counterexample into a human question.

The human should not need to write TLA+, Alloy, or lambda calculus. The goal is for the human to sign a controlled English contract whose formal meaning is compiler-checked.

The barrier was never only verification. The barrier was specification authoring. SPEX explores whether LLM assistance can make the formal model writable.

---

## 9. "Business Logic Is Only a Small Part of the System."

**SDD Defense:**

Business logic is often only a small part of a real application. Most effort goes into databases, user interfaces, integrations, authentication, deployment, observability, performance, and infrastructure. SPEX does not make those concerns disappear, so it does not solve most of the work involved in building software.

**SPEX Response:**

Correct. SPEX does not claim to eliminate infrastructure work. It identifies a boundary between two different kinds of requirement.

Functional requirements define the business behavior that must be true: entities, actions, permissions, state transitions, and outcomes. They must be clear enough to verify, because they define the signed business contract. SPEX would make this contract authoritative and deterministically executable.

Non-functional requirements define measurable operational goals: latency, availability, throughput, cost, security policy, supported technologies, and deployment constraints. They matter, but they do not define whether an Order may be cancelled, an Invoice may be paid, or a Shipment may be released.

Those operational concerns may account for most implementation effort. They are still not a reason to make the business contract probabilistic. SPEX keeps the contract small precisely because it is the part that must remain stable, legible, and correct while the surrounding technology changes.

## 10. "Clean Architecture Already Separates Business Logic."

**SDD Defense**:

Separating infrastructure code from business logic is not new. Clean Architecture, Hexagonal Architecture, and similar approaches have pursued that separation for years. We can specify the architecture and let an LLM implement it. They are quite good in it.

**SPEX Response:**

Exactly. SPEX applies the same principle before application code even exists.

Clean and Hexagonal Architecture ask teams to preserve a separation in code. SPEX would represent business behavior in the verified specification and expose infrastructure only through declared boundary ports. The compiler produces portable, distributable business-engine parts; adapters bind them to databases, interfaces, deployment platforms, and outside authorities.

This makes the separation a property of the specification and compilation process rather than a convention maintained by every implementation. Database queries, framework calls, and cloud configuration cannot silently become business rules because they are outside the language that expresses the business contract.

An LLM can implement and optimize adapters against declared ports, permitted technologies, and measurable non-functional goals. It does not define or alter business meaning outside the specification-authoring process. The accepted specification and deterministic compiler remain authoritative.

----

## Summary

SDD is a necessary transition away from vibe coding, but it remains trapped in the code-generation paradigm. It improves the way teams instruct AI to produce code, but it does not eliminate the liability of generated code.

SPEX changes the target artifact.

| | SDD | SPEX |
|---|---|---|
| **Role of spec** | Guides code | Becomes the authoritative business contract |
| **Role of AI** | Generates implementation | Helps author formal intent |
| **Correctness** | Contained by review, tests, guardrails | Targeted through deterministic compilation, verification, and portable, distributable business-engine execution |
| **Source of authority** | Distributed across specs, prompts, code, tests, and agent instructions | Accepted specification and formal model |
| **Implementation** | Probabilistic code generation from an incomplete specification | Deterministic compilation of an accepted specification |
| **Review focus** | AI-generated code, tests, and diffs | Business meaning expressed in the signed specification |
| **Change cycle** | Generate, review, correct, and regenerate | Refine the specification, verify it, then compile |
| **Business and infrastructure** | Separation depends on architecture and implementation discipline | Business core and boundary ports are distinct in the specification model |
| **Infrastructure work** | AI generates application and infrastructure code together | AI may implement adapters against declared ports and measurable goals |
| **Failure of the spec** | The model guesses at omissions or conflicts | The solver should reject incomplete or contradictory formal meaning |

### The Real Difference

- SDD is better prompting for software production. It attempts to make a probabilistic system reliable through increasingly elaborate instructions, guardrails, tests, and review. It accelerates the existing development loop without removing its central problem: because the specification remains ambiguous and LLMs must guess, generated code still has to be understood, reviewed, checked, tested, corrected, and trusted at a volume humans cannot handle at the speed AI can generate it.
Where the specification does not need to be authoritative, Vibe Coding may reach a result faster. SDD is an attempt to repair Vibe Coding without abandoning its probabilistic foundation.

- SPEX asks whether software should instead be defined around the signed specification. The business contract is verified and compiled deterministically; AI assists in authoring the contract but is guarded and driven by the compiler. SPEX separates the parts for which **how** is not important from the business contract. It can leave implementation of infrastructure concerns to artificial intelligence without allowing it to change or break the specification.
