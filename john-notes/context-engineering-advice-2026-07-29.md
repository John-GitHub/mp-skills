<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Kindly review the following document. Explain best practices in context engineering and context patterns in general, then correlate what I've learned in the attached.

Ask Matt Skill Review
Matt opens the skill with two compact statements:
You don't remember every skill, so ask.
A flow is a path through the skills.
Together, they explain why the router exists and provide the mental model it will use.
“You don't remember every skill, so ask.”
A limitation followed by a response
The sentence contains two clauses joined by so:
You don't remember every skill (limitation), so ask (response).
Expanded, it means:
Because you cannot reliably recall every available skill, consult the router.
The first clause identifies a predictable failure. The second gives the corrective behavior. This produces a compact decision rule:
incomplete recall → consultation
The word every matters. Matt is not claiming that the reader remembers nothing. He is saying that complete recall is unreliable—and complete recall is what choosing from the full skill set would require.
One sentence, two audiences
The explicit you makes the sentence sound like advice to the user. Inside a SKILL.md, however, the immediate operational audience is the agent.
The sentence therefore supports two compatible readings:
For the agent: Do not improvise a complete skill map from uncertain recall; use this router.
For the user: You do not need to memorize the available skills; ask the router where to begin.
The command ask is an imperative whose subject is an implied you. The heading, “Ask Matt,” supplies what the short command leaves unstated: whom to ask and why.
This dual audience is useful because a skill file is both executable context for an agent and readable documentation for people who inspect it.
“Remember” as deliberate compression
Remember is ordinary human language, not a precise description of model operation. A language model does not remember an installed skill inventory as a person remembers a list. Its behavior depends on which skill descriptions and instructions the surrounding system makes available in the current context.
A technically exact version would be longer:
You cannot reliably identify every relevant user-invoked skill from the currently available context.
Matt's version preserves the operational point while removing implementation detail. The important invariant is simple: neither the user nor the agent should depend on unaided, comprehensive recall.
Why it is effective context engineering
Context engineering is the deliberate design of information and instructions placed before a model so that useful behavior becomes more likely.
This sentence follows several useful practices:
Name the failure mode. The agent may overlook a relevant skill.
Provide the recovery behavior. Consult the router instead of guessing.
Connect them explicitly. So turns an observation into a reason for action.
State the intent, not the implementation. The instruction can remain useful even if the skill-loading mechanism changes.
Use familiar language. A short human explanation consumes less context than an account of retrieval and invocation architecture.
The sentence may bias the model away from confident improvisation and toward consultative behavior. That is a plausible effect of the wording, not a guarantee about how every model will respond.
“A flow is a path through the skills.”
Linguistic structure
The sentence has a simple copular structure:
A flow (subject) is (copula) a path through the skills (subject complement).
A copula is a linking verb—usually a form of be—that connects a subject to an identity, category, or description. Here, is connects the term being defined, flow, to its defining description, a path through the skills.
This makes the sentence a copular definition:
term being defined + is + defining description
In formal terminology, a flow is the definiendum: the term to be explained. A path through the skills is the definiens: the expression that explains it.
The definition is local rather than universal. Matt is effectively saying:
In this skill system, understand a “flow” as a path through the available skills.
Why the definition is effective
English commonly uses X is Y to establish a concept directly. The structure presents X as the topic and gives Y as the model for understanding it.
Matt maps the abstract idea of a flow onto the familiar spatial idea of a path. That metaphor suggests order, movement, intermediate points, and destination without requiring him to state each idea separately.
The sentence compresses a larger explanation:
Skills may be used in a meaningful sequence, and that sequence can be treated as a route.
The rest of the skill can now reuse the same spatial vocabulary: main flow, on-ramp, route, merge, and underneath. One short definition establishes the shared conceptual system.
Likely effect on a language model
A language model does not store the sentence as a formal symbolic equation. During inference, the surrounding tokens shape one another's contextual representations.
The construction associates flow with path and anchors both to skills. This reduces ambiguity: flow becomes more likely to mean an ordered route through a workflow than fluid movement, cash flow, or a psychological state.
It also makes later route-related language more probable. Terms such as step, on-ramp, destination, and merge fit the conceptual frame the definition has established.
It is reasonable to say that the sentence biases later predictions toward a path-based interpretation. It would be too strong to claim that we know its exact geometry in the model's latent space or that attention weights alone explain the model's understanding.
“Everything else is standalone, or a vocabulary layer that runs underneath.”
Classifying the remainder
This is another copular classification:
Everything else (subject) is (copula) standalone, or a vocabulary layer that runs underneath (alternative subject complements).
Everything else refers back to the categories Matt has already introduced: the main flow and its on-ramps. It gathers the remaining skills into a single remainder set.
The sentence therefore completes a coarse opening map:
principal routes → main flow and on-ramps
remainder → standalone skills and cross-cutting vocabulary
The word everything presents this classification as exhaustive. In practice, the later document introduces finer headings such as codebase health and crossing sessions. The opening is therefore rhetorically exhaustive but operationally coarse: it gives the reader enough structure to become oriented without presenting the complete inventory.
“Standalone” as workflow independence
Standalone compresses a routing rule:
This skill can be reached for independently; it is not a mandatory stage in a sequential workflow.
It describes how a skill participates in the workflow, not its formal invocation policy. A standalone skill may still be user-invoked or model-invoked.
Nor does standalone mean that the skill is atomic, stateless, isolated, or dependency-free:
/prototype is independently reachable but can also serve as a detour within the main flow.
/teach is standalone while maintaining a stateful workspace across sessions.
/research is standalone, but the artifact it produces can feed /grill-with-docs.
The useful distinction is between independent entry and required sequence, not between isolated and interconnected implementation.
“A vocabulary layer that runs underneath”
This phrase distinguishes a cross-cutting discipline from a position in a workflow.
A flow organizes skills into a sequence of conditional steps:
first → next → branch or detour → destination
A layer can inform several of those steps without occupying a numbered position of its own.
The relative clause that runs underneath extends the spatial metaphor. Underneath presents the vocabulary as foundational and supporting; runs suggests continued applicability across the work above it.
This does not mean that the vocabulary skill executes continuously in the background or becomes globally available through the model's attention mechanism. A more grounded paraphrase is:
These disciplines can shape several workflow skills without becoming sequential stages in the flow.
The concrete vocabulary layers
The repository identifies two such layers: /domain-modeling and /codebase-design.
/domain-modeling is not a passive glossary. It is an active discipline that:
challenges vague or conflicting terminology;
tests domain relationships with concrete scenarios;
checks claims against the code;
updates CONTEXT.md when terms are resolved;
records qualifying decisions as ADRs.
Other workflow skills use this discipline when domain language must remain coherent. /grill-with-docs, /triage, and /wayfinder explicitly invoke or work with /domain-modeling.
/codebase-design supplies a different vocabulary: module, interface, depth, seam, adapter, leverage, and locality. It establishes the terms and principles used to reason about deep, testable modules. /tdd and /improve-codebase-architecture speak through that vocabulary.
The phrase runs underneath therefore compresses an actual repository relationship:
not a step every workflow must visit
a shared discipline that multiple steps can invoke or use
The metaphor succeeds because later skill definitions make the cross-cutting relationship concrete.
Likely context-engineering effect
Before presenting detailed routing rules, the sentence gives the model familiar categories for interpreting the remaining skill descriptions.
A coarse routing heuristic becomes available:
Is this skill part of a route?
If yes, locate it in the main flow or an on-ramp.
If no, consider whether it is independently reachable or supports several steps through shared vocabulary.
Later uses of standalone, vocabulary, and underneath reinforce that distinction. This can make a workflow-versus-cross-cutting interpretation more likely in the current context.
The sentence does not prove that the model constructs a particular latent-space topology, configures global attention, suppresses hallucinated dependencies, or substantially reduces prediction entropy. Those would be hypotheses to test through evaluation.
Its defensible contribution is simpler: it supplies a compact conceptual classification that later routing instructions can refine.
Ask Matt Introduction Summary
The introduction establishes an orientation model before presenting the detailed skill inventory. In three short statements, Matt explains why the router exists, defines the metaphor used to navigate it, and classifies the skills by their relationship to the workflow.
The model established
Question resolvedMatt's languageContext establishedRouting consequence
Why does the router exist?
“You don't remember every skill, so ask.”
Complete recall is unreliable; consultation is the recovery behavior.
Use the router instead of improvising the skill inventory.
What is a flow?
“A flow is a path through the skills.”
Skill usage can be understood spatially as movement along a route.
Locate the user's situation on a path through the available skills.
How do skills participate in the system?
Main flow, on-ramps, standalone skills, and vocabulary underneath
Some skills form routes, some provide independent entry points, and others support several routes through shared language.
Recommend a route, a standalone skill, or a cross-cutting vocabulary discipline.
Reduced to an operational sequence, the model is:

