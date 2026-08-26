# Spec-Driven Development Gets Your Spec Wrong — Part 1

*Once the intended behaviour has been specified precisely, why ask a probabilistic model to interpret it again — and then test whether that interpretation was correct?*

*Clemens Meier · 13 min read*

> **AI disclosure:** I used AI assistance while editing and refining the English in this article. The ideas, arguments, technical concepts, and conclusions are my own.

*[Image: an illustration of a typical spec-driven development flow.]*

That’s what SDD looks like to me. And I’m quite sure an AI system would represent it in much the same way.

AI has changed software development faster than anything I have seen in my career. A capable coding agent can now turn an idea into working software at astonishing speed.

But the difficult part often comes later: requirements change, existing behaviour has to be preserved, and we need to know whether the generated software still does exactly what was intended. No matter how fast the code was generated, someone still has to establish that.

That makes two old questions more important than ever:

> What exactly should the software do?

> How do we know that the software actually does exactly that?

These questions are not new. Software engineers have always filled gaps in requirements, made assumptions, misunderstood intentions and forgotten to update specifications after changing the code.

## SDD Does Not Close the Semantic Gap

Many current AI-oriented SDD workflows resemble the old development process with AI inserted into the translation steps.

Traditionally, humans would read an incomplete specification, interpret what it probably meant, and turn that interpretation into code.

AI does the same, but faster and at a much greater scale.

SDD puts much more emphasis on answering the first question:

> What exactly should the software do?

But then, when we are finally satisfied with what the software should do…

…we hand the specification to a blazing-fast probabilistic model and ask it to translate the specification into code.

So the second question remains:

> How do we know that the software actually does exactly what the specification says?

SDD also tries to address the second through tests, reviews, guardrails and consistency checks, but that is not enough.

## Why More SDD Artifacts Don’t Close the Gap

GitHub Spec Kit and Kiro are examples of current workflows that progressively derive plans or designs and implementation tasks from specifications before generating code.

A simplified version looks like this:

```text
Specification → Plan/Design → Tasks → Implementation
```

Modern SDD tools try to control this uncertainty with clarification steps, checklists, consistency analyses, acceptance criteria, reviews and tests. These mechanisms can reduce ambiguity and semantic drift, but they also create more artifacts to generate, inspect and maintain.

And whenever an LLM transforms one representation into another, it still has to interpret what the previous representation means. That requires additional inference, tokens and computation.

If the intended behaviour has already been defined precisely, why keep spending effort checking whether repeated interpretations preserve the same meaning? A deterministic translation could preserve that meaning directly.

The model providers may not mind the extra inference.

Our planet and your wallet probably do.

## How Important Are Specifications Anyway?

For any particular piece of behaviour, consider three simplified cases:

1. We don’t care about the behaviour.
2. The specification is incomplete.
3. The specification is precise.

These cases can coexist within the same application. Some behaviour may be deliberately left open, some may still be underspecified, and some may need to be defined precisely.

So let’s look at each case in more detail.

### 1. We Don’t Care About the Behaviour

Then we can just vibe code.

> “Hey Claude! How are you? Write me a 3D snake game — please— if you have time — or whatever.”

AI might produce an entertaining 3D snake game. Nobody cares exactly how it works, and perhaps it even becomes the next viral game on the internet.

There is nothing fundamentally wrong with that.

Vibe coding can work perfectly well when we are happy to look at the result and decide afterwards whether we like it.

But would you accept the same approach for software managing transactions in your bank account?

You cannot expect even the smartest AI to implement exactly what you intended if you never expressed exactly what you intended.

Humans were never fundamentally better at this. A lot of existing software was “specified” through emails, meetings and conversations — the historical version of vibe coding.

But for software that increasingly controls our daily lives, the question is no longer simply: Do we care enough about the behaviour to specify it?

The difference is scale. AI can now generate software much faster than humans can inspect it, making line-by-line review increasingly unrealistic as the final mechanism for deciding what a system actually does.

And as AI becomes more capable, this question becomes more important, not less:

> Which decisions are we willing to let AI infer — and which must remain explicitly defined by humans?

This is ultimately a philosophical question. AI may eventually make many decisions better than we do. But if we also let AI decide what the software should do, humans stop being the authors of that behaviour.

Software already controls our money, critical infrastructure and even weapons systems.

At some point, the question is no longer whether AI can implement our decisions. It is whether we still want the decisions to be ours.

And:

> If we decide that we no longer need to know exactly what such software is supposed to do, we are not merely delegating programming — we are delegating the decisions themselves.

That is why I strongly believe specifications matter more than ever.

### 2. The Specification Is Incomplete

Then somebody has to decide what the missing behaviour should be.

A junior developer may make that decision. An AI system can make that decision too — perhaps even better in some cases.

But it is still a guess.

And then someone has to check what was generated.

But against what can it be checked?

We cannot establish that software matches an intended business behaviour that was never defined.

So during implementation, missing behaviour often ends up being defined through tests written by the developer — human or AI.

I have seen many tests that clearly state what the system should do, but nobody can point to the business specification from which that behaviour was derived.

