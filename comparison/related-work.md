# Related Work

This document catalogs technologies, languages, and research that influenced, relate to, or contrast with Spex.

## Mathematical Foundations

### Lambda Calculus

- **Source:** [Wikipedia](https://en.wikipedia.org/wiki/Lambda_calculus)
- **Relevance:** A candidate mathematical foundation for Spex. The design goal is for accepted specification clauses to map to typed lambda calculus expressions or an equivalent deterministic formal model. Lambda calculus provides a stateless, side-effect-free evaluation model for business meaning.
- **Spex's difference:** Lambda calculus alone is a theoretical formalism. Spex explores whether a controlled English authoring layer can make that kind of formal model readable and signable by non-programmers.

### Datalog

- **Source:** [Wikipedia](https://en.wikipedia.org/wiki/Datalog)
- **Relevance:** A declarative logic programming language based on first-order logic. Like Spex, Datalog is purely declarative — you state *what* is true, not *how* to compute it. Its fixed-point semantics are useful for reasoning about reachability and completeness.
- **Spex's difference:** Datalog targets database query reasoning. Spex targets business rules, actor permissions, and state transitions — and presents them in legal English, not logic notation.

---

## Specification Languages & Tools

### CASE Tools

- **Source:** [Computer-aided software engineering](https://en.wikipedia.org/wiki/Computer-aided_software_engineering)
- **Relevance:** CASE tools are the older ancestor of many MDA/MDE dreams: graphical analysis models, repositories, generators, integrated workbenches, and round-trip engineering. They show that the desire to raise abstraction above code has been around for decades.
- **Spex's alignment:** Spex shares the ambition to make higher-level artifacts matter more than hand-written implementation code.
- **Spex's difference:** CASE tools often became heavy, methodology-bound environments whose models and generated code had to be kept synchronized. Spex tries to avoid round-trip engineering by making the signed specification the reviewed business artifact and treating compiled output as subordinate.

### Model Driven Architecture (MDA)

- **Source:** [Object Management Group MDA](https://www.omg.org/mda/), [MDA Guide 2.0](https://www.omg.org/cgi-bin/doc?ormsc/14-06-01)
- **Relevance:** MDA is one of Spex's closest ancestors. It explicitly tries to separate business/application logic from platform technology through platform-independent models (PIMs), platform-specific models (PSMs), and transformations between them.
- **Spex's alignment:** The core ambition is shared: business meaning should not be trapped in platform-specific code or infrastructure choices.
- **Spex's difference:** MDA still depends heavily on modeling tools, platform mappings, and generated artifacts. UML models are also often not much easier for business readers than classes: they can expose structure, implementation relationships, and technical "how" rather than business "what." Spex explores whether the signed business specification can remain the authoritative artifact, with the compiler producing portable, distributable business-engine parts rather than platform-specific application code as the reviewed product.

### Model-Driven Engineering (MDE)

- **Source:** [Model-driven engineering overview](https://en.wikipedia.org/wiki/Model-driven_engineering), [InfoQ: 8 Reasons Why Model-Driven Approaches Fail](https://www.infoq.com/articles/8-reasons-why-MDE-fails/)
- **Relevance:** MDE broadens MDA into a larger engineering discipline: domain models, model transformations, CASE-tool history, model quality, tooling, and lifecycle concerns. The InfoQ critique is especially relevant because it points to round-trip problems, model synchronization, insufficient tooling, and the difficulty of keeping generated artifacts aligned with changing models.
- **Spex's alignment:** Spex inherits MDE's desire to make a higher-level artifact drive software behavior and to reduce sensitivity to platform change.
- **Spex's difference:** Spex treats many MDE failure modes as design constraints: avoid making technical models the source of business truth, avoid round-trip engineering, avoid platform-specific generated code as the artifact humans must trust, and keep infrastructure bindings outside the signed business specification.

### Executable UML / xUML

- **Source:** [Executable UML](https://en.wikipedia.org/wiki/Executable_UML)
- **Relevance:** Executable UML is a direct historical attempt to make models executable, testable, and compilable into implementations. It also has concepts close to Spex: domain models, state machines, action languages, model compilation, and platform-independent models.
- **Spex's alignment:** Executable UML shows that the desire for executable, higher-level models is old and serious, not an AI-era novelty.
- **Spex's difference:** Executable UML remains diagram-heavy and typically needs action languages plus model compilers that target known implementation technologies. Its models may be executable, but they still often look like software structure rather than signed business meaning. Spex explores a text-first, controlled-English path where business stakeholders can read and sign the authoritative artifact.

### fUML, ALF, and OCL

- **Source:** [Foundational UML](https://www.omg.org/spec/FUML/), [Action Language for Foundational UML](https://www.omg.org/spec/ALF/), [Object Constraint Language](https://www.omg.org/spec/OCL/)
- **Relevance:** These OMG standards show what happens when UML needs precise execution and constraint semantics: fUML defines executable semantics, ALF supplies an action language, and OCL expresses constraints. They are important because they expose the gap between diagrams and executable meaning.
- **Spex's difference:** Spex does not want business readers to learn UML action languages or formal constraint syntax. The research question is whether LLM-assisted controlled English can provide a more readable authoring surface while still compiling to deterministic semantics.

### Shlaer-Mellor Method

- **Source:** [Shlaer-Mellor method](https://en.wikipedia.org/wiki/Shlaer%E2%80%93Mellor_method)
- **Relevance:** A major predecessor of Executable UML, with domain modeling and action-language traditions aimed at separating problem-domain models from implementation mechanics.
- **Spex's difference:** Shlaer-Mellor is still a modeling method for trained practitioners. Spex asks whether signed business specifications can carry that responsibility in a form readable by non-programmers.

### Eclipse Modeling Framework and domain-specific modeling tools

- **Source:** [Eclipse Modeling Framework](https://www.eclipse.org/modeling/emf/), [MetaEdit+](https://www.metacase.com/)
- **Relevance:** EMF and domain-specific modeling tools show how powerful model-driven tooling can be when a metamodel, editor, generator, and ecosystem line up. They also show why model-driven approaches can become tool-dependent and platform-shaped.
- **Spex's difference:** Spex should learn from their tooling depth without becoming dependent on a heavy, opinionated modeling platform. The spec should remain text-based, reviewable, and independent of the physical infrastructure that eventually runs it.

### Domain-Specific Modeling (DSM)

- **Source:** [Domain-specific modeling](https://en.wikipedia.org/wiki/Domain-specific_modeling), [MetaEdit+](https://www.metacase.com/)
- **Relevance:** DSM raises abstraction by creating modeling languages whose primitives match a domain. This is close to Spex's goal of avoiding general-purpose programming concepts in the business artifact.
- **Spex's difference:** DSM often lives inside specialized modeling tools and still tends toward code generation. Spex explores whether the domain language can remain plain-text, signed, compiler-checked, and independent of tool-specific modeling environments.

### Alloy

- **Source:** [alloytools.org](https://alloytools.org)
- **Relevance:** A structural specification language and model checker. Alloy finds counterexamples by exhaustive exploration of state space — similar to the kind of counterexample-driven authoring loop Spex would need.
- **Spex's difference:** Alloy uses a custom DSL with sigs, facts, and predicates. Spex uses controlled English. Alloy targets systems researchers; Spex targets business stakeholders and lawyers.

### TLA+

- **Source:** [azurewebsites.net](https://tlaplus.github.io)
- **Relevance:** Leslie Lamport's formal specification language for distributed systems. Lamport has spent decades arguing that programming is just a low-level implementation detail and that the *specification* is where the actual engineering happens. TLA+ excels at proving temporal properties (liveness, safety) and finding subtle concurrency bugs through model checking.
- **Spex's alignment:** Lamport's thesis is close to Spex's thesis: the specification is the engineering artifact; implementation details should not be the place where meaning is discovered. Spex explores how far that idea can move from systems architecture into business logic, and from mathematical notation into controlled English.
- **Spex's difference:** TLA+ is aimed at systems architects who write temporal logic. Spex is aimed at business stakeholders who read declarative English. TLA+ specifies *how systems behave over time*; Spex specifies *what actors are permitted to do*.

### Fizzbee

- **Source:** [github.com](https://github.com)
- **Relevance:** A tool that translates natural-language requirements into formal models for verification. Fizzbee attempts to bridge the gap between ambiguous requirements and machine-checkable specs — the same gap Spex investigates.
- **Spex's difference:** Fizzbee translates requirements into a formal model for verification. Spex explores whether controlled English itself can become the signed business artifact whose formal meaning is compiler-checked.

### GitHub Spec Kit

- **Source:** [Microsoft Dev Blog](https://developer.microsoft.com/blog/spec-driven-development-spec-kit), [github.com/github/spec-kit](https://github.com/github/spec-kit)
- **Relevance:** A toolkit from GitHub that promotes Spec-Driven Development (SDD). It provides a CLI (`specify`), templates, and slash commands (`/specify`, `/plan`, `/tasks`) to help teams write specs before handing them to AI coding agents. Key concepts: a `constitution.md` for non-negotiable project principles, and a sequential workflow of spec → technical plan → task breakdown → AI implementation. Spec Kit acknowledges that "code is not the best medium for requirements negotiation" and that specs should be "living documents that evolve alongside your code."
- **Spex's difference:** Spec Kit still uses AI agents to *generate code from specs*. The spec is a steering document for code generation — not the final business artifact. Spex explores whether core business behavior can move out of probabilistic code generation and into a compiler-checked specification. Spec Kit's specs are markdown templates for developers; Spex investigates controlled English for business stakeholders and lawyers.

### Fowler / Böckeler on SDD tools

- **Source:** [Understanding Spec-Driven-Development: Kiro, spec-kit, and Tessl](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html)
- **Relevance:** Birgitta Böckeler's survey is useful because it separates current SDD usage into three levels: spec-first, spec-anchored, and spec-as-source. It also compares Kiro, GitHub Spec Kit, and Tessl as concrete examples of how AI coding tools are currently treating specs as task documents, workflow anchors, or possible source artifacts.
- **Spex's alignment:** Spex shares the strongest version of the ambition: the spec should become the artifact humans maintain and trust over time, not just a prompt or temporary planning document.
- **Spex's difference:** The tools surveyed still rely on LLM-mediated code generation and markdown-heavy review workflows. Spex explores a stricter path: controlled English accepted by a solver/compiler, with compiled business-engine parts subordinate to the signed spec rather than probabilistic code as the trusted output.

### Accord Project (Cicero)

- **Source:** [accordproject.org](https://accordproject.org)
- **Relevance:** Cicero is a toolkit for creating, testing, and running computable legal agreements. It uses templates with logical conditions — conceptually close to Spex's permission rules ("may X if and only if Y").
- **Spex's difference:** Cicero templates are JSON-based and require programming to author. Spex investigates whether controlled English can express business rules directly enough to be verified without making generated application code the central artifact.

### Gherkin / Cucumber

- **Source:** [cucumber.io/docs/gherkin/reference](https://cucumber.io/docs/gherkin/reference)
- **Relevance:** Gherkin is a natural-language specification format using `Given/When/Then` steps to describe behavior. It is the closest existing technology to Spex's goal of human-readable specs: business stakeholders write scenarios in plain language, and they execute as tests. The `Rule` keyword groups scenarios by business rule, and the philosophy of "imagine it's 1922" (no technology assumptions) aligns with Spex's logical/physical separation.
- **Spex's difference:** Gherkin scenarios are test harnesses, not executable specifications. Each `Given/When/Then` step requires a code step definition that maps the text to an implementation — so Gherkin still depends on code. Spex explores whether controlled specification clauses can be compiler-checked directly, without making hand-written step definitions the authority for business meaning.

---

## Controlled English & Natural Language

### Attempto Controlled English (ACE)

- **Source:** [uzh.ch](https://www.ace-editor.org)
- **Relevance:** A direct conceptual ancestor for a possible Spex grammar. ACE is a strict subset of English designed to be unambiguous and directly translatable to first-order logic. Spex builds on the same controlled-language ambition.
- **Spex's difference:** ACE targets knowledge representation and NLP research. Spex explores business specifications, solver-guided authoring, glossary-checked domain concepts, and portable execution targets.

### Logical English

- **Source:** [Logical English](https://logicalcontracts.com/logical-english/), [Logical English meets legal English for swaps and derivatives](https://link.springer.com/article/10.1007/s10506-021-09295-3), [Logical English handbook](https://github.com/LogicalContracts/LogicalEnglish/blob/main/le_handbook.pdf)
- **Relevance:** Logical English is a controlled natural language inspired by logic programming. It can be understood as syntactic sugar for Prolog-like languages and has been applied to legal and regulatory texts, including finance, insurance, citizenship, tax, and ISDA swap/derivatives clauses.
- **Spex's alignment:** Logical English is one of the closest language-level relatives to Spex. It shares the ambition that rule-like English can be both readable by humans and executable or compilable by machines.
- **Spex's difference:** Logical English maps into logic programming systems such as Prolog, ASP, or s(CASP). Spex is exploring a broader business-contract surface: solver-guided authoring, signed specifications, state responsibility, outside authorities, value constraints, boundary ports, and portable/distributable business-engine parts.

---

## Software Engineering Lineage

### Bertrand Meyer / Design by Contract

- **Source:** [Object-Oriented Software Construction](https://bertrandmeyer.com/OOSC2/), [Eiffel Design by Contract](https://www.eiffel.org/doc/solutions/Design_by_Contract_and_Assertions)
- **Relevance:** Meyer made contracts, preconditions, postconditions, and invariants central to software correctness. This is one of the clearest ancestors of Spex's idea that obligations should be explicit and checkable, not merely implied by implementation.
- **Spex's difference:** Design by Contract attaches correctness obligations to code. Spex explores whether those obligations can move up into a signed business specification, with code or compiled parts subordinate to that contract.

### Martin Fowler / DSLs and Analysis Patterns

- **Source:** [Domain-Specific Languages](https://martinfowler.com/books/dsl.html), [Analysis Patterns](https://martinfowler.com/books/ap.html)
- **Relevance:** Fowler's work is important for two sides of Spex: domain-specific language design and explicit modeling of business domains. Analysis patterns show that business meaning has recurring structures; DSLs show that language can be shaped around a domain instead of around general-purpose programming.
- **Spex's difference:** Fowler's DSL work is still mainly for developers. Spex asks whether the domain language can be readable and signable by business stakeholders while still being compiler-checked.

### Robert C. Martin / Clean Architecture

- **Source:** [The Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
- **Relevance:** Clean Architecture states the dependency rule: source-code dependencies should point inward toward policies and business rules, not outward toward frameworks, databases, or UI. This is directly adjacent to Spex's functional/nonfunctional boundary.
- **Spex's difference:** Clean Architecture separates business rules from infrastructure inside code architecture. Spex pushes the boundary further: business meaning should live in the signed specification, while infrastructure binds around a portable, distributable business engine.

### Alistair Cockburn / Hexagonal Architecture

- **Source:** [Hexagonal Architecture](https://alistair.cockburn.us/hexagonal-architecture/)
- **Relevance:** Ports and adapters are the closest architecture ancestor for Spex's **boundary ports**. The core application exposes ports; adapters connect databases, UIs, APIs, and external systems without letting those mechanisms define the core.
- **Spex's difference:** Hexagonal Architecture is an application architecture pattern. Spex treats boundary ports as specification-level concepts whose physical bindings are chosen by infrastructure.

---

## Programming Languages

### Unison

- **Source:** [unison-lang.org](https://www.unison-lang.org)
- **Relevance:** A purely functional language built on lambda calculus with content-addressed code storage. The creators started Unison because they believe the way we store and distribute code across computers is archaic and fundamentally wrong. Unison's content-addressed model is relevant to Spex's interest in portable executable parts.
- **Spex's alignment:** Unison attacks the problem from the programming language side: the file-tree model of code distribution is broken. Spex attacks it from the specification side: business meaning should not be buried in generated application code.
- **Spex's difference:** Unison is still a programming language for developers. Spex explores whether business stakeholders can author the core business artifact in controlled English while compiler-produced parts handle execution.

### Haskell

- **Source:** [haskell.org](https://www.haskell.org)
- **Relevance:** A pure functional language whose type system and referential transparency mirror the properties Spex requires: no side effects, deterministic evaluation, composability. Haskell demonstrates that pure functional design scales to production systems.
- **Spex's difference:** Haskell requires expert programmers. Spex explores whether humans can read and sign controlled-English specifications while algorithmic implementations remain subordinate contract-bound adapters.

### Eiffel (Design by Contract)

- **Source:** [eiffel.org](https://www.eiffel.org)
- **Relevance:** Eiffel's Design by Contract (DbC) embeds preconditions, postconditions, and invariants directly into code. The concept of "the contract defines correctness" is a precursor to Spex's "the spec is the code" principle.
- **Spex's difference:** DbC attaches contracts *to code that still exists*. Spex explores whether the business contract can become the authoritative artifact, with code or compiled parts subordinate to that contract rather than the other way around.

---

## Architecture Boundaries & Portable Execution

### Orleans

- **Source:** [Microsoft Learn](https://learn.microsoft.com/dotnet/orleans/overview)
- **Relevance:** Orleans is Microsoft's virtual actor framework for distributed systems. Its core innovation — the *virtual actor model* — is conceptually adjacent to Spex's logical/physical separation: actors exist logically by stable identity, while the framework decides where they run physically (placement, activation, deactivation, failover). Orleans silos manage the physical world (servers, clusters, persistence) invisibly to the grain code.
- **Spex's difference:** Orleans is still a programming framework — developers write C# grain interfaces, implement state management, and reason about distributed transactions. Orleans hides *where* actors run; Spex explores whether business specifications can avoid talking about physical execution at all, using boundary ports and nonfunctional requirements instead.

### WebAssembly Component Model

- **Source:** [WebAssembly Component Model](https://component-model.bytecodealliance.org/)
- **Relevance:** The component model is a practical reference point for portable, composable execution units with declared imports and exports. It is close to Spex's idea of compiler-produced, distributable business-engine parts with declared inputs and outputs.
- **Spex's difference:** WebAssembly is a technical execution substrate. Spex's split is driven by business semantics: dependencies, transactions, state responsibilities, boundary ports, and nonfunctional requirements determine where distribution is allowed.

### WASI

- **Source:** [WASI](https://wasi.dev/)
- **Relevance:** WASI standardizes how portable modules interact with host capabilities. It is relevant because Spex needs a way to keep the business engine portable while still allowing infrastructure to provide outside capabilities.
- **Spex's difference:** WASI defines technical host interfaces. Spex's boundary ports would be business-facing concepts first, with technical bindings supplied by infrastructure.

---

## Business Process Modeling

### BPMN / BPEL

- **Source:** [BPMN](https://www.omg.org/spec/BPMN/), [WS-BPEL](https://docs.oasis-open.org/wsbpel/2.0/OS/wsbpel-v2.0-OS.html)
- **Relevance:** BPMN tried to create a notation understandable by business and technical stakeholders, while BPEL represented executable process orchestration. Together they show the recurring split between business-readable process models and execution-oriented languages.
- **Spex's alignment:** Spex shares the goal of describing business processes without forcing every detail into application code.
- **Spex's difference:** BPMN and BPEL drift toward workflow, orchestration, messages, services, and execution mechanics. Spex tries to keep infrastructure protocols and orchestration mechanisms outside the signed business specification, using boundary ports and nonfunctional requirements instead.

### DCR Graphs

- **Source:** [dcrgraphs.net](https://dcrgraphs.net)
- **Relevance:** Declarative Constraint-based Rule graphs model business processes as sets of constraints (response, precedence, exclusion, succession) rather than imperative flowcharts. This declarative approach to "what is allowed" aligns with Spex's intent-driven governance.
- **Spex's difference:** DCR Graphs are visual diagrams — hard to version, diff, and review. Spex specs are text-based, diffable, and legally signable.

### Decision Model and Notation (DMN)

- **Source:** [Object Management Group DMN](https://www.omg.org/dmn/)
- **Relevance:** DMN is business-readable decision modeling with execution semantics. It is important prior art because it tries to make business decisions explicit, analyzable, and executable without burying them inside application code.
- **Spex's difference:** DMN is strongest for decisions and tables. Spex aims at a broader signed business contract: permissions, state responsibility, outside authorities, transactions, value constraints, views, and boundary ports.

### Business Rules Engines

- **Source:** [Apache KIE / Drools](https://kie.apache.org/)
- **Relevance:** Business rules engines pull decision logic out of application code and make it separately manageable. They are important prior art for the claim that business rules should not be buried in imperative code.
- **Spex's difference:** Rules engines usually manage fragments of business logic within a larger application architecture. Spex asks whether the signed specification can become the authoritative business artifact for the whole behavior, not only a rules subsystem.

---

## Domain Discovery and Ubiquitous Language

### Domain-Driven Design (DDD)

- **Source:** [Domain-Driven Design resources](https://www.domainlanguage.com/ddd/)
- **Relevance:** DDD places domain language, bounded contexts, aggregates, entities, value objects, and domain events at the center of software design. It is one of the strongest traditions for treating business language as an engineering artifact.
- **Spex's alignment:** Spex inherits the belief that the language of the business domain matters, and that implementation should not distort it.
- **Spex's difference:** DDD still typically ends in code as the authoritative executable artifact. Spex explores whether the domain language itself can become compiler-checked and signable.

### EventStorming

- **Source:** [EventStorming](https://www.eventstorming.com/)
- **Relevance:** EventStorming is a collaborative method for discovering events, commands, policies, boundaries, and competing perspectives in complex business domains. It is highly relevant to Spex's authoring loop because it starts before implementation and focuses on shared business understanding.
- **Spex's difference:** EventStorming is a discovery and modeling workshop format. Spex would need to turn discovered business concepts into controlled, compiler-checked specification text.

---

## Legal and Policy Rules as Code

### Rules as Code / Computable Law

- **Source:** [OECD OPSI: New Techniques for Building and Using Legal Encodings in the Drafting Room](https://oecd-opsi.org/innovations/new-techniques-for-building-and-using-legal-encodings-in-the-drafting-room/), [Encoding legislation](https://link.springer.com/article/10.1007/s10506-023-09350-1)
- **Relevance:** Rules as Code and computable law are very close to Spex's core question. They explore whether legislation, regulation, and policy can be written or maintained in machine-consumable form so that legal effects can be tested, validated, and made operational before or alongside implementation.
- **Spex's alignment:** Spex shares the belief that rules should be checkable before they are operationalized. The important overlap is not merely automation; it is using formalization to expose ambiguity, contradiction, incompleteness, and implementation drift.
- **Spex's difference:** Rules as Code usually starts from public policy or legislation and often produces code or formal encodings beside the legal text. Spex explores whether a controlled specification can become the signed business artifact itself, with LLMs helping authoring and the solver/compiler preserving deterministic meaning.

### Catala

- **Source:** [Catala](https://github.com/CatalaLang/catala), [Catala: A Programming Language for the Law](https://arxiv.org/abs/2103.03198), [Inria: CATALA translates law into code](https://www.inria.fr/en/catala-software-dgfip-cnaf)
- **Relevance:** Catala is a domain-specific language for formalizing statutory law, especially socio-fiscal rules such as tax and benefits. It is explicitly compiler-oriented and aimed at making legal calculations more reliable by translating legal specifications into executable code.
- **Spex's alignment:** Catala is one of the closest technical relatives to Spex: lawyers and computer scientists collaborate around a formal legal encoding, and the compiler becomes a tool for reliability rather than a mere code generator.
- **Spex's difference:** Catala focuses on legal rules that are already algorithmic, especially calculations. Spex is broader and more speculative: signed business specifications, permissions, state responsibility, outside authorities, value constraints, boundary ports, and portable/distributable business-engine parts.

### Blawx

- **Source:** [Blawx Rules as Code Demonstration](https://law.mit.edu/pub/blawxrulesascodedemonstration), [Blawx paper](https://ceur-ws.org/Vol-3193/paper4GDE.pdf)
- **Relevance:** Blawx is a user-friendly Rules as Code tool powered by goal-directed answer set programming. It emphasizes legal reasoning, explanations linked to source material, and usability for legal/policy users rather than only programmers.
- **Spex's alignment:** Blawx is important because it treats legal rule authoring as an interface and explanation problem, not just a backend logic problem. That overlaps strongly with Spex's LLM-assisted authoring loop and solver feedback.
- **Spex's difference:** Blawx encodes statutes into a reasoning system. Spex asks whether controlled specification text can become the reviewed business artifact and compile into portable, distributable business-engine parts.

### LegalRuleML

- **Source:** [LegalRuleML Core Specification](https://docs.oasis-open.org/legalruleml/legalruleml-core-spec/v1.0/os/legalruleml-core-spec-v1.0-os.html)
- **Relevance:** LegalRuleML is serious prior art for representing legal norms, authorities, sources, temporal validity, obligations, permissions, prohibitions, defeasible reasoning, and links between legal text and formal rules.
- **Spex's alignment:** Spex shares the concern that legally or commercially meaningful rules need traceable, checkable structure.
- **Spex's difference:** LegalRuleML is XML-heavy and aimed at legal rule interchange. Spex explores a more readable controlled-English surface for business stakeholders, with formal structure beneath it.

---

## AI & Code Generation (What Spex Replaces)

### Kimi AI / Moonshot AI

- **Source:** [kimi.ai](https://kimi.ai), [github.com](https://github.com)
- **Relevance:** Kimi and similar AI coding agents represent the state of the art in probabilistic code generation. They write, review, and debug source code through trial-and-error — the paradigm Spex explicitly rejects.
- **Spex's difference:** Kimi copies the craft of manual programming with AI. Spex asks whether business logic should move out of probabilistic code generation entirely. Kimi generates code that must be reviewed and trusted; Spex explores a human-readable, compiler-checked specification as the reviewed artifact.

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

- **Source:** [MindStudio Blog](https://www.mindstudio.ai/blog/is-llm-a-compiler-analysis); counterpoint: [I still care about the code](https://martinfowler.com/articles/exploring-gen-ai/i-still-care-about-the-code.html)
- **Relevance:** MindStudio argues that an LLM is a compiler "in the structural sense": it translates a source you own (the spec) into a derived artifact (the app) that you don't hand-maintain. They concede LLMs are non-deterministic at the token level, but claim reproducibility is a workflow property — pin the spec, annotations, and schemas, and the output is "behaviorally equivalent." Key claim: "the spec is the program; the code is what gets compiled."
- **Spex's fundamental disagreement:** MindStudio claims the LLM is a compiler structurally, because spec is source and code is derived. Böckeler's counterpoint is closer to Spex's position: compilers are repeatable and predictable in a way LLM inference is not. Behavioral equivalence is not correctness. If the spec is the single source of truth, the only thing you need to correct is the spec — not some probabilistic artifact whose correctness you cannot verify.

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
| MDA | Model-driven architecture | Platform-independent models and platform-specific mappings |
| MDE | Model-driven engineering | Model transformations, tooling, lifecycle problems |
| Executable UML | Executable modeling | Models as executable/testable artifacts |
| fUML / ALF / OCL | UML semantics | Executable semantics, action language, constraints |
| Shlaer-Mellor | Executable modeling lineage | Domain models and action languages |
| EMF / MetaEdit+ | Modeling tools | Metamodels, generators, tool dependence |
| Alloy | Model checking | Counterexample generation |
| TLA+ | Formal specs | Temporal properties, safety proofs |
| Fizzbee | Requirements → formal | Bridge between natural language and verification |
| Spec Kit | Spec tooling | Specification-as-artifact movement |
| Fowler / Böckeler SDD tools survey | AI spec tooling | Spec-first, spec-anchored, and spec-as-source distinctions |
| Accord Project / Cicero | Legal agreements | Computable contracts, template conditions |
| ACE | Controlled English | Direct grammatical ancestor |
| Logical English | Controlled English / logic programming | Legal rules as executable readable English |
| Unison | Functional language | Lambda-calculus runtime, content-addressed code |
| Haskell | Functional language | Pure evaluation, referential transparency |
| Eiffel / DbC | Contract programming | Contracts as correctness criteria |
| Meyer / OOSC | Software engineering | Explicit obligations, invariants, correctness |
| Fowler DSLs / Analysis Patterns | Domain modeling | Domain language and recurring business structures |
| Clean Architecture | Architecture boundaries | Business rules independent from mechanisms |
| Hexagonal Architecture | Ports/adapters | Boundary ports and infrastructure adapters |
| DCR Graphs | Business processes | Declarative constraints over imperative flow |
| BPMN / BPEL | Business processes | Business-readable notation vs executable orchestration |
| Business rules engines | Rules management | Business rules outside application code |
| DDD | Domain modeling | Ubiquitous language, bounded contexts, domain events |
| EventStorming | Domain discovery | Collaborative discovery of events, policies, boundaries |
| Rules as Code / Computable Law | Legal/policy automation | Machine-consumable rules, validation, legal alignment |
| Catala | Law-specific DSL | Compiler-oriented formalization of statutory calculations |
| Blawx | Rules as Code tool | Legal reasoning with source-linked explanations |
| LegalRuleML | Legal rules | Norms, authorities, sources, deontic logic |
| Orleans | Distributed runtime | Virtual actor model, logical/physical separation |
| WebAssembly Component Model | Portable execution | Distributable parts with declared interfaces |
| WASI | Portable host interfaces | Host capabilities without embedding infrastructure assumptions |
| Gherkin / Cucumber | BDD testing | Natural-language specs, but still requires code step definitions |
| DMN | Business decisions | Executable decision semantics |
| Siemens Knowledge Fabric | Industrial AI | Empirical evidence: agentic code gen needs massive scaffolding |
| Kimi / Moonshot | AI code generation | Paradigm Spex replaces |
| LLMbda Calculus | LLM + math | LLM reasoning about formal expressions |