1. Consult the router.
2. Locate the situation on the map.
3. Recommend a route, an independent entry point, or a supporting discipline.

This is an orientation layer rather than a complete routing specification:
Introduction: How should this skill system be understood?
Later sections: Which skill or sequence fits this particular situation?

A concrete example: /domain-modeling
/domain-modeling demonstrates what Matt means by a vocabulary layer that “runs underneath.”
It is not a numbered step in the main flow. It is an active, cross-cutting discipline that several workflow skills invoke when domain language must be challenged, clarified, checked against the code, or recorded in CONTEXT.md and ADRs.
This makes the spatial metaphor concrete:
/domain-modeling supports several routes without occupying a fixed position on any one route.
Its companion layer, /codebase-design, performs the same structural role for architectural language: it supplies the exact vocabulary and principles used by skills such as /tdd and /improve-codebase-architecture.
Context-engineering evaluation
DimensionAssessmentReason
Purpose clarity
Strong
The opening immediately names the failure the router remedies.
Conceptual compression
Strong
Three statements establish motivation, a navigation model, and a coarse taxonomy.
Context economy
Strong
Familiar language carries more structure than a longer explanation of retrieval and routing mechanics would require.
Metaphorical coherence
Strong
Flow, path, on-ramp, and underneath reinforce one spatial model.
Human readability
Strong
The prose works as agent context while remaining understandable to someone inspecting the skill.
Behavioral precision
Moderate
The introduction orients the agent but cannot route a real request without the later sections.
Taxonomic precision
Moderate
The categories are deliberately coarse, and the later structure introduces finer distinctions.
Dependence on later context
High but appropriate
The metaphors become operationally reliable only when later sections connect them to concrete skills and decisions.
The installed 1.2.0 file also contains a small consistency problem: the introduction says that two on-ramps merge onto the main flow, while the later \#\# On-ramps section presents three bullet-pointed starting situations. This does not destroy the orientation model, but it demonstrates the risk of compressing a changing inventory into a specific count.
Why the approach works
The introduction applies several reusable context-engineering practices:
State the failure before the machinery. The agent learns why consultation is necessary before receiving routing details.
Define local terminology early. Matt gives flow a document-specific meaning before relying on it.
Organize before enumerating. The agent receives a map before encountering the full inventory.
Use a coherent metaphorical system. The spatial terms reinforce one another rather than introducing unrelated analogies.
Progress from coarse orientation to concrete rules. The introduction establishes categories; later sections provide decisions, exceptions, and skill relationships.
Write for execution and inspection. The wording can guide an agent while remaining legible to a human reviewer.
Overall assessment
The introduction is highly effective as compressed orientation and intentionally insufficient as operational routing logic.
Its sparsity works because each sentence establishes a reusable relationship:
known limitation → corrective behavior
new concept → familiar mental model
remaining elements → coarse workflow categories
The remainder of the skill then converts those relationships into concrete routes and cross-skill dependencies. Without that follow-through, the introduction would be memorable but underspecified.
The reusable lesson is not simply to write less. It is to make each short statement establish structure that later instructions consistently reuse.
Handoff Record and System State
Cold-start instruction
This section is the persistent state record and the sole conversational context carried across session restarts.
On a cold start:
Adopt the required persona recorded below.
Read this complete review document to recover the detailed analysis.
Read the installed runtime SKILL.md identified below.
Await John's selection of the next passage. Do not advance independently.
Role and status
required_persona: "Senior technical writer specializing in AI training for software project managers"
status: "awaiting_next_passage"
last_completed_topic: "The complete Ask Matt introduction and its context-engineering evaluation"

Explain technical concepts accurately for a reader who is technically competent but still developing expertise in AI systems and context engineering.
Purpose and sources
Goal: Study Matt Pocock's ask-matt skill closely to understand how its sparse language influences human comprehension and agent behavior. Extract reusable skill-writing and context-engineering practices, then eventually evaluate Matt's implementation against John's own version.
Persistent state and review document:
john-notes/ask-matt-skill-review.md

Installed runtime artifact under review:
C:\Users\johnp\.claude\plugins\cache\mattpocock\mattpocock-skills\1.2.0\skills\engineering\ask-matt\SKILL.md

