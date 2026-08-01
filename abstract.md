# Abstract

> This is a draft for an abstract that could be used in a future whitepaper

Business specifications often appear precise in conversation while still containing ambiguity, contradictions, and unspecified cases. Conventional development and AI-assisted code generation both rely on a lossy translation from this informal business meaning into application code. During that translation, information may be lost, gaps may be filled implicitly, and assumptions may become part of the implementation without explicit approval.

SPEX explores a specification language in which a reviewed business specification becomes the single source of truth. The proposed language uses a restricted, computable form of English that can be discussed directly with domain experts while remaining suitable for formal analysis and deterministic compilation.

During authoring, an LLM acts primarily as an interviewer and discussion partner. A compiler and solver identify incomplete, ambiguous, inconsistent, or invalid statements. The LLM uses this feedback to ask focused questions and help reformulate informal business knowledge. The human must confirm that every reformulation still expresses the intended meaning.

Examples discussed with domain experts are used to uncover misunderstandings in the specification and can later be reused as integration tests.

Once accepted, the specification should compile without AI into a deterministic and distributable business core. AI-assisted engineering can then focus on the surrounding infrastructure, guided by declared interfaces and measurable non-functional requirements.

The research question is whether this separation can make AI-assisted software engineering more verifiable, privacy-preserving, energy-efficient, and less dependent on increasingly powerful commercial models.
