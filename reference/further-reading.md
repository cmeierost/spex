# Further reading

This page collects external material that helps frame Spex. It is not an endorsement list or a complete bibliography. The goal is to keep useful prior art close to the questions Spex is exploring.

## Controlled English and executable language

| Reading | Why it matters for Spex |
|---------|--------------------------|
| [Attempto Controlled English](https://www.ace-editor.org/) | Direct prior art for readable English with formal semantics. Spex inherits the ambition, but must avoid making the language feel like confusing pseudo-English. |
| [Attempto Controlled English overview](https://en.wikipedia.org/wiki/Attempto_Controlled_English) | Quick orientation to controlled natural language as knowledge representation, specification language, and query language. |
| [Generating Executable Scenarios from Natural Language](https://www.weizmann.ac.il/math/harel/sites/math.harel/files/users/user56/NLToLSCs_Cicling09_final.pdf) | Shows an older route from controlled natural language to executable scenarios. Useful for comparing translation pipelines with Spex's solver-guided authoring loop. |

## Formal specification and counterexamples

| Reading | Why it matters for Spex |
|---------|--------------------------|
| [TLA+](https://lamport.azurewebsites.net/tla/tla.html) | Major reference point for specifications as engineering artifacts. Especially relevant for the idea that testing code is insufficient for large state spaces. |
| [Alloy](https://alloytools.org/) | Strong prior art for finding counterexamples in formal models. Spex's solver loop needs this kind of feedback, but surfaced in business-readable language. |
| [Practical Formal Methods](https://github.com/ligurio/practical-fm) | Survey of real-world formal-methods use. Helpful for understanding why formal verification succeeds in narrow domains but remains hard to mainstream. |
| [Abstract State Machines, Alloy, B, TLA, VDM, and Z](https://link.springer.com/book/10.1007/978-3-642-54108-7) | Broad comparison point for established specification formalisms. Useful background for deciding what Spex should avoid becoming. |

## Model-driven development and its limits

| Reading | Why it matters for Spex |
|---------|--------------------------|
| [Computer-aided software engineering](https://en.wikipedia.org/wiki/Computer-aided_software_engineering) | Older CASE-tool lineage behind MDA and MDE: repositories, diagrams, generators, integrated workbenches, and round-trip engineering. Useful for understanding why "higher-level development" has repeatedly disappointed. |
| [OMG Model Driven Architecture](https://www.omg.org/mda/) | The canonical MDA vision: separate business/application logic from platform technology through platform-independent models and platform-specific mappings. This is one of Spex's closest ancestors. |
| [OMG MDA Guide 2.0](https://www.omg.org/cgi-bin/doc?ormsc/14-06-01) | Primary standard-style reference for MDA terminology and model transformation goals. Useful for understanding the PIM/PSM split Spex inherits and criticizes. |
| [Martin Fowler: Model Driven Architecture](https://martinfowler.com/bliki/ModelDrivenArchitecture.html) | Classic skeptical view: MDA needs a more effective programming environment than existing alternatives. Spex should answer this objection directly. |
| [InfoQ: 8 Reasons Why Model-Driven Approaches Fail](https://www.infoq.com/articles/8-reasons-why-MDE-fails/) | Useful critique of round-trip engineering, model/code drift, and the problem of technical models replacing business models. |
| [Model-driven engineering overview](https://en.wikipedia.org/wiki/Model-driven_engineering) | Broader context for MDE beyond OMG MDA, including CASE-tool history, domain models, model transformations, and tooling ecosystems. |
| [Model-driven architecture overview](https://en.wikipedia.org/wiki/Model-driven_architecture) | Basic background on MDA terminology, standards, and goals. |
| [Executable UML](https://en.wikipedia.org/wiki/Executable_UML) | Important older attempt to make models executable, testable, and compilable into implementations. Especially relevant to Spex's question of whether the model/spec can be the primary artifact. |
| [Shlaer-Mellor method](https://en.wikipedia.org/wiki/Shlaer%E2%80%93Mellor_method) | Predecessor lineage for Executable UML and action-language-based executable models. Useful for understanding older attempts to model domains independently from implementation. |
| [Object Constraint Language](https://www.omg.org/spec/OCL/) | Formal constraint language for UML models. Relevant because Spex also needs constraints, invariants, and unambiguous semantics, but wants them in controlled business language. |
| [Foundational UML](https://www.omg.org/spec/FUML/) | OMG standard for executable semantics of a UML subset. Useful prior art for the idea that models need precise execution semantics, not just diagrams. |
| [Action Language for Foundational UML](https://www.omg.org/spec/ALF/) | Action language for executable UML models. Relevant contrast: MDA needed action languages to become executable; Spex tries to keep business meaning in controlled specification text. |
| [Eclipse Modeling Framework](https://www.eclipse.org/modeling/emf/) | Major tooling ecosystem for model-driven engineering. Useful for studying both the power and the platform/tooling dependence of model-based approaches. |
| [Domain-specific modeling](https://en.wikipedia.org/wiki/Domain-specific_modeling) | Directly relevant to the idea that domain-specific language can raise abstraction above general-purpose programming. Spex differs by aiming for signed, business-readable text rather than tool-bound diagrams. |
| [MetaEdit+ / Domain-Specific Modeling](https://www.metacase.com/) | Long-running domain-specific modeling tool lineage. Relevant to Spex's interest in domain language, while highlighting the risk of heavy tool dependence. |

## Software engineering lineage

| Reading | Why it matters for Spex |
|---------|--------------------------|
| [Bertrand Meyer: Object-Oriented Software Construction](https://bertrandmeyer.com/OOSC2/) | Foundational text for Design by Contract and the idea that software correctness should be expressed through explicit obligations, not only tested after the fact. Spex pushes that concern from code contracts toward signed business specifications. |
| [Eiffel: Design by Contract](https://www.eiffel.org/doc/solutions/Design_by_Contract_and_Assertions) | Practical lineage for preconditions, postconditions, invariants, and executable obligations. Useful for comparing code-level contracts with Spex's goal of business-level contracts. |
| [Martin Fowler: Domain-Specific Languages](https://martinfowler.com/books/dsl.html) | Important background for language design that fits a domain rather than general-purpose programming. Spex is partly a domain-specific language question, but with business stakeholders as readers and signers. |
| [Martin Fowler: Analysis Patterns](https://martinfowler.com/books/ap.html) | Shows the long-standing attempt to model business domains explicitly and repeatably. Useful prior art for thinking about entities, events, responsibilities, and domain meaning before implementation. |
| [Robert C. Martin: The Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) | Classic statement of keeping business rules independent from frameworks, databases, UI, and external agencies. Spex radicalizes the same boundary by moving business meaning into the signed spec. |
| [Robert C. Martin: The Dependency Rule](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html#the-dependency-rule) | Useful shorthand for the architectural direction Spex inherits: dependencies should point inward toward policy, not outward toward mechanisms. Spex treats the signed business specification as the innermost policy artifact. |

## Functional vs nonfunctional requirements

| Reading | Why it matters for Spex |
|---------|--------------------------|
| [Functional vs. Nonfunctional Requirements](https://www.altexsoft.com/blog/functional-and-non-functional-requirements-specification-and-types/) | Accessible overview of the distinction Spex reframes as spec meaning vs infrastructure pressure. |
| [Do Software Architectures Meet Extra-Functional or Non-Functional Requirements?](https://www.infoq.com/articles/architectural-non-functional-requirements/) | Useful discussion of how quality attributes, constraints, and architecture drivers shape system design beyond business features. |
| [ISO/IEC/IEEE 29148 overview](https://www.iso.org/standard/72089.html) | Requirements engineering standard often referenced for qualities such as unambiguity, verifiability, and singularity. |

## Architecture boundaries and ports

| Reading | Why it matters for Spex |
|---------|--------------------------|
| [Alistair Cockburn: Hexagonal Architecture](https://alistair.cockburn.us/hexagonal-architecture/) | Original Ports and Adapters essay. Closest architecture ancestor for boundary ports and infrastructure outside the business core. |
| [Hexagonal Architecture resources](https://jmgarridopaz.github.io/content/hexagonalarchitecture.html) | Expanded explanation of ports/adapters and keeping application meaning independent of external technology. |
| [Clean Architecture](https://8thlight.com/insights/the-clean-architecture) | Related boundary model: dependencies should point inward toward business rules. Spex pushes this boundary from code architecture toward signed specifications. |

## Portable and distributable execution

| Reading | Why it matters for Spex |
|---------|--------------------------|
| [WebAssembly](https://webassembly.org/) | Reference point for portable execution artifacts that are not tied to one host environment. |
| [WebAssembly Component Model](https://component-model.bytecodealliance.org/) | Important analogy for portable, composable parts with declared interfaces. Spex's execution target is conceptually adjacent, though business semantics drive the split. |
| [wasmCloud: Interfaces, WASI, and the Component Model](https://wasmcloud.com/docs/overview/interfaces) | Concrete documentation for components with imports and exports. Useful analogy for boundary ports and infrastructure bindings. |
| [WASI](https://wasi.dev/) | Reference point for standardizing how portable modules interact with host capabilities without embedding host-specific assumptions in the core module. |

## Business rules, decisions, and computable contracts

| Reading | Why it matters for Spex |
|---------|--------------------------|
| [Apache KIE / Drools](https://kie.apache.org/) | Business rules engine lineage: separating rules from application code. Useful contrast because rule engines usually address rule fragments, not the whole signed business contract. |
| [Decision Model and Notation](https://www.omg.org/dmn/) | Business-readable decision modeling with execution semantics. Useful comparison for what Spex may borrow and where tables/visual models may be too limited. |
| [Camunda DMN tutorial](https://docs.camunda.org/manual/latest/user-guide/dmn-engine/) | Practical view of executable decision tables. Helpful contrast with Spex's goal of broader signed business contracts. |
| [Accord Project Cicero](https://accordproject.org/projects/cicero/) | Computable legal agreements with templates, data models, and executable logic. Strong prior art for signed/business-readable artifacts with machine execution. |
| [LegalRuleML Core Specification](https://docs.oasis-open.org/legalruleml/legalruleml-core-spec/v1.0/os/legalruleml-core-spec-v1.0-os.html) | Formal standard for representing legal norms, deontic concepts, authorities, sources, temporal validity, and defeasible reasoning. Important legal-spec prior art, though much heavier than Spex wants to be. |
| [Conversion of Legal Agreements into Smart Legal Contracts using NLP](https://arxiv.org/abs/2210.08954) | Shows the difficulty of extracting computable structure from legal language. Relevant to why Spex should use LLMs as authoring assistants, not final authorities. |

## Domain language and business process discovery

| Reading | Why it matters for Spex |
|---------|--------------------------|
| [Domain-Driven Design resources](https://www.domainlanguage.com/ddd/) | DDD's ubiquitous language, bounded contexts, aggregates, and domain events are major prior art for putting business language at the center of software design. |
| [EventStorming](https://www.eventstorming.com/) | Collaborative discovery method for complex business domains. Relevant to Spex's authoring loop because it treats business events and boundaries as shared artifacts before implementation. |
| [Business Process Model and Notation](https://www.omg.org/spec/BPMN/) | Business process notation intended for both business and technical users. Useful prior art for process semantics, but it often drifts into execution and tooling concerns. |
| [Business Process Execution Language](https://docs.oasis-open.org/wsbpel/2.0/OS/wsbpel-v2.0-OS.html) | Execution-oriented process language for web-service orchestration. Useful contrast with Spex's desire to keep infrastructure protocols out of the business specification. |

## LLMs in requirements engineering

| Reading | Why it matters for Spex |
|---------|--------------------------|
| [Large Language Models for Requirements Engineering: A Systematic Literature Review](https://arxiv.org/abs/2509.11446) | Recent survey of how LLMs are used for elicitation, validation, classification, and requirements-related workflows. Useful for grounding Spex's claim that LLMs belong in authoring. |
| [Research directions for using LLM in software requirement engineering](https://www.frontiersin.org/journals/computer-science/articles/10.3389/fcomp.2025.1519437/full) | Survey-style view of NLP/LLM use in requirements extraction, analysis, and ambiguity reduction. |
| [From issue titles to requirements](https://link.springer.com/article/10.1007/s00766-026-00462-z) | Relevant to quality criteria such as unambiguity, verifiability, and singularity in generated requirements. |
| [Evaluating LLM Performance in Requirement Generation](https://werpapers.dimap.ufrn.br/papers/WER2025/wer202511.pdf) | Useful empirical angle on LLMs extracting functional and nonfunctional requirements from stakeholder interviews. |

## How to use this list

When adding a new Spex document, ask which reading cluster it touches:

- controlled English and formal meaning;
- solver feedback and counterexamples;
- failure modes of model-driven development;
- functional vs nonfunctional requirements;
- ports and infrastructure boundaries;
- portable/distributable execution;
- computable contracts and business decisions;
- domain language and business process discovery;
- LLM-assisted requirements authoring.

A good Spex claim should either build on one of these lines of prior art or explain why Spex deliberately departs from it.
