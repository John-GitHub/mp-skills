# Ask Matt Skill Review

Matt opens the skill with two compact statements:

> You don't remember every skill, so ask.
>
> A flow is a path through the skills.

Together, they explain why the router exists and provide the mental model it will use.

## “You don't remember every skill, so ask.”

### A limitation followed by a response

The sentence contains two clauses joined by *so*:

> **You don't remember every skill** (limitation), **so ask** (response).

Expanded, it means:

> Because you cannot reliably recall every available skill, consult the router.

The first clause identifies a predictable failure. The second gives the corrective behavior. This produces a compact decision rule:

> **incomplete recall → consultation**

The word *every* matters. Matt is not claiming that the reader remembers nothing. He is saying that complete recall is unreliable—and complete recall is what choosing from the full skill set would require.

### One sentence, two audiences

The explicit *you* makes the sentence sound like advice to the user. Inside a `SKILL.md`, however, the immediate operational audience is the agent.

The sentence therefore supports two compatible readings:

- **For the agent:** Do not improvise a complete skill map from uncertain recall; use this router.
- **For the user:** You do not need to memorize the available skills; ask the router where to begin.

The command *ask* is an imperative whose subject is an implied *you*. The heading, “Ask Matt,” supplies what the short command leaves unstated: whom to ask and why.

This dual audience is useful because a skill file is both executable context for an agent and readable documentation for people who inspect it.

### “Remember” as deliberate compression

*Remember* is ordinary human language, not a precise description of model operation. A language model does not remember an installed skill inventory as a person remembers a list. Its behavior depends on which skill descriptions and instructions the surrounding system makes available in the current context.

A technically exact version would be longer:

> You cannot reliably identify every relevant user-invoked skill from the currently available context.

Matt's version preserves the operational point while removing implementation detail. The important invariant is simple: neither the user nor the agent should depend on unaided, comprehensive recall.

### Why it is effective context engineering

Context engineering is the deliberate design of information and instructions placed before a model so that useful behavior becomes more likely.

This sentence follows several useful practices:

1. **Name the failure mode.** The agent may overlook a relevant skill.
2. **Provide the recovery behavior.** Consult the router instead of guessing.
3. **Connect them explicitly.** *So* turns an observation into a reason for action.
4. **State the intent, not the implementation.** The instruction can remain useful even if the skill-loading mechanism changes.
5. **Use familiar language.** A short human explanation consumes less context than an account of retrieval and invocation architecture.

The sentence may bias the model away from confident improvisation and toward consultative behavior. That is a plausible effect of the wording, not a guarantee about how every model will respond.

## “A flow is a path through the skills.”

### Linguistic structure

The sentence has a simple copular structure:

> **A flow** (subject) **is** (copula) **a path through the skills** (subject complement).

A **copula** is a linking verb—usually a form of *be*—that connects a subject to an identity, category, or description. Here, *is* connects the term being defined, *flow*, to its defining description, *a path through the skills*.

This makes the sentence a **copular definition**:

> term being defined + *is* + defining description

In formal terminology, *a flow* is the **definiendum**: the term to be explained. *A path through the skills* is the **definiens**: the expression that explains it.

The definition is local rather than universal. Matt is effectively saying:

> In this skill system, understand a “flow” as a path through the available skills.

### Why the definition is effective

English commonly uses `X is Y` to establish a concept directly. The structure presents `X` as the topic and gives `Y` as the model for understanding it.

Matt maps the abstract idea of a *flow* onto the familiar spatial idea of a *path*. That metaphor suggests order, movement, intermediate points, and destination without requiring him to state each idea separately.

The sentence compresses a larger explanation:

> Skills may be used in a meaningful sequence, and that sequence can be treated as a route.

The rest of the skill can now reuse the same spatial vocabulary: *main flow*, *on-ramp*, *route*, *merge*, and *underneath*. One short definition establishes the shared conceptual system.

### Likely effect on a language model

A language model does not store the sentence as a formal symbolic equation. During inference, the surrounding tokens shape one another's contextual representations.

The construction associates *flow* with *path* and anchors both to *skills*. This reduces ambiguity: *flow* becomes more likely to mean an ordered route through a workflow than fluid movement, cash flow, or a psychological state.

It also makes later route-related language more probable. Terms such as *step*, *on-ramp*, *destination*, and *merge* fit the conceptual frame the definition has established.

It is reasonable to say that the sentence biases later predictions toward a path-based interpretation. It would be too strong to claim that we know its exact geometry in the model's latent space or that attention weights alone explain the model's understanding.