The runtime artifact is the installed ask-matt skill loaded by Claude Code from the mattpocock-skills plugin at version 1.2.0. It is distinct from the editable source file in Matt's repository.
Before analyzing a new passage, read both the complete review document and the installed runtime artifact. Ground the discussion in the runtime artifact rather than relying on memory.
Settled findings
Router rationale
You don't remember every skill, so ask.
The sentence uses a limitation-and-response structure:
incomplete recall → consultation
It operationally addresses the agent while remaining readable as guidance to the user. It names a predictable failure—incomplete recall of the skill inventory—and immediately supplies the recovery behavior: consult the router rather than improvise.
Copular definition
A flow is a path through the skills.
The sentence associates the new term flow with the familiar concept path. In context, this can bias subsequent interpretation and generation toward path-related ideas such as sequence, direction, branching, intermediate points, and destination.
This is a plausible account of the sentence's effect on model behavior. Do not overstate it as proof of a particular latent-space geometry, attention pattern, or reduction in prediction entropy.
Remainder classification
Everything else is standalone, or a vocabulary layer that runs underneath.
The sentence classifies skills outside the primary routes by workflow role. Standalone means independently reachable rather than isolated, stateless, or dependency-free. A vocabulary layer is a cross-cutting discipline that can inform multiple workflow steps without occupying a numbered position in the flow.
The repository makes this relationship concrete through /domain-modeling and /codebase-design. These are behaviorally active disciplines used by other skills, not passive dictionaries or hidden background processes.
Introduction synthesis
The introduction installs a three-part orientation model:
incomplete recall → consult the router
flow → path through skills
skill participation → routes, independent entry points, or cross-cutting disciplines
This is effective context engineering because it establishes purpose, a shared mental model, and coarse categories before presenting the detailed inventory. Its behavioral and taxonomic precision depend on the later sections cashing out the metaphors through concrete routing rules.
/domain-modeling demonstrates the meaning of a vocabulary layer: it actively maintains domain language across several workflows without occupying a fixed position in the main flow.
The installed 1.2.0 skill says that two on-ramps merge onto the main flow, while its later \#\# On-ramps section contains three bullet-pointed starting situations. Treat this as a source inconsistency rather than silently forcing the later inventory into the opening count.
Context continuity
A restarted session does not automatically receive the preceding conversation. Durable conclusions, working constraints, and the current stopping point must therefore be recorded explicitly in this handoff.
Formatting choice
The handoff combines:
YAML for compact, predictable status fields.
Markdown for nuanced decisions and human-readable instructions.
This format was selected for clarity and maintainability. Do not claim improved model performance without evaluation evidence.
Current position
Review of the complete Ask Matt introduction is finished, including its aggregate context-engineering evaluation:
You don't remember every skill, so ask.
A flow is a path through the skills. Most paths run along one main flow, and two on-ramps merge onto it. Everything else is standalone, or a vocabulary layer that runs underneath.
The next unreviewed section begins at:

## The main flow: idea → ship

No passage from that section has been selected. Await John's selection and do not advance independently.
Working protocol
One passage at a time: Analyze only the passage John selects. Do not advance to adjacent material independently.
Dual analysis: Investigate both the language and its likely operational effect on an AI agent.
Epistemic discipline: Clearly distinguish established model mechanics from plausible interpretation or speculation.
Audience: Prefer concise, structured explanations for software project managers who are technically competent but still learning AI.
Convergence first: Do not advance or record a conclusion while either party has unresolved questions. Treat John's confirmation of convergence as the transition signal.
Shared memory: Treat this document as the semantic handshake between John and the agent.
Maintenance protocol
When the discussion produces a durable conclusion or changes the stopping point:
Draft the proposed update.
Show John the exact wording.
Obtain John's explicit approval before editing this document.
Update only the affected findings, status fields, or stopping point.
Verify the saved document after editing.
Do not update the handoff merely because a turn has ended.

Your review is unusually strong: it distinguishes **what the wording demonstrably does in the prompt** from stronger claims about latent representations, attention, or guaranteed behavior. Its central lesson is correct: a skill system needs an orientation layer, operational routing rules, and durable state—each compact enough to preserve attention for the actual task.

## Context engineering, broadly

**Context engineering** is the design, selection, organization, retrieval, and maintenance of the tokens available to a model at inference time—not merely the phrasing of a single prompt. That includes system instructions, tool contracts, skill files, retrieved documents, chat history, runtime state, and persistent notes.

The governing optimization is:

$$
\text{Maximize task-relevant signal subject to a finite attention budget.}
$$

More context is not automatically better. Large contexts can dilute salient instructions, complicate retrieval, and reduce precision on long-range dependencies; therefore the target is the **smallest sufficient set of high-signal context** for the next decision.

A helpful distinction:


| Layer | Question answered | Typical artifact |
| :-- | :-- | :-- |
| Orientation | “What kind of system is this?” | Router introduction, conceptual model |
| Policy | “What must I do or avoid?” | Explicit rules, guardrails, decision criteria |
| Procedure | “What sequence applies now?” | Flow, checklist, state machine |
| Evidence | “What facts govern this case?” | Files, docs, tool outputs, citations |
| State | “What has been decided and what remains?” | Handoff, ADR, task ledger |
| Evaluation | “Did this arrangement improve behavior?” | Test set, traces, acceptance criteria |

Your document recognizes most of these layers, particularly orientation, procedure, and state.

## Core best practices

### Start with failure modes

The best short instructions name a predictable failure and give a recovery action. Your reading of “You don’t remember every skill, so ask” is exactly that: incomplete inventory recall becomes a reason to consult a router rather than improvise. This is preferable to abstract admonitions such as “be careful” because it supplies an observable action.

A robust pattern is:

> **When** condition X occurs, **do** action Y, **because** risk Z otherwise applies.

For example: “When a request spans more than one domain, inspect the router before selecting a workflow, because locally plausible skill choices may conflict with the global process.”

### Use minimal, direct language

Good instructions are neither vague aspirations nor brittle pseudo-code. They should be specific enough to constrain behavior, but abstract enough to remain valid when tools, models, or repository structure change. Anthropic describes this as choosing the right “altitude”: clear, direct instruction without encoding a fragile implementation.

Your review correctly identifies Matt’s use of **intent over mechanism**. “Ask” works even if the router eventually changes from a static skill file to semantic retrieval, a tool call, or a registry.

### Define local vocabulary early

Terms such as *flow*, *on-ramp*, *standalone*, *layer*, *seam*, and *decision ticket* are not merely stylistic. They are a compression scheme: once terms have stable, project-local meanings, later instructions can refer to complex operational relationships in a few tokens.

Matt’s repository explicitly treats a shared language as a way to improve consistency in names and navigation, while making agent communication more concise.  Your analysis of “A flow is a path through the skills” captures the valuable part: it disambiguates a local term and establishes reusable spatial vocabulary.

The caveat you articulate is important: say that such wording **makes certain interpretations more likely**, not that it proves a particular internal geometric structure in a model.

### Separate routing from capability

A common agent-design mistake is giving a model a large capability catalogue and assuming it can reliably select the right capability. A router solves a different problem from a skill:

