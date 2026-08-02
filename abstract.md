# Abstract

> This is a draft abstract for a possible future white paper.

Business specifications often appear precise in conversation while still containing ambiguity, contradictions, and unspecified cases. Conventional software development and AI-assisted code generation both depend on a potentially lossy translation from informal business meaning into application code. During this translation, information may be lost, gaps may be filled implicitly, and assumptions may enter the implementation without explicit human approval.

SPEX proposes a specification language in which an approved business specification becomes the authoritative source of business behaviour. The language would use a restricted, computable form of English that remains readable and discussable by domain experts while supporting formal analysis and deterministic compilation.

During authoring, an LLM would act primarily as an interviewer, discussion partner, and drafting assistant. A compiler would check syntax, types, references, and structural completeness, while solvers and other formal-analysis tools could identify contradictions, violated invariants, unreachable cases, and counterexamples. The LLM would use these diagnostics to ask focused questions and propose reformulations. Humans would remain responsible for confirming that every reformulation preserves the intended business meaning.

Executable examples would be discussed with domain experts to uncover misunderstandings and confirm shared interpretation. They would not replace or complete the specification, but could later be reused as integration tests against the completed system.

Once formally valid and approved, the specification would compile without AI into a deterministic and distributable business core. AI-assisted engineering could then focus on infrastructure that satisfies declared interfaces and measurable non-functional requirements.

The central research question is whether compiler-guided LLM assistance can make precise, human-authoritative specifications practical for realistic software systems, and whether smaller models can achieve results comparable to frontier models when supported by formal diagnostics.