## “Everything else is standalone, or a vocabulary layer that runs underneath.”

### Classifying the remainder

This is another copular classification:

> **Everything else** (subject) **is** (copula) **standalone, or a vocabulary layer that runs underneath** (alternative subject complements).

*Everything else* refers back to the categories Matt has already introduced: the main flow and its on-ramps. It gathers the remaining skills into a single remainder set.

The sentence therefore completes a coarse opening map:

> **principal routes → main flow and on-ramps**
>
> **remainder → standalone skills and cross-cutting vocabulary**

The word *everything* presents this classification as exhaustive. In practice, the later document introduces finer headings such as codebase health and crossing sessions. The opening is therefore rhetorically exhaustive but operationally coarse: it gives the reader enough structure to become oriented without presenting the complete inventory.

### “Standalone” as workflow independence

*Standalone* compresses a routing rule:

> This skill can be reached for independently; it is not a mandatory stage in a sequential workflow.

It describes how a skill participates in the workflow, not its formal invocation policy. A standalone skill may still be user-invoked or model-invoked.

Nor does *standalone* mean that the skill is atomic, stateless, isolated, or dependency-free:

- `/prototype` is independently reachable but can also serve as a detour within the main flow.
- `/teach` is standalone while maintaining a stateful workspace across sessions.
- `/research` is standalone, but the artifact it produces can feed `/grill-with-docs`.

The useful distinction is between **independent entry** and **required sequence**, not between isolated and interconnected implementation.

### “A vocabulary layer that runs underneath”

This phrase distinguishes a cross-cutting discipline from a position in a workflow.

A flow organizes skills into a sequence of conditional steps:

> first → next → branch or detour → destination

A layer can inform several of those steps without occupying a numbered position of its own.

The relative clause *that runs underneath* extends the spatial metaphor. *Underneath* presents the vocabulary as foundational and supporting; *runs* suggests continued applicability across the work above it.

This does not mean that the vocabulary skill executes continuously in the background or becomes globally available through the model's attention mechanism. A more grounded paraphrase is:

> These disciplines can shape several workflow skills without becoming sequential stages in the flow.

### The concrete vocabulary layers

The repository identifies two such layers: `/domain-modeling` and `/codebase-design`.

`/domain-modeling` is not a passive glossary. It is an active discipline that:

- challenges vague or conflicting terminology;
- tests domain relationships with concrete scenarios;
- checks claims against the code;
- updates `CONTEXT.md` when terms are resolved;
- records qualifying decisions as ADRs.

Other workflow skills use this discipline when domain language must remain coherent. `/grill-with-docs`, `/triage`, and `/wayfinder` explicitly invoke or work with `/domain-modeling`.

`/codebase-design` supplies a different vocabulary: *module*, *interface*, *depth*, *seam*, *adapter*, *leverage*, and *locality*. It establishes the terms and principles used to reason about deep, testable modules. `/tdd` and `/improve-codebase-architecture` speak through that vocabulary.

The phrase *runs underneath* therefore compresses an actual repository relationship:

> **not a step every workflow must visit**
>
> **a shared discipline that multiple steps can invoke or use**

The metaphor succeeds because later skill definitions make the cross-cutting relationship concrete.

### Likely context-engineering effect

Before presenting detailed routing rules, the sentence gives the model familiar categories for interpreting the remaining skill descriptions.

A coarse routing heuristic becomes available:

> Is this skill part of a route?
>
> - If yes, locate it in the main flow or an on-ramp.
> - If no, consider whether it is independently reachable or supports several steps through shared vocabulary.

Later uses of *standalone*, *vocabulary*, and *underneath* reinforce that distinction. This can make a workflow-versus-cross-cutting interpretation more likely in the current context.

The sentence does not prove that the model constructs a particular latent-space topology, configures global attention, suppresses hallucinated dependencies, or substantially reduces prediction entropy. Those would be hypotheses to test through evaluation.

Its defensible contribution is simpler: it supplies a compact conceptual classification that later routing instructions can refine.

## Ask Matt Introduction Summary

The introduction establishes an orientation model before presenting the detailed skill inventory. In three short statements, Matt explains why the router exists, defines the metaphor used to navigate it, and classifies the skills by their relationship to the workflow.

### The model established