- A **skill** says how to perform a kind of work.
- A **router** says which kind of work applies.
- A **flow** says how several skills compose over time.
- A **cross-cutting discipline** says what vocabulary or standards should shape many kinds of work.

The public skills repository makes this distinction partly through invocation boundaries: user-invoked skills orchestrate work, while model-invoked skills hold reusable disciplines that can be reached automatically when the work fits.  That is broadly consistent with your route-versus-layer interpretation.

### Prefer progressive disclosure

Do not preload an agent with every implementation detail. Provide a compact map first, then let it retrieve authoritative detail when a branch becomes relevant. This preserves context budget and makes the system easier to update.

A practical hierarchy is:

1. **Always loaded:** identity, non-negotiable constraints, directory/index, current task state.
2. **Loaded on route selection:** the selected skill and its immediate dependencies.
3. **Loaded just in time:** source files, specifications, evidence, or tool results relevant to the next decision.
4. **Persisted outside context:** handoffs, decisions, findings, and indexes.

This aligns with the “just-in-time” retrieval pattern: maintain lightweight identifiers such as paths and links, then load detailed material at runtime as needed rather than injecting a complete corpus up front.

### Build clean tool and skill boundaries

Tools and skills should be easy to distinguish. If a human cannot state when to use Tool A rather than Tool B, an agent is unlikely to do so reliably. Clear names, unambiguous inputs, low overlap, and concise outputs reduce routing ambiguity and token waste.

Applied to skills, that means each skill should state:

- Trigger or entry condition
- Desired outcome
- Inputs it needs
- Outputs/artifacts it produces
- Authority boundaries
- Escalation or exit conditions
- Related skills it may invoke or defer to


### Use canonical examples, not rule dumps

A few diverse, representative examples often teach an operational pattern better than dozens of edge-case rules. But examples should not become a substitute for policy: pair each example with the principle it demonstrates. Anthropic specifically recommends curated canonical examples over a laundry list of special cases.

For an Ask Matt router, three or four contrastive examples would be especially useful:


| User situation | Router decision | Why |
| :-- | :-- | :-- |
| “I know what to build; turn this discussion into a spec.” | `/to-spec` | Synthesis, not discovery |
| “We have a vague product idea and conflicting terms.” | `/grill-with-docs` plus domain modeling | Alignment and language work precede implementation |
| “A production regression appeared after a release.” | Diagnosis flow | Evidence-driven debugging should precede a fix |
| “The code works but has become hard to change.” | Architecture improvement | The bottleneck is structural design, not feature delivery |

## Reusable context patterns

### Router pattern

A router turns a broad request into a small set of candidate workflows. It should expose the user-visible map, capture key discriminators, and route to an entry point rather than attempting to execute every domain procedure itself.

**Best use:** many skills, overlapping task descriptions, or a novice user who does not know the inventory.

**Failure mode:** it becomes an overgrown master prompt that restates every skill. Keep it navigational; load the selected procedure afterward.

### Flow pattern

A flow is a conditional path through skills or stages:

$$
\text{intake} \rightarrow \text{clarify} \rightarrow \text{plan} \rightarrow \text{execute} \rightarrow \text{verify}
$$

The important word is **conditional**. A useful flow specifies branches, detours, return points, and stopping conditions—not just an idealized linear sequence.

Your interpretation is strong here: “path” usefully implies movement, order, branches, and destination, while not claiming every route requires every step.

### On-ramp pattern

An on-ramp recognizes that users enter a workflow from different states. Someone may have an idea, a ticket, an incident, a partial implementation, or a failing test; forcing them all through the same first step wastes effort.

Each on-ramp should declare:

- Preconditions
- The first skill to use
- The state it produces
- Where it merges with the main flow

Your detected inconsistency—“two on-ramps” in the framing but three later bullets—is precisely the kind of integrity defect that can confuse both human users and agents. Treat numerical inventory claims as maintained interface contracts; update them with the underlying map.

### Cross-cutting discipline pattern

A cross-cutting skill should not masquerade as a mandatory stage. It supplies a vocabulary, evaluative lens, or invariant that applies wherever relevant.

Your `/domain-modeling` example is well chosen: it is not simply a glossary but an active practice for resolving terminology, testing relationships against concrete cases, checking against the codebase, and recording durable decisions. The repository similarly describes `/codebase-design` as a reusable vocabulary and discipline for deep modules and clean seams.

A good cross-cutting layer has:

- A clear trigger: “when terms conflict,” “when module boundaries change”
- Concrete actions: challenge, test, update, record
- Artifacts: glossary, `CONTEXT.md`, ADRs
- Explicit integration points: which workflow skills use it


### State ledger / handoff pattern

Long tasks need durable state outside the live chat. A handoff should preserve only what a future agent needs to continue safely:

- Objective and scope
- Current state / stopping point
- Decisions and their rationale
- Open questions
- Constraints and non-goals
- Sources of truth and paths
- Next action
- What must not be assumed

This corresponds closely to your “semantic handshake” language. Structured notes are a well-supported approach for long-horizon work because they preserve progress and dependencies without retaining the entire conversational trace.

The distinction you make between YAML for predictable state fields and Markdown for nuanced reasoning is sensible. Just avoid claiming intrinsic model-performance gains without an evaluation.

### Decision record pattern

An ADR, decision log, or compact ledger prevents the same dispute from reappearing after compaction or a session restart.

A minimal entry:

```yaml
decision: Use domain term "materialization cascade"
status: accepted
why: Distinguishes file-system realization from generic creation
evidence: Discussion on 2026-07-29; validated against code paths X and Y
consequences: Update CONTEXT.md, names, and future specs
revisit_when: The lifecycle model changes
```

The key is not the format; it is explicit provenance and a trigger for reconsideration.

### Compaction pattern

Compaction is the controlled conversion of a long, noisy history into a shorter, high-fidelity continuation context. Preserve decisions, unresolved issues, architectural constraints, and the current operational state; discard repetitive discussion and raw tool output once it has been absorbed.

Your handoff is more detailed than a standard compaction note because it is also an analytical record. That can be appropriate for a multi-session close reading, but it should be partitioned so the next agent can load the **operational core** before the extensive historical analysis.

### Isolated research pattern

For broad research, use a focused subagent or separate process with a clean task context; return a concise, cited result to the coordinating agent. This prevents raw exploration material from swamping the main thread. Subagent separation is particularly useful when the primary agent must preserve a plan while multiple investigations produce large temporary contexts.

## Correlation with your review

Your document already applies most of the high-value patterns correctly.


