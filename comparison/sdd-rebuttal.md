# Rebuttal: How SDD Tries To Defend Itself

This document pairs the strongest possible defenses of Spec-Driven Development with direct Spex responses.
An experiment done with QWEN3.6 35B.

---

## 1. "SDD Is Pragmatic. Spex Is Theoretical."

**SDD Defense:**

SDD does not claim mathematical perfection. It claims to be better than vibe coding for teams that need to ship software now. It is an engineering practice, not a theoretical manifesto. The point is simple: stop prompting AI with vague intent, write specs first, and use those specs to guide implementation.

The practical challenge to Spex is obvious: SDD is usable today. Spex has no widely adopted parser, solver, runtime, or production ecosystem. The fair objection is: show a Spex specification that actually runs.

**Spex Response:**

This is a fair challenge, but it attacks implementation maturity, not conceptual validity.

Spex does not depend on inventing new mathematics. It combines existing foundations: typed functional evaluation for pure algorithms, contract logic for preconditions and results, event/state transition systems for business behavior, and workflow logic for executable events. The missing piece is not the mathematics. The missing piece is the integrated language and runtime.

SDD improves the prompt-to-code pipeline. Spex removes the pipeline as the central artifact. The question is not whether Spex can be engineered. The question is whether the industry wants to keep maintaining probabilistically generated application code, or move toward deterministic executable contracts.

---

## 2. "Disposable Code Does Not Mean Unreviewed Code."

**SDD Defense:**

When SDD says code is disposable, it does not mean code should be shipped without review. It means teams should stop emotionally attaching themselves to generated code. Review, sandboxing, tests, and guardrails are still necessary. They are standard engineering practice.

**Spex Response:**

That is exactly the contradiction.

If generated code still needs review, tests, sandboxing, security checks, and human inspection, then it is not truly disposable. It is still a liability. The fact that the code was generated does not remove the cost of understanding it. It may increase that cost because reviewers must now inspect code they did not design and may not trust.

Spex changes the unit of liability. The reviewed artifact is not generated application code. The reviewed artifact is the specification contract. The solver, compiler, and runtime are infrastructure components that can be audited as platform assets. Application behavior changes by changing the signed specification, not by regenerating and re-reviewing disposable code.

Spex does not eliminate all engineering. It eliminates generated application code as the thing teams must continuously inspect for correctness.

---

## 3. "Token Optimization Is Responsible Engineering."

**SDD Defense:**

Token optimization is not a confession of failure. It reduces cost, latency, and energy consumption. If YAML works better than JSON for a given model, using YAML is responsible engineering.

**Spex Response:**

Token optimization is reasonable inside an LLM-driven implementation loop. But it does not solve the structural problem.

SDD still performs a repeated cycle:

```
spec → prompt → generated code → generated tests → review → correction → regeneration
```

Every cycle consumes tokens. Every generated artifact must be checked against the original intent. Optimizing the format of prompts may reduce waste, but it does not remove the loop.

Spex uses the LLM differently. The LLM is an authoring assistant, not the implementation engine. It translates imprecise human intent into controlled specification text. The compiler either accepts the text or produces a counterexample. Once accepted, the specification is executed by the runtime.

The difference is not "fewer tokens per prompt." The difference is bounded translation versus continuous regeneration.

---

## 4. "Instruction Fragmentation Is Separation of Concerns."

**SDD Defense:**

Having instructions in chat, specs, skills, system prompts, and project files is not fragmentation. It is separation of concerns. Different levels of instruction have different lifetimes and scopes.

**Spex Response:**

Separation of concerns is good. Fragmentation of authority is not.

In SDD, important behavior may be distributed across chat history, agent instructions, project conventions, generated plans, skills, tests, and code. That means the true source of behavior is not one artifact. It is an emergent result of many artifacts interpreted by a probabilistic model.

Spex separates concerns differently. Business logic, algorithms, UI state, rendering, and infrastructure may live in separate files, but they compile into one formal model. The specification system defines the dependency boundaries and the meaning of each file.

Physical concerns such as programming language versions, framework choices, database products, and deployment mechanisms belong to infrastructure policy or runtime configuration. They are not part of the business contract.

The goal is not one giant document. The goal is one authoritative formal model.

---

## 5. "Hallucination Is Known and Contained."