In many SDD workflows, tests and guardrails have quietly become part of the specification — hidden inside technical artifacts that the domain expert responsible for the behaviour may never read.

When the specification is incomplete, tests can become an accidental hidden specification.

### 3. The Specification Is Precise

Now the situation changes.

If the specification precisely defines the required behaviour, then the implementation has one primary obligation:

> Conform to the behaviour defined by the specification.

There may still be many valid implementations. The specification can define the functional behaviour and domain data structures, while leaving technical implementation choices open — for example which sorting algorithm to use, how persistence is realised, or how the user interface is rendered. Non-functional requirements can further constrain those choices through measurable goals such as performance.

But most current AI development workflows still ask an LLM to interpret that specification and generate the implementation.

And because we do not fully trust that translation, we add tests, reviews and guardrails to check whether the generated business behaviour still matches what we intended — much as we do when humans write the code.

Tests, reviews and guardrails are still valuable for catching regressions, integration failures and measurable technical problems. Here, however, I am specifically talking about tests whose purpose is to check whether the business code behaves as specified.

That raises another question:

> Who or what verifies that the tests themselves express what the specification says?

When implementation and tests are independently derived from the specification, agreement between them does not by itself establish conformance to the specification. We would still need to know that both derivations preserve its meaning correctly.

In other words, agreement between implementation and tests is not the same as demonstrated conformance to the specification.

I call this **unverified verification**.

This leads to a simple conclusion:

> If the specification already defines the intended behaviour precisely, there should be nothing left to interpret.

By “precisely” I do not mean merely well-written natural language. I mean that the relevant statements are expressed in a language whose valid constructs have defined semantics, so that the same statement cannot legitimately acquire a different meaning during implementation.

Any valid implementation should preserve that meaning.

So why do we ask an LLM to reinterpret the specification into code — and then build tests, reviews and guardrails to check whether that interpretation was correct?

At that point, interpretation is no longer a strength.

It is the problem.

If the specification leaves a required decision unspecified, someone has to guess— the human or the model.

If the specification defines the required behaviour exactly, there is nothing left to guess.

That is the semantic gap that current AI-oriented SDD still has to bridge.

## Can We Make Probabilistic Translation Reliable Enough?

### What about reviews?

Human or AI review can reduce risk by checking traceability, contradictions and observable behaviour. But review cannot create an authoritative meaning that the specification itself does not define.

### What About Better AI?

Some people have already asked me:

> “But what if future AI becomes completely deterministic?”

Determinism does not solve ambiguity. If a specification leaves room for interpretation, there may be no single correct interpretation to find.

Current AI-oriented SDD still depends on sufficiently capable agents, combined with validation and guardrails, to make specification-to-code translation reliable enough in practice.

The real problem is not merely the quality of the translation.

Ordinary natural language does not come with formally defined semantics that assign each well-formed statement a precisely defined meaning. A deterministic translator cannot preserve a meaning that was never formally defined in the first place.

Once the specification is expressed in a language with formally defined semantics, however, there is no need for an LLM to infer what it means.

To translate a language with formally defined semantics, we already have a technology. It is called a compiler.

A compiler does not make the trust problem disappear. The compiler toolchain itself must be trusted to preserve the semantics defined by the language. But that is a fundamentally different problem from repeatedly asking a probabilistic model to infer the meaning of each specification.

LLMs are good at interpreting ambiguity.

Compilers are good at preserving defined meaning.

Once a specification gives the required behaviour a precisely defined meaning, probabilistic interpretation is the wrong semantic bridge.

Better LLMs alone cannot remove this semantic gap.

> If we can define behaviour precisely, why ask AI to guess what it means again?

## What Should AI Decide?

There is a big difference between:

> what behaviour the system must guarantee
>
> and
>
> how that behaviour is implemented.

Functional requirements define the behaviour of the system: its business rules, decisions and state transitions. If that behaviour matters, it should have precise semantics.

The technical implementation can remain open. Persistence, integration, deployment and even UI rendering can be designed and implemented in many different ways by AI, humans or other tools.

Non-functional requirements can constrain those implementation choices through measurable goals such as performance, availability, durability, technical security properties or cost.

Open implementation choices do not imply unrestricted or unreviewed AI generation. Some areas — especially security-critical infrastructure — may require stricter constraints, specialist review, approved components or independent verification.

What matters is that the requirements define the guarantees the infrastructure has to provide without unnecessarily prescribing how it provides them.

If an account balance must survive a restart, that durability requirement tells the infrastructure what it must guarantee. Whether it is realised with PostgreSQL, an event store or something else remains an implementation decision.

If only a certain role may approve a payment, that authorization rule defines required behaviour. Whether it is enforced through an identity provider, signed claims, access-control policies or another authorization mechanism remains an implementation decision.

The same distinction applies to the user interface. How an interface looks and is rendered can remain an implementation choice. But how application state changes in response to a user action is functional behaviour and therefore belongs to the specification.

Functional behaviour is specified semantically. Infrastructure is constrained by the guarantees and measurable goals it has to satisfy.

So the boundary is not between software that AI may or may not write. It is between decisions that have already been made and implementation choices that remain open.