| What you learned | Context-engineering principle | Assessment |
| :-- | :-- | :-- |
| “You don’t remember every skill, so ask.” | Failure mode plus recovery action | **Strong.** It creates a compact routing heuristic and avoids dependence on full recall. |
| “A flow is a path through the skills.” | Early local definition and coherent metaphor | **Strong.** It establishes shared vocabulary that later route language can reuse. |
| “Everything else is standalone, or a vocabulary layer…” | Taxonomy and cross-cutting discipline | **Strong.** It distinguishes sequence from reusable evaluative language. |
| Main flow and on-ramps | Conditional workflow design | **Promising, but needs inventory consistency.** The “two” versus “three” mismatch should be corrected. |
| `/domain-modeling` as active layer | Cross-cutting practice plus persistent artifacts | **Strong.** You correctly reject the weaker “passive glossary” interpretation. |
| Handoff record | Structured external memory | **Strong.** It preserves role, task status, sources, findings, and stopping point. |
| “Read review document and installed runtime SKILL.md” | Authority ordering and source-of-truth discipline | **Strong.** It reduces drift from analyzing stale or remembered content. |
| “Await John’s selection; do not advance” | Scope control and human-in-the-loop gate | **Strong for a tutorial/review workflow.** It prevents the agent from consuming unapproved scope. |
| “Get approval before editing handoff” | Controlled state mutation | **Strong.** It protects durable memory from accidental or premature revision. |
| Claims about model influence | Epistemic calibration | **Excellent.** You distinguish likely steering effects from unverified mechanistic claims. |

## Your strongest insight

The most important insight in your review is this:

> **Compression is valuable only when the compressed phrase is later redeemed by concrete operational rules.**

“Flow,” “on-ramp,” and “underneath” are effective because the rest of the system specifies actual skills, routes, and cross-cutting practices. Without that follow-through, the prose would be memorable but operationally ambiguous.

This resembles software abstraction. A clean interface is not valuable merely because it is short; it is valuable because its implementation preserves the contract. Likewise, a compact context statement earns its brevity by reliably expanding into consistent behavior when needed.

## Improvements to the handoff

Your handoff is high quality, but it currently combines two artifacts:

1. A **durable operational state record** for the next session.
2. A **full analytical essay** documenting all prior conclusions.

That is excellent for auditability but suboptimal for first-load attention. Separate them.

### Recommended structure

**`ask-matt-review-state.yaml`** — small, always load

```yaml
persona: Senior technical writer specializing in AI training for software project managers
status: awaiting_next_passage
completed:
  - Ask Matt introduction
next_section: "## The main flow: idea → ship"
next_action: Await John's selected passage; do not advance independently
authoritative_sources:
  - john-notes/ask-matt-skill-review.md
  - C:\Users\johnp\.claude\plugins\cache\mattpocock\mattpocock-skills\1.2.0\skills\engineering\ask-matt\SKILL.md
protocol:
  - Analyze only a passage selected by John
  - Distinguish evidence, inference, and speculation
  - Do not edit durable notes without proposed wording and explicit approval
known_issue:
  - Opening says two on-ramps; later section lists three
```

**`ask-matt-skill-review.md`** — detailed findings, loaded when analysis resumes.

This is the same progressive-disclosure logic that the router embodies: start with the map and state, then retrieve the complete record when the selected passage requires it.

## Evaluation criteria

To move from plausible theory to demonstrated improvement, test the skill system against a representative request set. Context engineering should be evaluated as a behavioral intervention, not only judged by prose quality.

Measure:

- **Route accuracy:** Did the router select the appropriate entry point?
- **Clarification efficiency:** Did it ask only questions that materially changed routing?
- **Policy adherence:** Did it honor “do not advance,” approval gates, and source authority?
- **Artifact quality:** Did it create correct, usable state and decision records?
- **Recovery behavior:** Did it consult the router or source artifact rather than fabricate?
- **Context efficiency:** Did it retrieve the necessary material without loading irrelevant detail?
- **Cross-session continuity:** Did a fresh agent resume at the correct stopping point?
- **Consistency:** Did it maintain counts, terminology, and skill relationships across docs?

For your specific Ask Matt study, the simplest evaluation would use 15–25 realistic prompts, label the intended route beforehand, and compare: **no router**, **router introduction only**, and **full router plus selected skill**. The test would validate whether the elegant orientation language actually improves route selection and scope discipline rather than merely sounding coherent.
<span style="display:none">[^1_1][^1_10][^1_11][^1_12][^1_13][^1_14][^1_15][^1_2][^1_3][^1_4][^1_5][^1_6][^1_7][^1_8][^1_9]</span>

<div align="center">⁂</div>

[^1_1]: https://www.reddit.com/r/AIPractitioner/comments/1oo1cxz/context_engineering_why_ai_agents_need_more_than/

[^1_2]: https://www.linkedin.com/posts/giannidalerta_effective-context-engineering-for-ai-agents-activity-7379659066991603712-4fmy

[^1_3]: https://developers.openai.com/cookbook/examples/agents_sdk/context_personalization

[^1_4]: https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents

[^1_5]: https://github.com/mattpocock/skills

[^1_6]: https://www.promptingguide.ai/guides/context-engineering-guide

[^1_7]: https://fractal.ai/blog/context-engineering-openai/

[^1_8]: https://github.com/mattpocock/skills/blob/main/skills/productivity/grill-me/SKILL.md

[^1_9]: https://www.facebook.com/groups/chatgptpromptsandaitools/posts/1428536938654072/

[^1_10]: https://www.linkedin.com/posts/richmondalake_effective-context-engineering-for-ai-agents-activity-7386881837643603968-R-6K

[^1_11]: https://github.com/mattpocock/skills/blob/main/skills/productivity/handoff/SKILL.md

[^1_12]: https://community.openai.com/t/best-practices-for-cost-efficient-high-quality-context-management-in-long-ai-chats/1373996

[^1_13]: https://www.youtube.com/watch?v=M6mYodf0dJM

[^1_14]: https://www.langchain.com/blog/context-engineering-for-agents

[^1_15]: https://news.ycombinator.com/item?id=45418251


---

# That was a great summary. Now I need a quick primer on context engineering patterns and best practices. Give me a table of best practices and definitions, then include a path explaining which elements belong where when constructing a solution using these patterns.

Context engineering is the discipline of assembling the **right information, instructions, tools, and state for the next model decision**. Prompt wording matters, but it is only one layer; production quality comes from selecting, structuring, retrieving, compressing, and evaluating context deliberately.[^2_1]

## Definitions and practices