**SDD Defense:**

Hallucination is a known property of LLMs. SDD does not deny it. It contains the risk through review, tests, sandboxing, and guardrails.

**Spex Response:**

Containment is not the same as correctness.

The central weakness of AI-generated code is that the model is allowed to guess. A hallucination does not merely produce a wrong sentence. It can produce a coherent implementation that looks plausible, passes shallow tests, and still violates the intended business rule.

SDD adds safety layers around a probabilistic foundation. Spex removes probabilistic implementation from the correctness path. The LLM may propose specification text, but it does not decide what the text means. The compiler does.

A Spex clause is accepted only if it maps to the formal model. Preconditions, transformations, transactions, workflow rules, and results are checked deterministically. If the model is incomplete or contradictory, the solver produces a counterexample. The LLM can explain that counterexample to the human, but it cannot overrule it.

This is the core distinction:

- **SDD asks the model to implement.**
- **Spex asks the model to help author.**
- **The compiler decides.**

---

## 6. "The LLM Is a Compiler — Structurally."

**SDD Defense:**

An LLM is a compiler in the structural sense: it translates a source you own (the spec) into a derived artifact (the app) that you don't hand-maintain. The source is canonical, the artifact is derived, and you maintain the source rather than the artifact. That relationship is what matters.

**Spex Response:**

People who claim an LLM is a compiler have never tried to design a programming language with an unfinished spec.

Designing a programming language revealed the hard truth: when the spec is incomplete, changing it breaks the implementation. The LLM cannot reliably update only the affected parts. It produces uncontrolled transformations that require starting from scratch.

That is not a compiler. That is an uncontrolled code transformer.

A real compiler has a pipeline: lexer, parser, type checker, optimizer, code generator. Each stage is deterministic. Each stage can be debugged. Each stage preserves the meaning of the input. The compiler community spent decades solving this problem.

The LLM-as-compiler people are proposing something worse than what already exists, and calling it equivalent.

Spex doesn't use the LLM as a compiler. It uses the LLM as a translator — from free language to controlled English. The actual compilation is done by the solver, which is a real compiler: deterministic, debuggable, and mathematically verifiable.

---

## 7. "Formal Specification Languages Failed Because They Were Too Hard."

The review bottleneck exists because SDD continues to produce application code as the central artifact. If AI writes code faster than humans can understand it, delivery speed becomes an illusion. The organization has not removed work. It has moved work from writing to reviewing, debugging, and trusting.

Spex eliminates that specific bottleneck. There is no generated application source code to review after every change. The human reviews the specification. The solver verifies the formal model. The runtime evaluates the accepted contract.

This does not remove all review. It moves review to the correct level: business meaning, not generated implementation detail.

---

## 7. "Formal Specification Languages Failed Because They Were Too Hard."

**SDD Defense:**

Formal specification languages such as TLA+, Alloy, Z, and Event-B never became mainstream because they were too hard for normal engineers and business stakeholders. SDD works because it fits existing engineering practice.

**Spex Response:**

This is the strongest SDD defense, and Spex accepts the diagnosis.

Formal methods did not fail because the mathematics was useless. They failed because the authoring experience was too far away from how humans express business rules.

Spex changes the authoring interface. Humans describe intent in ordinary language. The LLM translates that intent into controlled English. The compiler checks the controlled English against the formal model. If the model is incomplete, ambiguous, or contradictory, the solver returns a counterexample. The LLM turns the counterexample into a human question.

The human does not need to write TLA+, Alloy, or lambda calculus. The human signs a controlled English contract whose formal meaning is compiler-checked.

The barrier was never only verification. The barrier was specification authoring. Spex makes the formal model writable.

---

## Summary

SDD is a necessary transition away from vibe coding, but it remains trapped in the code-generation paradigm. It improves the way teams instruct AI to produce code, but it does not eliminate the liability of generated code.

Spex changes the target artifact.

| | SDD | Spex |
|---|---|---|
| **Role of spec** | Guides code | Is the executable contract |
| **Role of AI** | Generates implementation | Helps author formal intent |
| **Correctness** | Contained by review, tests, guardrails | Enforced by deterministic compilation, verification, runtime evaluation |

SDD is better prompting for software production.

Spex is a different definition of software.