| Question resolved | Matt's language | Context established | Routing consequence |
|---|---|---|---|
| Why does the router exist? | “You don't remember every skill, so ask.” | Complete recall is unreliable; consultation is the recovery behavior. | Use the router instead of improvising the skill inventory. |
| What is a flow? | “A flow is a path through the skills.” | Skill usage can be understood spatially as movement along a route. | Locate the user's situation on a path through the available skills. |
| How do skills participate in the system? | Main flow, on-ramps, standalone skills, and vocabulary underneath | Some skills form routes, some provide independent entry points, and others support several routes through shared language. | Recommend a route, a standalone skill, or a cross-cutting vocabulary discipline. |

Reduced to an operational sequence, the model is:

```text
1. Consult the router.
2. Locate the situation on the map.
3. Recommend a route, an independent entry point, or a supporting discipline.
```

This is an orientation layer rather than a complete routing specification:

```text
Introduction: How should this skill system be understood?
Later sections: Which skill or sequence fits this particular situation?
```

### A concrete example: `/domain-modeling`

`/domain-modeling` demonstrates what Matt means by a vocabulary layer that “runs underneath.”

It is not a numbered step in the main flow. It is an active, cross-cutting discipline that several workflow skills invoke when domain language must be challenged, clarified, checked against the code, or recorded in `CONTEXT.md` and ADRs.

This makes the spatial metaphor concrete:

> `/domain-modeling` supports several routes without occupying a fixed position on any one route.

Its companion layer, `/codebase-design`, performs the same structural role for architectural language: it supplies the exact vocabulary and principles used by skills such as `/tdd` and `/improve-codebase-architecture`.

### Context-engineering evaluation

| Dimension | Assessment | Reason |
|---|---|---|
| Purpose clarity | Strong | The opening immediately names the failure the router remedies. |
| Conceptual compression | Strong | Three statements establish motivation, a navigation model, and a coarse taxonomy. |
| Context economy | Strong | Familiar language carries more structure than a longer explanation of retrieval and routing mechanics would require. |
| Metaphorical coherence | Strong | *Flow*, *path*, *on-ramp*, and *underneath* reinforce one spatial model. |
| Human readability | Strong | The prose works as agent context while remaining understandable to someone inspecting the skill. |
| Behavioral precision | Moderate | The introduction orients the agent but cannot route a real request without the later sections. |
| Taxonomic precision | Moderate | The categories are deliberately coarse, and the later structure introduces finer distinctions. |
| Dependence on later context | High but appropriate | The metaphors become operationally reliable only when later sections connect them to concrete skills and decisions. |

The installed `1.2.0` file also contains a small consistency problem: the introduction says that **two** on-ramps merge onto the main flow, while the later `## On-ramps` section presents three bullet-pointed starting situations. This does not destroy the orientation model, but it demonstrates the risk of compressing a changing inventory into a specific count.

### Why the approach works

The introduction applies several reusable context-engineering practices:

1. **State the failure before the machinery.** The agent learns why consultation is necessary before receiving routing details.
2. **Define local terminology early.** Matt gives *flow* a document-specific meaning before relying on it.
3. **Organize before enumerating.** The agent receives a map before encountering the full inventory.
4. **Use a coherent metaphorical system.** The spatial terms reinforce one another rather than introducing unrelated analogies.
5. **Progress from coarse orientation to concrete rules.** The introduction establishes categories; later sections provide decisions, exceptions, and skill relationships.
6. **Write for execution and inspection.** The wording can guide an agent while remaining legible to a human reviewer.

### Overall assessment

The introduction is highly effective as compressed orientation and intentionally insufficient as operational routing logic.

Its sparsity works because each sentence establishes a reusable relationship:

> **known limitation → corrective behavior**
>
> **new concept → familiar mental model**
>
> **remaining elements → coarse workflow categories**

The remainder of the skill then converts those relationships into concrete routes and cross-skill dependencies. Without that follow-through, the introduction would be memorable but underspecified.

The reusable lesson is not simply to write less. It is to make each short statement establish structure that later instructions consistently reuse.

## Handoff Record and System State

### Cold-start instruction

This section is the persistent state record and the sole conversational context carried across session restarts.

On a cold start:

1. Adopt the required persona recorded below.
2. Read this complete review document to recover the detailed analysis.
3. Read the installed runtime `SKILL.md` identified below.
4. Await John's selection of the next passage. Do not advance independently.

### Role and status

```yaml
required_persona: "Senior technical writer specializing in AI training for software project managers"
status: "awaiting_next_passage"
last_completed_topic: "The complete Ask Matt introduction and its context-engineering evaluation"
```

Explain technical concepts accurately for a reader who is technically competent but still developing expertise in AI systems and context engineering.

### Purpose and sources