| Pattern / best practice | Definition | Why it matters | Put it here |
| :-- | :-- | :-- | :-- |
| **System contract** | Stable rules defining role, authority, safety boundaries, output standards, and non-negotiable behavior | Prevents the agent from reinterpreting core operating rules task by task | System prompt / root instructions |
| **Local vocabulary** | Short definitions for project-specific terms such as *flow*, *ADR*, *trade lifecycle*, or *risk limit* | Compresses recurring concepts and prevents semantic drift | Project guide or domain glossary |
| **Router** | A decision layer that selects the appropriate workflow, tool, or skill for a request | Avoids asking the model to recall and choose from a large capability inventory unaided | Entry-point skill or orchestration prompt |
| **Flow** | A conditional path through stages or skills, with branches and exit conditions | Makes multi-step work repeatable without forcing every task through the same sequence | Workflow/skill documentation |
| **On-ramp** | An alternate entry point for users who begin from different states, such as an idea, bug, spec, or partial implementation | Reduces unnecessary steps and routes work from its actual starting condition | Router and flow documentation |
| **Cross-cutting discipline** | A reusable lens that applies across many flows, such as domain modeling, security review, or codebase design | Preserves consistent standards without turning the discipline into a mandatory serial step | Separate skill, project guide, or policy layer |
| **Selective retrieval** | Retrieve only the source material relevant to the current decision, rather than dumping all available documentation into context | Reduces distraction, token cost, and failures caused by irrelevant evidence | Retrieval pipeline / tool layer |
| **Grounding contract** | An explicit rule for what evidence the agent may rely on and what it must do when evidence is absent | Limits unsupported claims and makes uncertainty operational | Task prompt plus answer-format rules |
| **Canonical examples** | A small, curated set of representative input-output examples | Shows desired structure and judgment more reliably than abstract rules alone | Skill docs, example library, few-shot slot |
| **Structured state** | Compact external record of goals, decisions, open questions, current position, and next action | Enables reliable resumption across long tasks and session restarts | Handoff file, task ledger, database |
| **Decision record** | Durable note of a material decision, rationale, evidence, consequences, and review trigger | Stops the agent or team from reopening settled questions without new evidence | ADRs, decision log, issue tracker |
| **Validation gate** | Test, check, review, or acceptance criterion required before the workflow advances | Converts “do good work” into observable completion conditions | Flow steps, CI, test plan, review checklist |
| **Compaction** | Replacing long conversational history with a concise, faithful operational summary | Preserves essential state while protecting the context window from clutter | Handoff or rolling session summary |
| **Context isolation** | Giving a research or subtask agent a focused context and returning only a concise result | Keeps exploratory work from contaminating the main task context | Subagents, separate sessions, research tasks |
| **Observability and evaluation** | Logging assembled context and testing behavior against known cases | Lets you prove whether a context change improved routing, accuracy, cost, or adherence | Runtime instrumentation and eval suite |

A useful default is: **stable rules first, task-specific evidence last**. Keep stable material cacheable and unchanged where possible; inject retrieved sources, current state, tool outputs, and the user’s request only when they are needed.[^2_1]

## Construction path

Build the solution in this order. Each layer answers a different question and should live in a different artifact.

```text
1. Mission and boundaries
          ↓
2. Shared language and domain model
          ↓
3. Capability map and router
          ↓
4. Task-specific flow
          ↓
5. Evidence retrieval and tools
          ↓
6. Execution, validation, and state updates
          ↓
7. Compaction, measurement, and improvement
```


### 1. Mission and boundaries

Start with the **system contract**: what the agent is for, what it may do, what it must not do, when it should ask, and the required quality bar.

Put here:

- Persona or operating role
- Authority boundaries
- Privacy, security, and compliance constraints
- Output requirements
- Escalation rules
- Stable tool-use policies

For a quant-research agent, this might include: “Do not place orders; distinguish facts from inferences; cite market data; flag stale data; never represent a model estimate as a confirmed result.”

### 2. Shared language

Next, establish the vocabulary the system will reuse across workflows. This is where terms become local contracts rather than loose English labels.

Put here:

- Domain glossary
- Naming conventions
- Key entities and relationships
- Definitions of artifact types
- Invariants and non-negotiable distinctions

For example, in a trading-research system, define the difference between a **signal**, **hypothesis**, **backtest**, **paper-trading result**, **live result**, **execution assumption**, and **validated edge**. That prevents the agent from treating an attractive backtest as evidence of deployable alpha.

### 3. Capability map

Then create the **router**. Its job is not to do every kind of work; its job is to determine what kind of work this is and route the request appropriately.

Put here:

- Available skills or workflows
- Entry criteria for each
- Clarifying questions that change routing
- On-ramps for common starting states
- Cross-cutting disciplines that may apply

A compact router decision might look like:

```text
If the request needs factual investigation → research flow
If it needs a decision from existing evidence → analysis flow
If it needs implementation → plan/build/test flow
If terminology or entities are ambiguous → invoke domain modeling
If it is a long-running task → load the state ledger first
```

Your “Ask Matt” reading fits here: the router exists because comprehensive recall of the capability map is unreliable, so the agent should consult the map rather than confidently improvising one.

### 4. Task-specific flow

Once the request is routed, load only the selected workflow. A flow should specify the sequence, branches, artifacts, and completion gates.

Put here:

- Preconditions
- Inputs
- Ordered or conditional stages
- Branches and detours
- Required deliverables
- Validation gates
- Exit criteria
- Handoff requirements

A research flow might be:

```text
Question
→ Define claim and decision use
→ Retrieve primary evidence
→ Extract facts versus interpretation
→ Analyze and quantify uncertainty
→ Produce cited memo
→ Red-team the conclusion
→ Record decision-relevant findings
```

Do not force cross-cutting practices into a numbered flow step unless they truly apply every time. Domain modeling, codebase design, and security review are usually invoked **when triggered**, not mechanically at every stage.

### 5. Evidence and tools

Only after the flow is selected should you retrieve documents, query databases, inspect code, or call APIs. The model needs **relevant evidence**, not a warehouse dump.

Put here:

- Ranked document excerpts
- File paths and authoritative sources
- Tool schemas
- Database query outputs
- Current market or runtime data
- Relevant examples

Use a grounding rule such as:

> “Use the supplied primary sources for factual claims. If the evidence does not resolve the question, state the gap, identify what would resolve it, and do not infer certainty.”

Selective retrieval, source identifiers, and explicit “unknown” behavior are central defenses against unsupported answers.[^2_1]

### 6. Execution and validation

The agent now executes the chosen task, but it should not define success for itself. The flow should provide observable validation.

Put here:

- Unit/integration tests
- Linting and type checks
- Source-citation requirements
- Reconciliation checks
- Review criteria
- Acceptance thresholds
- Human approval gates where stakes warrant them

For a trading workflow, validation might include: no look-ahead bias, survivorship-bias checks, realistic transaction costs, clearly defined universe, out-of-sample evaluation, and a record of parameter selection.

### 7. State and continuous improvement

At the end of a meaningful unit of work, preserve what a later agent needs—not a full transcript.

Put in the **handoff/state record**:

- Current objective
- Completed work
- Open questions
- Settled decisions and rationale
- Source-of-truth paths
- Current stopping point
- The single next action
- Any permission or approval boundary