AI is extremely useful for finding good ways to implement the guarantees that have already been defined, but we do not have to leave those guarantees themselves for AI to decide.

## Does the Right Architecture Solve It?

Clean Architecture, Onion Architecture, and Hexagonal Architecture already separate business behaviour from technological mechanisms.

That is exactly the boundary we need for AI:

> Required behaviour should not be reinterpreted, while the surrounding technical implementation can remain flexible within its defined constraints.

AI can be very effective at implementing infrastructure around a protected business core. And the source code itself can serve as the specification — but that comes with an important consequence:

> That source code has become the only source of truth for that behaviour.

That is not necessarily a problem if the person responsible for the business behaviour can understand the complete implementation well enough to review it directly.

But that knowledge is then closely tied to the people who understand the implementation. If they leave, the organisation may be left with code that still runs, but without a clear shared understanding of why it behaves the way it does.

An LLM can of course explain the code to the next person. But that explanation is again a probabilistic interpretation of the implementation. It may help us understand the code, but it does not give us an authoritative statement of what the software is supposed to do.

And once the people who define or approve the behaviour cannot reliably understand the implementation, we need another representation they can review.

Then we are back to maintaining two representations of the same behaviour — specification and code — and the original problem returns:

> How do we know that the code still behaves exactly as the specification requires?

## What If We Compile Specifications?

What if the specification could be written in constrained, natural-language-like sentences that domain experts can read without learning a modelling notation — and still compile directly?

Not ordinary English. Natural language depends heavily on context. In a face-to-face conversation with a domain expert, we can ask questions, notice uncertainty and clarify what was meant. Written requirements often lose much of that context.

And many perfectly normal words are simply not precise enough to compile. What is a “large” withdrawal? $1,000? $10,000? Half of the account balance? Humans can use words like “large”, “recent” or “significant” without defining their exact boundaries. An LLM might just guess. A compiler cannot.

A specification needs definitions, not impressions.

And so does a compiler.

In his 1978 note on “natural language programming”, Dijkstra argued against replacing the precision of formal symbolism with unrestricted natural language.

But natural language can be restricted to a defined vocabulary and grammar, with formally defined semantics for every well-formed construct. Controlled natural languages such as Attempto Controlled English have explored this idea for decades.

A recurring challenge for controlled natural languages has been usability. A language that looks like English invites people to write ordinary English — only for the compiler to reject perfectly understandable sentences. This mismatch between apparent familiarity and actual grammatical restrictions can be frustrating.

This is where LLMs change the equation.

Formal semantics are not new. But LLMs may make formally constrained languages much more practical to use, because humans do not need to learn every syntactic restriction before expressing their intent.

An LLM can help translate fuzzy human intent into valid controlled language. But it does not get the final say. The developer or domain expert reads the precise result and decides whether it expresses what was intended.

The LLM may propose the specification. It does not decide the required behaviour, and it does not define the language semantics.

Once that precise form has been accepted, probabilistic interpretation should stop for the behaviour it defines.

The specification becomes the authoritative source for the business behaviour it defines — a shared language between the people who define that behaviour and the people who implement the system.

## Can Compiled Specifications Reduce the Verification Burden?

Tests can show that an implementation behaves as the tests expect, but not necessarily that it conforms to the specification — unless the tests themselves are known to represent that specification correctly.

If the specification is executable and deterministically translated, that extra correspondence no longer has to be reconstructed through independently written tests, reviews and guardrails — provided that the compiler toolchain can be trusted to preserve the language semantics.

For high-assurance systems, that trust can go beyond testing: projects such as CompCert demonstrate that semantic preservation can be formally and machine-checked.

Tests still matter for integration, measurable technical requirements and validating whether the specification expresses the intended behaviour.

But:

> Tests no longer need to compensate for a probabilistic translation of already-defined business behaviour.

## Conclusion

AI has made software generation dramatically faster. That makes it even more important to know which decisions we are delegating and which ones we are not.

If behaviour matters enough to specify precisely, we should not ask a probabilistic model to rediscover its meaning every time we implement it.

The principle is simple:

> Let AI interpret what is still open.
>
> Compile what has already been decided.

The challenge is not inventing executable specifications. They have existed for decades.

The challenge is making them practical enough for everyday software development: precise enough to compile, natural enough to read, and easy enough to write with AI assistance.

That is the idea I want to explore next with SPEX: a controlled natural language that compiles specifications into an executable business core.

If you disagree, think I am missing something, or have run into the same problem, leave a comment or send me a message.

In Part 2, I introduce SPEX as a concrete implementation of that idea:

[SPEX: Executable Specifications for AI-Assisted Development — Part 2](./spex-executable-specifications-for-ai-assisted-development.md)

## References

- GitHub Spec Kit — Documentation
- Kiro Specs — Documentation
- Thoughtworks — *Spec-driven development. Unpacking one of 2025’s key new engineering practices*
- Kapil Viren Ahuja — *Spec-Driven Development Isn’t Broken. It will collapse.*
- Attempto project resources
- CompCert documentation
- E. W. Dijkstra Archive — *On the foolishness of “natural language programming”* (EWD 667)
- Valentina Servile — *Should we still design code for humans?*