**Goal:** Study Matt Pocock's `ask-matt` skill closely to understand how its sparse language influences human comprehension and agent behavior. Extract reusable skill-writing and context-engineering practices, then eventually evaluate Matt's implementation against John's own version.

**Persistent state and review document:**

```text
john-notes/ask-matt-skill-review.md
```

**Installed runtime artifact under review:**

```text
C:\Users\johnp\.claude\plugins\cache\mattpocock\mattpocock-skills\1.2.0\skills\engineering\ask-matt\SKILL.md
```

The runtime artifact is the installed `ask-matt` skill loaded by Claude Code from the `mattpocock-skills` plugin at version `1.2.0`. It is distinct from the editable source file in Matt's repository.

Before analyzing a new passage, read both the complete review document and the installed runtime artifact. Ground the discussion in the runtime artifact rather than relying on memory.

### Settled findings

#### Router rationale

> You don't remember every skill, so ask.

The sentence uses a limitation-and-response structure:

> **incomplete recall → consultation**

It operationally addresses the agent while remaining readable as guidance to the user. It names a predictable failure—incomplete recall of the skill inventory—and immediately supplies the recovery behavior: consult the router rather than improvise.

#### Copular definition

> A flow is a path through the skills.

The sentence associates the new term *flow* with the familiar concept *path*. In context, this can bias subsequent interpretation and generation toward path-related ideas such as sequence, direction, branching, intermediate points, and destination.

This is a plausible account of the sentence's effect on model behavior. Do not overstate it as proof of a particular latent-space geometry, attention pattern, or reduction in prediction entropy.

#### Remainder classification

> Everything else is standalone, or a vocabulary layer that runs underneath.

The sentence classifies skills outside the primary routes by workflow role. *Standalone* means independently reachable rather than isolated, stateless, or dependency-free. A vocabulary layer is a cross-cutting discipline that can inform multiple workflow steps without occupying a numbered position in the flow.

The repository makes this relationship concrete through `/domain-modeling` and `/codebase-design`. These are behaviorally active disciplines used by other skills, not passive dictionaries or hidden background processes.

#### Introduction synthesis

The introduction installs a three-part orientation model:

> **incomplete recall → consult the router**
>
> **flow → path through skills**
>
> **skill participation → routes, independent entry points, or cross-cutting disciplines**

This is effective context engineering because it establishes purpose, a shared mental model, and coarse categories before presenting the detailed inventory. Its behavioral and taxonomic precision depend on the later sections cashing out the metaphors through concrete routing rules.

`/domain-modeling` demonstrates the meaning of a vocabulary layer: it actively maintains domain language across several workflows without occupying a fixed position in the main flow.

The installed `1.2.0` skill says that two on-ramps merge onto the main flow, while its later `## On-ramps` section contains three bullet-pointed starting situations. Treat this as a source inconsistency rather than silently forcing the later inventory into the opening count.

#### Context continuity

A restarted session does not automatically receive the preceding conversation. Durable conclusions, working constraints, and the current stopping point must therefore be recorded explicitly in this handoff.

#### Formatting choice

The handoff combines:

- YAML for compact, predictable status fields.
- Markdown for nuanced decisions and human-readable instructions.

This format was selected for clarity and maintainability. Do not claim improved model performance without evaluation evidence.

### Current position

Review of the complete Ask Matt introduction is finished, including its aggregate context-engineering evaluation:

> You don't remember every skill, so ask.
>
> A flow is a path through the skills. Most paths run along one main flow, and two on-ramps merge onto it. Everything else is standalone, or a vocabulary layer that runs underneath.

The next unreviewed section begins at:

```md
## The main flow: idea → ship
```

No passage from that section has been selected. Await John's selection and do not advance independently.

### Working protocol

- **One passage at a time:** Analyze only the passage John selects. Do not advance to adjacent material independently.
- **Dual analysis:** Investigate both the language and its likely operational effect on an AI agent.
- **Epistemic discipline:** Clearly distinguish established model mechanics from plausible interpretation or speculation.
- **Audience:** Prefer concise, structured explanations for software project managers who are technically competent but still learning AI.
- **Convergence first:** Do not advance or record a conclusion while either party has unresolved questions. Treat John's confirmation of convergence as the transition signal.
- **Shared memory:** Treat this document as the semantic handshake between John and the agent.

### Maintenance protocol

When the discussion produces a durable conclusion or changes the stopping point:

1. Draft the proposed update.
2. Show John the exact wording.
3. Obtain John's explicit approval before editing this document.
4. Update only the affected findings, status fields, or stopping point.
5. Verify the saved document after editing.

Do not update the handoff merely because a turn has ended.