Then measure the system. Context changes should be treated like changes to a trading model or software system: formulate the hypothesis, test against a held-out set of representative tasks, inspect failure cases, and retain the change only if it improves outcomes. Logging token usage, selected context, route choice, tool calls, and validation results makes that process possible.[^2_1]

## Context placement map

| Artifact / layer | Should contain | Should not contain |
| :-- | :-- | :-- |
| **System prompt** | Stable authority, safety, role, universal quality rules | Project-specific details that change frequently; large reference documents |
| **Project guide** | Architecture, conventions, glossary, durable project constraints | Current task status or raw research output |
| **Router** | Skill map, entry criteria, route-selection logic | Full procedures for every skill |
| **Skill / workflow file** | Steps, branches, inputs, outputs, validation, escalation | A complete unrelated skill inventory |
| **Retrieval layer** | Authoritative evidence selected for the present question | Unfiltered document collections |
| **Examples library** | Small, representative exemplars of desired work | Many near-duplicate examples or outdated patterns |
| **Task brief** | Current objective, scope, constraints, acceptance criteria | Long-term global policy |
| **Tool result** | Fresh facts needed for the immediate next decision | Durable policy or unvalidated interpretation |
| **Handoff / state ledger** | Decisions, unresolved items, stopping point, next action | Full conversational transcript |
| **ADR / decision log** | Durable architectural or domain decision and rationale | Ephemeral implementation chatter |
| **Evaluation suite** | Representative cases, expected behavior, metrics | Production secrets or mutable live context |

## A practical rule

When deciding where an element belongs, ask:

1. **Is it always true?** Put it in the system contract.
2. **Is it stable for this project?** Put it in the project guide or glossary.
3. **Does it choose a workflow?** Put it in the router.
4. **Does it define how one workflow proceeds?** Put it in that skill or flow.
5. **Is it evidence for this one task?** Retrieve it just in time.
6. **Must it survive a restart?** Put it in structured state or a decision record.
7. **Does it prove success?** Put it in validation or evaluation.

That division gives you a system that is compact at the top, evidence-rich at the point of action, and durable across sessions.
<span style="display:none">[^2_10][^2_2][^2_3][^2_4][^2_5][^2_6][^2_7][^2_8][^2_9]</span>

<div align="center">⁂</div>

[^2_1]: https://sureprompts.com/blog/context-engineering-best-practices-2026

[^2_2]: https://github.com/coleam00/context-engineering-intro

[^2_3]: https://www.datacamp.com/blog/context-engineering

[^2_4]: https://www.faros.ai/blog/context-engineering-for-developers

[^2_5]: https://www.promptingguide.ai/guides/context-engineering-guide

[^2_6]: https://redis.io/blog/context-engineering-best-practices-for-an-emerging-discipline/

[^2_7]: https://claude.ai/public/artifacts/f498a4cc-4c45-481c-a6dd-8e1d196dadb0

[^2_8]: https://sourcegraph.com/blog/context-engineering

[^2_9]: https://www.elastic.co/search-labs/blog/context-engineering-overview

[^2_10]: https://www.taskade.com/blog/context-engineering


---

# What is your source for: Definitions and practices? What are good books, docs, pdfs, or youtubes that I should review? Mark which ones are free. Do no suggest a youtube or a pdf until you confirm its existence by search or access.

The **“Definitions and practices”** table was a synthesis, not a single-source taxonomy. Its strongest sources were Anthropic’s agent-context engineering guidance, the context-management literature and practitioner documentation, and Matt Pocock’s skills repository as a concrete example of routing, flows, handoffs, and cross-cutting skills.[^3_1][^3_2][^3_3]

I should have labeled the table more precisely as a **practitioner framework synthesized from these sources**, rather than implying it was a verbatim canonical list.

## Source mapping

| Item in my table | Main basis |
| :-- | :-- |
| System contract, tool boundaries, selective retrieval, context isolation, compaction | Anthropic’s *Effective Context Engineering for AI Agents*, which frames context as finite and emphasizes curated context, tools, retrieval, compression, and subagents. [^3_1] |
| Local vocabulary, router, flows, on-ramps, cross-cutting disciplines, handoffs | Matt Pocock’s skills repository and the Ask Matt material you supplied; these labels are especially specific to that skill-system architecture. The repository includes routers, skills, and a handoff skill. [^3_3][^3_4] |
| Structured state, memory, retrieval, tool selection, context-window optimization | LlamaIndex’s overview of context engineering for agents. [^3_2] |
| Validation gates, examples, documentation, project rules | The Context Engineering Intro repository, which uses project rules, examples, planning artifacts, documentation, validation, and execution loops for coding agents. [^3_5] |
| Instrumentation, context budgets, stable-vs-dynamic ordering, cache-aware design | The practitioner guidance I cited from SurePrompts. These are useful engineering heuristics, but I would treat them as implementation advice rather than foundational doctrine. [^3_6] |
| Formal academic framing of context adaptation | The ACE paper defines context adaptation/context engineering as improving behavior through construction or modification of inputs rather than changing model weights. [^3_7] |

## Recommended reading

### Start here

| Resource | Format | Free? | Why review it |
| :-- | :-- | --: | :-- |
| **Anthropic — “Effective Context Engineering for AI Agents”** | Engineering article | **Yes** | Best practical starting point for finite-context thinking, tool design, retrieval, memory, compaction, and subagent separation. [^3_1] |
| **LlamaIndex — “Context Engineering: What It Is and Techniques to Consider”** | Technical article | **Yes** | Clear agent-centric treatment of context composition, memory, tools, and context-window management. [^3_2] |
| **Matt Pocock — `skills` repository** | GitHub repository | **Yes** | The closest concrete reference for the router/flow/standalone/cross-cutting-skill model you are studying. Read the router, handoff, and representative workflow skills as an integrated system. [^3_3] |
| **OpenAI Cookbook — Context Engineering for Personalization** | Developer documentation / cookbook | **Yes** | Useful for thinking about what should be stored, retrieved, and injected into an active context for a particular user or task. [^3_8] |
| **“A Survey of Context Engineering for Large Language Models”** | Academic paper | **Yes** | Best broad research survey to establish vocabulary, categories, and open research questions before treating any vendor framework as universal. [^3_9] |

## Papers and PDFs

All of these are confirmed to exist and are freely accessible.


