# Related Work

This document catalogs technologies, languages, and research that influenced, relate to, or contrast with Spex.

## Mathematical Foundations

### Lambda Calculus

- **Source:** [Wikipedia](https://en.wikipedia.org/wiki/Lambda_calculus)
- **Relevance:** The mathematical core of Spex. Every Spex clause maps 1:1 to typed lambda calculus expressions. The Church-Rosser theorem guarantees identical evaluation on any machine — no race conditions, no non-determinism. Lambda calculus provides the stateless, side-effect-free evaluation model that makes "spec is code" possible.
- **Spex's difference:** Lambda calculus alone is a theoretical formalism. Spex wraps it in controlled English so non-programmers can author and sign specifications directly.

### Datalog

- **Source:** [Wikipedia](https://en.wikipedia.org/wiki/Datalog)
- **Relevance:** A declarative logic programming language based on first-order logic. Like Spex, Datalog is purely declarative — you state *what* is true, not *how* to compute it. Its fixed-point semantics are useful for reasoning about reachability and completeness.
- **Spex's difference:** Datalog targets database query reasoning. Spex targets business rules, actor permissions, and state transitions — and presents them in legal English, not logic notation.

---

## Specification Languages & Tools

### Alloy

- **Source:** [alloytools.org](https://alloytools.org)
- **Relevance:** A structural specification language and model checker. Alloy finds counterexamples by exhaustive exploration of state space — similar to what Spex's solver does when it reports "what happens if X occurs while Y is blocked?"
- **Spex's difference:** Alloy uses a custom DSL with sigs, facts, and predicates. Spex uses controlled English. Alloy targets systems researchers; Spex targets business stakeholders and lawyers.

### TLA+

- **Source:** [azurewebsites.net](https://tlaplus.github.io)
- **Relevance:** Leslie Lamport's formal specification language for distributed systems. Lamport has spent decades arguing that programming is just a low-level implementation detail and that the *specification* is where the actual engineering happens. TLA+ excels at proving temporal properties (liveness, safety) and finding subtle concurrency bugs through model checking.
- **Spex's alignment:** Lamport's thesis is Spex's thesis: the spec is the engineering artifact; the implementation is disposable. Spex extends this from systems architecture to business logic, and from mathematical notation to controlled English.
- **Spex's difference:** TLA+ is aimed at systems architects who write temporal logic. Spex is aimed at business stakeholders who read declarative English. TLA+ specifies *how systems behave over time*; Spex specifies *what actors are permitted to do*.

### Fizzbee

- **Source:** [github.com](https://github.com)
- **Relevance:** A tool that translates natural-language requirements into formal models for verification. Fizzbee attempts to bridge the gap between ambiguous requirements and machine-checkable specs — the same gap Spex closes.
- **Spex's difference:** Fizzbee translates requirements *into code that is then verified*. Spex eliminates the translation step entirely — the spec *is* the executable artifact.

### GitHub Spec Kit

- **Source:** [Microsoft Dev Blog](https://developer.microsoft.com/blog/spec-driven-development-spec-kit), [github.com/github/spec-kit](https://github.com/github/spec-kit)
- **Relevance:** A toolkit from GitHub that promotes Spec-Driven Development (SDD). It provides a CLI (`specify`), templates, and slash commands (`/specify`, `/plan`, `/tasks`) to help teams write specs before handing them to AI coding agents. Key concepts: a `constitution.md` for non-negotiable project principles, and a sequential workflow of spec → technical plan → task breakdown → AI implementation. Spec Kit acknowledges that "code is not the best medium for requirements negotiation" and that specs should be "living documents that evolve alongside your code."
- **Spex's difference:** Spec Kit still uses AI agents to *generate code from specs*. The spec is a steering document for code generation — not the final product. Spex eliminates code generation entirely: the spec is directly evaluated by the solver and runtime. Spec Kit's specs are markdown templates for developers; Spex's specs are controlled English for business stakeholders and lawyers. Spec Kit requires a constitution, templates, and helper scripts; Spex requires one grammar and one manifest.

### Accord Project (Cicero)

- **Source:** [accordproject.org](https://accordproject.org)
- **Relevance:** Cicero is a toolkit for creating, testing, and running computable legal agreements. It uses templates with logical conditions — conceptually close to Spex's permission rules ("may X if and only if Y").
- **Spex's difference:** Cicero templates are JSON-based and require programming to author. Spex specs are pure controlled English, directly evaluable by the solver without a code generation step.

### Gherkin / Cucumber

- **Source:** [cucumber.io/docs/gherkin/reference](https://cucumber.io/docs/gherkin/reference)
- **Relevance:** Gherkin is a natural-language specification format using `Given/When/Then` steps to describe behavior. It is the closest existing technology to Spex's goal of human-readable specs: business stakeholders write scenarios in plain language, and they execute as tests. The `Rule` keyword groups scenarios by business rule, and the philosophy of "imagine it's 1922" (no technology assumptions) aligns with Spex's logical/physical separation.
- **Spex's difference:** Gherkin scenarios are test harnesses, not executable specifications. Each `Given/When/Then` step requires a code step definition that maps the text to an implementation — so Gherkin still depends on code. Spex eliminates step definitions entirely: the spec is evaluated directly by the solver. Gherkin verifies that code behaves correctly; Spex eliminates the code that needs verifying.

---

## Controlled English & Natural Language

### Attempto Controlled English (ACE)

- **Source:** [uzh.ch](https://www.ace-editor.org)
- **Relevance:** The direct conceptual ancestor of Spex's grammar. ACE is a strict subset of English designed to be unambiguous and directly translatable to first-order logic. Spex evolves ACE's approach: controlled grammar → mathematical evaluation.
- **Spex's difference:** ACE targets knowledge representation and NLP research. Spex targets executable business specifications with a complete runtime, solver, and UI generation pipeline.

---

## Programming Languages

### Unison

- **Source:** [unison-lang.org](https://www.unison-lang.org)
- **Relevance:** A purely functional language built on lambda calculus with content-addressed code storage. The creators started Unison because they believe the way we store and distribute code across computers is archaic and fundamentally wrong. Unison's core is a lambda-calculus-based runtime — structurally similar to Spex's mathematical evaluation model. Its "code is a set, not a tree" philosophy aligns with Spex's "spec is the only artifact" principle.
- **Spex's alignment:** Unison attacks the same problem from the programming language side: the file-tree model of code distribution is broken. Spex attacks it from the specification side: the code model itself is broken.
- **Spex's difference:** Unison is still a programming language for developers. Spex eliminates programming entirely — the spec is authored in English and evaluated directly.

### Haskell

- **Source:** [haskell.org](https://www.haskell.org)
- **Relevance:** A pure functional language whose type system and referential transparency mirror the properties Spex requires: no side effects, deterministic evaluation, composability. Haskell demonstrates that pure functional design scales to production systems.
- **Spex's difference:** Haskell requires expert programmers. Spex requires only that the human can read and sign a specification in controlled English.

### Eiffel (Design by Contract)

- **Source:** [eiffel.org](https://www.eiffel.org)
- **Relevance:** Eiffel's Design by Contract (DbC) embeds preconditions, postconditions, and invariants directly into code. The concept of "the contract defines correctness" is a precursor to Spex's "the spec is the code" principle.
- **Spex's difference:** DbC attaches contracts *to code that still exists*. Spex eliminates code — the contract *is* the entire system. DbC is a programming technique; Spex is a paradigm that replaces programming.

---

## Distributed Systems & Runtime

### Orleans

- **Source:** [Microsoft Learn](https://learn.microsoft.com/dotnet/orleans/overview)
- **Relevance:** Orleans is Microsoft's virtual actor framework for distributed systems. Its core innovation — the *virtual actor model* — is conceptually adjacent to Spex's logical/physical separation: actors exist logically forever (always addressable by stable identity), while the runtime decides where they run physically (placement, activation, deactivation, failover). Orleans silos manage the physical world (servers, clusters, persistence) invisibly to the grain code. This mirrors Spex's invariant that the logical world is immortal and the physical world is a disposable transit vehicle.
- **Spex's difference:** Orleans is still a programming framework — you write C# grain interfaces, implement state management, and reason about distributed transactions. Orleans hides *where* actors run; Spex hides *that* actors run at all. Orleans targets .NET developers building scalable backends; Spex targets business stakeholders authoring executable specs in controlled English.

---

## Business Process Modeling

### DCR Graphs

- **Source:** [dcrgraphs.net](https://dcrgraphs.net)
- **Relevance:** Declarative Constraint-based Rule graphs model business processes as sets of constraints (response, precedence, exclusion, succession) rather than imperative flowcharts. This declarative approach to "what is allowed" aligns with Spex's intent-driven governance.
- **Spex's difference:** DCR Graphs are visual diagrams — hard to version, diff, and review. Spex specs are text-based, diffable, and legally signable.

---

## AI & Code Generation (What Spex Replaces)

### Kimi AI / Moonshot AI

- **Source:** [kimi.ai](https://kimi.ai), [github.com](https://github.com)
- **Relevance:** Kimi and similar AI coding agents represent the state of the art in probabilistic code generation. They write, review, and debug source code through trial-and-error — the paradigm Spex explicitly rejects.
- **Spex's difference:** Kimi copies the craft of manual programming with AI. Spex eliminates programming entirely. Kimi generates unmaintainable AI legacy; Spex produces a single, human-readable, mathematically verified spec.

### LLMbda Calculus

- **Source:** [arxiv.org](https://arxiv.org)
- **Relevance:** Research into how LLMs can reason about lambda calculus expressions. Explores whether probabilistic models can be guided toward deterministic mathematical reasoning — adjacent to Spex's use of LLMs as translators between free language and formal spec.
- **Spex's difference:** LLMbda researches LLM capability. Spex uses the LLM as a narrow, bounded translator in a closed loop: human free text → LLM → controlled English → solver → counterexample → LLM → clarifying question → human.

### LLMs in Requirements Engineering (Hymel & Johnson)

- **Source:** [arxiv.org/2501.19297](https://arxiv.org/abs/2501.19297)
- **Relevance:** Empirical study comparing LLMs vs human experts in requirements elicitation. LLM-generated requirements scored higher on alignment (+1.12) and completeness (+10.2%), at 720x the speed and 0.06% the cost of a human expert. Validates that LLMs are effective at structuring and refining requirements — the exact role they play in Spex's design loop.
- **Spex's alignment:** Confirms the LLM's strength is in the authoring phase: translating imprecise human intent into structured, complete requirements. This is the LLM's job in Spex — help the human write the spec.
- **Spex's difference:** The paper measures requirements quality, not correctness. Better requirements are still requirements — they need to be verified, implemented, and tested. Spex adds the solver: the mathematical compiler that checks the requirements for completeness and contradiction. The LLM writes; the solver verifies.

This paper confirms a critical insight: an LLM alone is insufficient for writing specifications. The LLM produces well-aligned, seemingly complete output — but it cannot tell you what is missing. It writes, but it doesn't know what it forgot.

You need math to prove completeness. The solver is what drives the LLM to continue:

```
Human: "I want customers to cancel orders"
LLM: writes the cancel rule
Solver: "Counterexample — what if Customer A cancels Customer B's order?"
LLM: adds the ownership check
Solver: "Counterexample — what if the order is already shipped?"
LLM: adds the shipped check
Solver: ✓ complete
```

Without the solver, the LLM stops when it feels done — not when it actually is. The solver is the fuel that keeps the LLM iterating until the spec is provably complete.

But the solver alone is not enough either. The solver finds gaps; it does not fill them. When the solver returns a counterexample, there may be multiple valid ways to resolve it. The human decides which solution is correct.

The human is the main thing in the specification. The LLM proposes. The solver verifies. The human decides.

If there is no human in the loop, how can an LLM know what to program? It cannot. It guesses. And that is the difference between a specification and a hallucination.

---

## Real-World Case Studies

### MindStudio / Remy (Compiler Comparison)

- **Source:** [MindStudio Blog](https://www.mindstudio.ai/blog/is-llm-a-compiler-analysis)
- **Relevance:** MindStudio argues that an LLM is a compiler "in the structural sense": it translates a source you own (the spec) into a derived artifact (the app) that you don't hand-maintain. They concede LLMs are non-deterministic at the token level, but claim reproducibility is a workflow property — pin the spec, annotations, and schemas, and the output is "behaviorally equivalent." Key claim: "the spec is the program; the code is what gets compiled."
- **Spex's fundamental disagreement:** MindStudio claims the LLM is a compiler structurally, because spec is source and code is derived. This is wrong for a deeper reason: you can never prove the derived code is 100% correct. Behavioral equivalence is not correctness. If the spec is the single source of truth, the only thing you need to correct is the spec — not some probabilistic artifact whose correctness you cannot verify.

This was learned from practice, not theory. Designing a programming language with an unfinished spec revealed the hard truth: when the spec is incomplete, changing it breaks the implementation. The LLM cannot reliably update only the affected parts. It produces uncontrolled transformations that require starting from scratch. That is not a compiler — that is an uncontrolled code transformer.

And the second point is worse: we know how to build real compilers. The compiler pipeline (lexer, parser, type checker, optimizer, code generator) is a solved problem. The LLM-as-compiler people are proposing something worse than what already exists, and calling it equivalent.

### Siemens Knowledge Fabric

- **Source:** [Google Cloud Blog](https://cloud.google.com/blog/products/ai-machine-learning/how-siemens-sliced-the-elephant-modernizing-legacy-code-with-agentic-workflows/)
- **Relevance:** Siemens, with hundreds of millions of lines of industrial code, built "Knowledge Fabric" — an agentic AI system using knowledge graphs, Google ADK, and Gemini to modernize legacy software. The case study explicitly states: *"hallucinated or unvalidated changes are operationally unacceptable."* To make AI code generation work, Siemens needed: knowledge graphs to map code structure, human-in-the-loop at every step, and a "slicing the elephant" pattern to break tasks into tiny agent-led chunks. This is empirical evidence that agentic code generation, even at the highest level, cannot produce verifiable output without massive scaffolding and human oversight.
- **Spex's difference:** Siemens spent enormous effort building scaffolding to make probabilistic AI produce deterministic results. Spex starts from determinism: the spec is mathematically evaluated, not probabilistically generated. Siemens' "slicing the elephant" is a workaround for AI's inability to reason holistically; Spex's solver reasons over the entire spec atomically.

---

## Summary Map

| Technology | Domain | Relationship to Spex |
|---|---|---|
| Lambda Calculus | Mathematics | Core evaluation foundation |
| Datalog | Logic programming | Declarative reasoning, reachability |
| Alloy | Model checking | Counterexample generation |
| TLA+ | Formal specs | Temporal properties, safety proofs |
| Fizzbee | Requirements → formal | Bridge between natural language and verification |
| Spec Kit | Spec tooling | Specification-as-artifact movement |
| Accord Project / Cicero | Legal agreements | Computable contracts, template conditions |
| ACE | Controlled English | Direct grammatical ancestor |
| Unison | Functional language | Lambda-calculus runtime, content-addressed code |
| Haskell | Functional language | Pure evaluation, referential transparency |
| Eiffel / DbC | Contract programming | Contracts as correctness criteria |
| DCR Graphs | Business processes | Declarative constraints over imperative flow |
| Orleans | Distributed runtime | Virtual actor model, logical/physical separation |
| Gherkin / Cucumber | BDD testing | Natural-language specs, but still requires code step definitions |
| Siemens Knowledge Fabric | Industrial AI | Empirical evidence: agentic code gen needs massive scaffolding |
| Kimi / Moonshot | AI code generation | Paradigm Spex replaces |
| LLMbda Calculus | LLM + math | LLM reasoning about formal expressions |