| Resource | Format | Free? | Best use |
| :-- | :-- | --: | :-- |
| **Mei et al., “A Survey of Context Engineering for Large Language Models”** | arXiv paper; PDF available | **Yes** | Academic overview of the field’s methods and taxonomy. Read this first if you want an intellectual map rather than vendor guidance. [^3_9] |
| **Zhang et al., “Agentic Context Engineering: Evolving Contexts for Agentic Systems”** | arXiv PDF | **Yes** | Relevant to your interest in persistent, improving playbooks: it treats context as an evolving artifact generated, reflected upon, and curated over time. [^3_7] |
| **Paulsen et al., “The Maximum Effective Context Window for Real World…”** | arXiv PDF | **Yes** | Useful corrective to “more context is always better.” It studies the idea that additional context can eventually degrade output quality for a task. [^3_10] |
| **Xu et al., “Agentic File System Abstraction for Context Engineering”** | arXiv PDF | **Yes** | Relevant if you are thinking of files, indexes, and artifact stores as the substrate through which agents build and access working context. [^3_11] |

I would read these papers selectively: abstract, introduction, architecture/method, experiments, and limitations. For your purposes, do not begin by trying to absorb every technical detail.

## Books

The term is still new enough that books are thinner and more vendor- or framework-specific than the articles and papers above.


| Book | Free? | Why consider it |
| :-- | --: | :-- |
| **Boni García, *Context Engineering***, Manning | **No** for the full book; publisher previews may be available | A dedicated hands-on book covering the broader context-engineering stack and tools including DSPy, LangChain, CrewAI, and LlamaIndex. [^3_12] |
| ** *Context Engineering for Multi-Agent Systems: Move Beyond Prompt Engineering…* ** | **No** | Consider only after the fundamentals if your focus shifts toward semantic blueprints and multi-agent orchestration rather than a single-agent skill system. [^3_13] |

For your current project, I would not start with either book. The freely available Anthropic article, survey paper, and Matt Pocock repository will teach more directly relevant material faster.

## Confirmed videos

These are confirmed YouTube resources. I would prioritize the first two; the others are supplementary.


| Video | Free? | Why watch it |
| :-- | --: | :-- |
| **Lance Martin / LangChain — “Context Engineering for Agents”** | **Yes** | A practitioner discussion focused on giving an LM the right context for the next step, with examples of structured research workflows and decomposition. [^3_14] |
| **Anthropic meetup — “Effective Context Engineering for AI Agents”** | **Yes** | A direct video discussion of the same practical agent-context framing that appears in Anthropic’s written guidance. [^3_15] |
| **Dave Ebbelaar — “Effective Context Engineering for AI Agents”** | **Yes** | An accessible technical walkthrough covering context components, routers, tool boundaries, structured sections, examples, and state management. [^3_16] |
| **“Context Engineering in 29 Minutes: Complete Course”** | **Yes** | A quick overview of collecting, selecting, compressing, arranging, and assembling context, including stable-first and dynamic-last prompt construction. [^3_17] |
| **Andrej Karpathy — “Software Is Changing (Again)”** | **Yes** | Broader perspective on the changing software-development workflow; useful context, but it is not a dedicated context-engineering course. [^3_18] |

## Suggested sequence

For your Ask Matt study and your likely use case—building reliable, reusable agent workflows—I would use this sequence:

1. **Anthropic article**: establish the practical mental model of context as a finite operating resource.[^3_1]
2. **Matt Pocock repository plus your installed `ask-matt` artifact**: examine a real skill architecture—routing, workflows, vocabulary layers, and handoff state.[^3_3][^3_4]
3. **Lance Martin video**: reinforce the “right context for the next step” framing in agent workflows.[^3_14]
4. **Survey paper**: broaden from one implementation style into the research taxonomy.[^3_9]
5. **ACE paper**: study evolving playbooks, reflection, and curated persistent context—the closest paper-level analogue to your handoff and settled-findings approach.[^3_7]
6. **Maximum Effective Context Window paper**: pressure-test the temptation to keep adding documentation and history to a session.[^3_10]
7. **Manning book**, only if you want framework-specific implementation depth across the current tooling ecosystem.[^3_12]
<span style="display:none">[^3_19][^3_20][^3_21][^3_22][^3_23][^3_24][^3_25][^3_26][^3_27][^3_28][^3_29][^3_30][^3_31][^3_32]</span>

<div align="center">⁂</div>

[^3_1]: https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents

[^3_2]: https://www.llamaindex.ai/blog/context-engineering-what-it-is-and-techniques-to-consider

[^3_3]: https://github.com/mattpocock/skills

[^3_4]: https://github.com/mattpocock/skills/blob/main/skills/productivity/handoff/SKILL.md

[^3_5]: https://github.com/coleam00/context-engineering-intro

[^3_6]: https://sureprompts.com/blog/context-engineering-best-practices-2026

[^3_7]: https://arxiv.org/pdf/2510.04618.pdf

[^3_8]: https://developers.openai.com/cookbook/examples/agents_sdk/context_personalization

[^3_9]: https://arxiv.org/abs/2507.13334

[^3_10]: https://arxiv.org/pdf/2509.21361.pdf

[^3_11]: https://arxiv.org/pdf/2512.05470.pdf

[^3_12]: https://www.manning.com/books/context-engineering

[^3_13]: https://www.amazon.com/Context-Engineering-Multi-Agent-Systems-architecture/dp/1806690055

[^3_14]: https://www.youtube.com/watch?v=_IlTcWciEC4

[^3_15]: https://www.youtube.com/watch?v=U4r2tsMfZ4Q

[^3_16]: https://www.youtube.com/watch?v=nkJXADeI62c

[^3_17]: https://www.youtube.com/watch?v=-h9VVJIqtvA

[^3_18]: https://www.youtube.com/watch?v=LCEmiRjPEtQ

[^3_19]: https://www.reddit.com/r/LangChain/comments/1oqedqq/what_are_the_best_learning_resources_on_context/

[^3_20]: https://www.reddit.com/r/ContextEngineering/comments/1opug52/what_are_the_best_learning_resources_on_context/

[^3_21]: https://www.reddit.com/r/LLMDevs/comments/1lo7a4t/context_engineering_a_practical_firstprinciples/

[^3_22]: https://www.promptingguide.ai/guides/context-engineering-guide

[^3_23]: https://packmind.com/context-engineering-ai-coding/context-engineering-best-practices/

[^3_24]: https://eugeneasahara.com/2025/10/17/context-engineering-and-my-two-books/

[^3_25]: https://www.youtube.com/watch?v=Egeuql3Lrzg\&vl=en

[^3_26]: https://www.inngest.com/blog/context-engineering-in-practice

[^3_27]: https://www.youtube.com/watch?v=96jN2OCOfLs

[^3_28]: https://www.youtube.com/watch?v=uhJJgc-0iTQ

[^3_29]: https://www.youtube.com/watch?v=lcpWbg0jiYU

[^3_30]: https://www.youtube.com/watch?v=mR-WAvEPRwE

[^3_31]: https://www.youtube.com/watch?v=yfeHoOkn2TI

[^3_32]: https://www.youtube.com/watch?v=7xTGNNLPyMI

