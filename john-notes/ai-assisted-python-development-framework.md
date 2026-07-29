# AI-Assisted Python Development Framework

## Program-Manager Operating Model for Claude Code and Codex

**Status:** Working Draft
**Primary audience:** Experienced project managers directing AI coding agents
**Primary use case:** Python tools, scripts, command-line interfaces, and reusable components

---

## 1. Purpose

This framework defines a controlled process for developing Python software when human leadership remains at the program-management level and AI agents perform the coding.

The program manager governs:

* Product intent
* Scope
* Public promises
* Acceptance criteria
* Dependencies
* Risk
* Change control
* Release readiness

The coding agents govern:

* Codebase exploration
* Technical design proposals
* Test implementation
* Production implementation
* Refactoring
* Debugging
* Technical verification

The objective is not to supervise code line by line. The objective is to manage **promises, evidence, dependencies, and change**.

---

## 2. Operating Model

The program manager acts as the domain authority and final decision-maker. The AI coding agent acts as a senior software engineer.

The agent is expected to:

* Translate product intent into precise engineering language
* Identify ambiguities, contradictions, and hidden dependencies
* Correct material technical misunderstandings
* Recommend sensible defaults
* Distinguish facts from decisions
* Present consequential decisions for human approval
* Avoid silently changing approved scope or contracts

Development discussions proceed one decision at a time. Decisions remain provisional until explicitly accepted. Previously accepted decisions may be reopened when new evidence exposes a defect, inconsistency, or better alternative.

The resulting software process is iterative but governed. Discovery is permitted; invisible scope drift is not.

---

## 3. Core Principles

### 3.1 Interface First

Before implementation begins, draft the component’s externally observable interface.

For a Python CLI or reusable tool, the interface contract should define:

* Accepted inputs
* Required and optional arguments
* Input formats and validation rules
* Guaranteed outputs
* Output schemas and formats
* Exit codes
* Error categories
* `stdout` and `stderr` behavior
* Filesystem or database side effects
* Idempotency expectations
* Ordering guarantees
* Compatibility commitments
* Security or privacy constraints
* Conditions under which partial work may remain

The interface is a functional contract. Humans, agents, scripts, and downstream components must be able to rely on it.

The initial interface should be substantially complete, but it is not immutable. It is a strong provisional design that will be tested against detailed requirements and implementation discoveries.

Any interface revision must be deliberate, documented, and evaluated for its effect on consuming systems.

---

### 3.2 Requirements Explain Intent

Requirements define what behavior is needed and why it matters.

They should describe:

* The problem being solved
* Required capabilities
* Business or operational rationale
* Constraints
* Acceptance conditions
* Explicit exclusions
* Compatibility obligations
* Performance expectations
* Security and data-handling expectations

Requirements should exist before implementation at a sufficient level to establish scope. They are then **elaborated test by test** as specific behavior is developed.

Requirements must not be invented entirely during coding. Test-driven development is a method for refining and implementing an agreed product, not a substitute for deciding what product to build.

---

### 3.3 Tests Provide Executable Evidence

Tests do not create the contract. They provide executable evidence that the implementation honors it.

Each test should trace to:

* A requirement
* A public interface promise
* A known failure mode
* A compatibility obligation
* A previously discovered defect

A green test demonstrates only that the implementation satisfies what that particular test asserts. It does not prove that the test itself correctly represents the requirement.

Tests are therefore evidentiary artifacts, not autonomous sources of product authority.

---

### 3.4 Development Proceeds in Vertical Slices

Implementation proceeds one behavior at a time.

For each behavior:

1. Select one approved or provisionally approved behavior.
2. Clarify the associated requirement.
3. Write one failing test through the appropriate public interface or test seam.
4. Confirm that the test fails for the intended reason.
5. Review whether the test exposes an ambiguity or defect in the requirement or interface.
6. Escalate any normative conflict for human decision.
7. Implement the minimum coherent behavior needed to pass the test.
8. Confirm that the test passes.
9. Run the relevant existing regression tests.
10. Confirm continued alignment with the current requirements and interface contract.
11. Record any newly discovered requirement, risk, or design decision.

The next behavior begins only after the current slice is coherent.

This follows the vertical-slice model in Matt Pocock’s TDD skill: one failing test, one minimal implementation, then the next slice. His current workflow places broader refactoring in the review stage rather than automatically expanding every red-green cycle. ([GitHub][1])

---

### 3.5 Verification Must Be Observable

An agent’s statement that work is complete is not sufficient evidence.

The agent must be given checks it can run and interpret, such as:

* Automated tests
* Type checking
* Linting
* Package builds
* Installation checks
* Schema validation
* Golden-file comparisons
* Output snapshots
* Performance thresholds
* Security scans
* Documentation example execution

Claude Code’s official guidance emphasizes giving the agent a concrete way to verify its work and recommends exploring and planning before implementation. ([Claude][2])

---

### 3.6 Public Interfaces Are Expensive to Change

A released interface may be consumed by code that is not currently visible to the implementing agent.

Most defects should therefore be corrected by restoring compliance with the existing contract.

A public interface should change only when:

* The contract is internally inconsistent
* Correct behavior cannot be expressed through it
* A security or data-integrity defect requires revision
* A deliberate product decision justifies the compatibility cost

Convenience for the current implementation is not sufficient justification.

---

## 4. Artifact Model

### 4.1 Normative Artifacts

Normative artifacts define what the system is supposed to do.

#### Requirements and Specification

Defines:

* Intended behavior
* Rationale
* Constraints
* Scope
* Acceptance conditions
* Exclusions

#### Interface Contract

Defines:

* Externally observable promises
* Input and output behavior
* Error behavior
* Compatibility expectations
* Side effects

Requirements and interface contracts are complementary. Neither may be silently rewritten to resolve a conflict.

If they disagree, the disagreement must be presented to the program manager for an explicit decision.

---

### 4.2 Evidentiary Artifacts

Evidentiary artifacts demonstrate whether the normative artifacts have been satisfied.

These include:

* Acceptance tests
* Contract tests
* Unit tests
* Integration tests
* End-to-end tests
* Build results
* Static-analysis results
* Installation smoke tests
* Independent review findings

Tests may reveal that a requirement or interface is flawed. They do not unilaterally redefine it.

---

### 4.3 Explanatory Artifacts

Explanatory artifacts help humans and agents understand usage, rationale, and implementation.

These include:

* `README.md`
* User guides
* Architecture Decision Records
* Code comments
* Domain glossaries
* Operational runbooks

Explanatory documentation should not duplicate normative documents unnecessarily.

---

### 4.4 Implementation

The implementation is the code that realizes the approved behavior.

Code is not the final authority on intended behavior. Existing behavior may be accidental, defective, or undocumented.

When the code conflicts with an approved requirement or interface contract, the conflict must be investigated rather than rationalized.

---

## 5. Behavior Ledger

Before coding begins, create a provisional behavior ledger.

Each entry should include:

| Field                  | Purpose                                                  |
| ---------------------- | -------------------------------------------------------- |
| Behavior ID            | Stable traceability identifier                           |
| Behavior               | Concise statement of required behavior                   |
| Rationale              | Why the behavior exists                                  |
| Interface surface      | Command, function, file, schema, or other public seam    |
| Representative example | Concrete input and expected result                       |
| Known edge cases       | Boundary conditions already identified                   |
| Failure behavior       | Expected response to invalid or impossible conditions    |
| Priority               | Delivery order or importance                             |
| Status                 | Unverified, in progress, verified, deferred, or rejected |
| Test references        | Tests that provide evidence                              |
| Notes                  | Open questions or dependencies                           |

The ledger is an implementation control surface, not a replacement for the specification.

It prevents the agent from:

* Declaring completion after implementing only the obvious path
* Losing requirements across context windows
* Implementing features in an arbitrary order
* Forgetting deferred edge cases
* Conflating “coded” with “verified”

Anthropic’s long-running-agent work found that maintaining a feature list and directing the agent to complete one feature at a time improved continuity across extended development. ([Anthropic][3])

---

## 6. Testing Strategy

### 6.1 Contract and Acceptance Tests

Contract tests verify externally observable behavior.

For a CLI, they may verify:

* Command invocation
* Argument parsing
* Exit codes
* `stdout`
* `stderr`
* Generated files
* Output schemas
* Error messages
* Side effects
* Compatibility with existing outputs

These tests provide the most direct evidence for program-level acceptance.

---

### 6.2 Unit Tests

Unit tests verify focused internal behavior such as:

* Parsing
* Validation
* Calculations
* Transformations
* State transitions
* Error classification

Unit tests are valuable but cannot independently prove that the installed CLI or integrated component works correctly.

---

### 6.3 Integration Tests

Integration tests verify interactions with:

* Filesystems
* Databases
* External APIs
* Subprocesses
* Configuration systems
* Serialization libraries
* Third-party packages

External dependencies should be isolated or controlled sufficiently to keep failures interpretable.

---

### 6.4 Packaging and Installation Tests

Reusable Python tools should be tested as installed products, not only as source trees.

Verification should include:

* Building the package
* Installing it into a clean environment
* Importing the package
* Launching the CLI entry point
* Running at least one representative command
* Confirming dependency and Python-version compatibility

---

### 6.5 Regression Tests

Every accepted TDD test becomes part of the permanent regression suite.

“TDD” describes how a test originates. “Regression testing” describes the protection that the accumulated tests provide over time.

A separate duplicate regression suite is unnecessary. Additional suites should exist only where they verify a distinct scope, such as integration, packaging, compatibility, performance, or security.

---

## 7. Development Lifecycle

### Phase 1: Classify the Work

Classify the proposed work before selecting the process.

#### Disposable Spike

Purpose:

* Explore feasibility
* Learn an API
* Compare alternatives
* Reduce uncertainty

Characteristics:

* No compatibility promise
* Minimal documentation
* May use lightweight tests
* Must be discarded or deliberately promoted

#### Internal Script

Purpose:

* Support a bounded personal or internal workflow

Characteristics:

* Lightweight requirements
* Explicit inputs and outputs
* Tests for critical behavior
* Basic usage documentation
* Limited compatibility commitments

#### Reusable Component or CLI

Purpose:

* Serve multiple workflows, agents, or downstream systems

Characteristics:

* Full interface contract
* Approved specification
* Behavior ledger
* Vertical TDD
* Layered testing
* Independent review
* Versioning and interface-change governance

A prototype must not drift silently into production. Promotion requires an explicit decision and completion of the missing controls.

---

### Phase 2: Explore

The coding agent examines:

* Existing code
* Repository conventions
* Related components
* Current interfaces
* Test infrastructure
* Build and packaging configuration
* Dependencies
* Known constraints
* Existing documentation

Exploration should produce findings, risks, and questions. It should not modify production code.

---

### Phase 3: Clarify

The program manager and senior engineering agent resolve decisions one at a time.

The discussion should distinguish:

* Facts that can be determined from the codebase or external documentation
* Decisions that require human judgment
* Assumptions that require validation
* Open risks that cannot yet be resolved

Matt Pocock’s `grilling` process uses a one-question-at-a-time decision tree, while `grill-with-docs` connects that dialogue to domain terminology and durable decisions. ([GitHub][4])

---

### Phase 4: Specify

Create or update:

* Problem statement
* Goals
* Non-goals
* Requirements
* Interface contract
* Behavior ledger
* Testing seams
* Constraints
* Dependencies
* Risks
* Release assumptions

Matt Pocock’s `to-spec` skill converts clarified discussion and codebase understanding into a specification and identifies the seams through which the feature will be tested. ([GitHub][5])

---

### Phase 5: Decompose

Divide the work into independently reviewable vertical slices.

Each slice should:

* Deliver observable behavior
* Trace to one or more requirements
* Include its testing seam
* Avoid unnecessary implementation prescription
* Be small enough to complete coherently
* Leave the repository in a working state

In Matt Pocock’s current workflow, `to-tickets` follows `to-spec` and prepares implementation-ready vertical slices. ([GitHub][6])

---

### Phase 6: Implement Test by Test

For each slice:

```text
Select behavior
    ↓
Clarify requirement
    ↓
Write one failing test
    ↓
Confirm intended failure
    ↓
Review interface implications
    ↓
Implement minimum coherent behavior
    ↓
Pass the test
    ↓
Run relevant regression tests
    ↓
Update traceability and documentation
```

No test may be weakened merely to obtain a green result.

No requirement or interface promise may be changed invisibly to accommodate an implementation shortcut.

---

### Phase 7: Review

Review should address two separate questions:

#### Conformance Review

* Does the implementation satisfy the approved requirements?
* Does it honor the interface contract?
* Are all required behaviors represented by evidence?
* Has unauthorized scope been added?
* Have exclusions been respected?

#### Engineering Review

* Is the design maintainable?
* Is the implementation appropriately simple?
* Are names and boundaries clear?
* Is duplication justified?
* Are errors handled consistently?
* Are comments useful?
* Are security and performance risks addressed?
* Can the implementation be simplified without changing behavior?

Refactoring belongs primarily at this stage, after behavior has been established and protected by tests.

---

### Phase 8: Verify and Release

Run the complete Definition of Done.

Record:

* Commands executed
* Tool versions when material
* Test results
* Build results
* Static-analysis results
* Installation results
* Known limitations
* Deferred work
* Review findings and their resolution
* Version or release identifier

Release only when unresolved findings are explicitly accepted by the responsible human.

---

## 8. Separation of Duties

For nontrivial components, use distinct logical roles.

### Specification Agent

Responsibilities:

* Explore the problem and repository
* Draft requirements
* Draft the interface contract
* Identify ambiguity and risk
* Prepare the behavior ledger
* Define acceptance evidence

The specification agent does not approve its own unresolved assumptions.

---

### Implementation Agent

Responsibilities:

* Implement one behavior at a time
* Write and run tests
* Preserve the approved interface
* Report conflicts
* Maintain traceability
* Produce a coherent final diff

The implementation agent may recommend changes but may not silently redefine the product.

---

### Verification Agent

Responsibilities:

* Review the approved requirements and interface
* Examine the final diff
* Run or inspect verification evidence
* Search for missing edge cases
* Challenge assumptions
* Identify accidental behavior changes
* Report unresolved findings independently

The verification role should use a fresh context when practical. This reduces correlated error between the reasoning that created the implementation and the reasoning that evaluates it.

Claude Code subagents operate in isolated context windows and can be configured with focused instructions and restricted tool access, making them suitable for specialized exploration or review. ([Claude][7])

For consequential releases, a separate Claude Code session may provide stronger independence than a subagent within the implementation session.

---

## 9. Definition of Done

A reusable Python component or CLI is complete only when all applicable conditions are satisfied.

### Product Conformance

* Approved requirements are implemented.
* Explicit exclusions remain excluded.
* Every required behavior has acceptance evidence.
* The public interface matches the approved contract.
* No unapproved interface change has occurred.
* Error behavior is tested.
* Known edge cases are tested or explicitly deferred.

### Automated Quality

* The complete test suite passes.
* Static type checking passes.
* Linting passes.
* Formatting checks pass.
* The package builds successfully.
* Clean-environment installation succeeds.
* The installed CLI or package passes a smoke test.
* Documentation examples execute successfully where practical.

### Review

* Conformance review is complete.
* Engineering review is complete.
* Material findings are resolved or explicitly accepted.
* The final diff contains no unrelated changes.
* Security-sensitive changes receive appropriate scrutiny.

### Documentation

* Usage documentation is current.
* Interface documentation is current.
* Requirements and behavior status are current.
* Consequential design decisions are recorded.
* Known limitations are visible.
* Migration instructions exist for compatibility changes.

### Release Evidence

* Verification commands and outcomes are recorded.
* The release or version identifier is recorded.
* Rollback or recovery expectations are understood where relevant.

---

## 10. Documentation Architecture

### 10.1 `CLAUDE.md`

Use `CLAUDE.md` for concise, stable instructions that should influence every relevant session, such as:

* Standard build commands
* Standard test commands
* Repository-wide conventions
* Architectural invariants
* Universal workflow rules
* Prohibited actions
* Required escalation conditions

Do not use it as a warehouse for full specifications, lengthy procedures, or volatile project notes.

Claude Code treats `CLAUDE.md` and auto memory as context rather than hard enforcement. Its documentation recommends moving multi-step procedures into skills and using path-scoped rules for instructions that apply only to portions of a repository. ([Claude][8])

---

### 10.2 `.claude/rules/`

Use path-scoped rules for instructions tied to:

* Specific packages
* File types
* Test directories
* Database code
* CLI code
* Documentation
* Security-sensitive modules

This limits irrelevant context and reduces contradictory instructions.

---

### 10.3 `.claude/skills/`

Use skills for repeatable multi-step workflows, such as:

* `develop-component`
* `implement-behavior`
* `fix-defect`
* `review-interface`
* `verify-release`
* `update-documentation`
* `promote-prototype`

Skills should define procedures, required inputs, outputs, stopping conditions, and escalation rules.

---

### 10.4 Auto Memory

Auto memory may retain:

* Frequently used commands
* Recurring debugging discoveries
* Local workflow preferences
* Non-authoritative implementation observations

Auto memory must not become the sole repository for:

* Approved requirements
* Interface contracts
* Release decisions
* Security rules
* Compatibility commitments
* Architecture decisions

Authoritative project knowledge must remain visible, reviewable, and version-controlled.

---

### 10.5 Hooks and CI

Use prompts and skills for judgment. Use hooks and CI for deterministic enforcement.

Hooks are appropriate for:

* Blocking prohibited commands
* Formatting edited files
* Validating protected paths
* Running targeted checks after edits
* Requiring checks before task completion
* Recording lifecycle events

Claude Code hooks run at defined lifecycle points and provide deterministic controls that do not depend on the model deciding to follow an instruction. ([Claude][9])

CI remains the final repository-level enforcement mechanism for:

* Tests
* Type checking
* Linting
* Packaging
* Security checks
* Compatibility checks
* Release gates

---

## 11. Recommended Repository Structure

```text
project/
├── CLAUDE.md
├── README.md
├── pyproject.toml
├── src/
├── tests/
│   ├── unit/
│   ├── contract/
│   ├── integration/
│   └── smoke/
├── docs/
│   ├── requirements/
│   │   └── component-name.md
│   ├── interfaces/
│   │   └── component-name.md
│   ├── behaviors/
│   │   └── component-name.md
│   ├── adr/
│   ├── user-guide/
│   └── runbooks/
└── .claude/
    ├── rules/
    ├── skills/
    ├── agents/
    └── settings.json
```

The exact structure may be simplified for small projects. The separation of purposes should remain clear even when several sections are combined into one document.

---

## 12. Defect Management

A defect discovered after release reopens the red-green loop.

### Defect Sequence

1. Reproduce the defect.
2. Minimize the reproduction where practical.
3. Write a failing regression test.
4. Confirm that the test fails for the intended reason.
5. Classify the defect.
6. Assess upstream and downstream effects.
7. Update normative artifacts only where justified.
8. Implement the smallest coherent correction.
9. Run the complete regression suite.
10. Perform focused independent review.
11. Retain the new test permanently.
12. Record compatibility or migration consequences.

### Defect Classification

#### Implementation Defect

The code violates the existing requirement or interface contract.

Expected response:

* Correct the implementation
* Preserve the interface
* Add the regression test
* Clarify documentation only if useful

#### Requirements Omission

The intended behavior was not adequately specified.

Expected response:

* Obtain a human decision
* Add or revise the requirement
* Assess interface implications
* Add the regression test
* Implement the approved behavior

#### Contract Defect

The public interface is ambiguous, contradictory, unsafe, or incapable of expressing correct behavior.

Expected response:

* Escalate for explicit approval
* Identify affected consumers
* Classify compatibility impact
* Define migration or deprecation
* Revise contract tests
* Version the change appropriately

#### Environmental or Dependency Defect

The failure originates in packaging, configuration, dependency behavior, platform assumptions, or operational context.

Expected response:

* Capture the environmental condition
* Add the appropriate integration or smoke test
* Update compatibility or operational documentation
* Avoid disguising the problem as a narrow unit-level fix

---

## 13. Interface Change Control

Every proposed change to a released interface must document:

* The reason for the change
* The defect or requirement that motivates it
* Alternatives considered
* Compatibility classification
* Affected consumers
* Migration approach
* Deprecation strategy
* Versioning impact
* Updated tests
* Approval

Compatibility classifications should include:

* Backward compatible
* Backward compatible with deprecation
* Behaviorally changed but syntactically compatible
* Breaking
* Security-mandated breaking change

A change that is syntactically compatible may still be behaviorally breaking if consumers rely on ordering, timing, error behavior, defaults, side effects, or output content.

---

## 14. Documentation Standards

### Requirements and Interface Documents

Write for decision-makers and implementers.

Use:

* Normative language
* Concrete examples
* Explicit exclusions
* Stable identifiers
* Defined terminology
* Visible open questions

Avoid:

* Implementation speculation
* Unnecessary code snippets
* Repetition of obvious information
* Ambiguous terms such as “handle gracefully”
* Requirements that cannot be verified

---

### User Documentation

Explain:

* Installation
* Prerequisites
* Common commands
* Input and output examples
* Configuration
* Failure recovery
* Exit codes
* Known limitations

Examples should be executable or automatically checked where practical.

---

### Architecture Decision Records

Create an ADR only for decisions that are:

* Consequential
* Non-obvious
* Difficult to reverse
* Likely to be challenged later
* Based on meaningful trade-offs

An ADR should record:

* Context
* Decision
* Alternatives
* Consequences
* Status

ADRs explain why a design was selected. They do not replace requirements or interfaces.

---

### Code Comments

Comments should explain:

* Why an unusual approach is necessary
* Invariants
* Hidden assumptions
* Compatibility hazards
* Security considerations
* Performance trade-offs
* Counterintuitive behavior

Comments should not narrate code that is already clear from names and structure.

---

## 15. Mapping to Matt Pocock’s Skills

The framework can be implemented through the following skill sequence:

```text
setup-matt-pocock-skills
    ↓
grill-with-docs
    ↓
to-spec
    ↓
to-tickets
    ↓
implement
        ↳ repeated tdd slices
    ↓
code-review
```

### `setup-matt-pocock-skills`

Establishes repository conventions, issue-tracker configuration, domain-document locations, and shared workflow assumptions.

### `grill-with-docs`

Supports atomic clarification, terminology refinement, domain modeling, and durable decision capture.

### `to-spec`

Converts the clarified product intent into an implementation-independent specification with explicit testing seams.

### `to-tickets`

Decomposes the specification into independently deliverable vertical slices.

### `implement`

Coordinates implementation of the approved work.

### `tdd`

Provides the red-green engine for one behavior at a time.

### `code-review`

Separates specification conformance from engineering-quality review and provides the appropriate stage for refactoring.

### `diagnosing-bugs`

Provides the defect on-ramp:

```text
reproduce
    → minimize
    → hypothesize
    → instrument
    → fix
    → retain regression test
```

Matt Pocock’s current repository presents the principal delivery flow as idea clarification, specification, ticket decomposition, implementation, and review, with TDD operating inside implementation. ([GitHub][10])

---

## 16. Anti-Patterns

The process should explicitly prevent the following behaviors.

### Coding Before the Contract Is Understood

The agent begins implementation while inputs, outputs, or failure behavior remain ambiguous.

### Horizontal Test Writing

The agent writes an entire speculative test suite before implementing and learning from the first behavior.

### Test Laundering

The agent weakens, deletes, or rewrites a failing test merely to produce green.

### Contract Erosion

The interface is gradually changed to fit implementation convenience.

### Silent Scope Expansion

The agent adds adjacent features that were not approved.

### Implementation-Led Requirements

Existing code behavior is treated as the intended requirement without investigation.

### Self-Certification

The same context writes the specification, implementation, tests, and final approval without independent challenge.

### Documentation Duplication

The same promise is rewritten across several documents until the copies diverge.

### Prompt-Only Enforcement

Mandatory checks are described in instructions but are not enforced through executable tooling.

### Prototype Drift

Exploratory code becomes operational without formal promotion, testing, documentation, or compatibility review.

### Context Accumulation

One Claude Code session is allowed to absorb extensive exploration, implementation, debugging, and review until the context becomes noisy and unreliable.

---

## 17. Program-Manager Control Points

Human approval is required when:

* Product scope changes
* A normative artifact conflicts with another
* A public interface may change
* A new dependency materially affects risk
* A security or data-handling issue appears
* A requirement is removed or deferred
* A test appears inconsistent with product intent
* A breaking change is proposed
* A known defect is accepted for release
* Independent review identifies a material unresolved risk
* A prototype is promoted into maintained software

Human attention should not be required for:

* Routine code generation
* Mechanical refactoring within approved boundaries
* Running tests
* Formatting
* Lint correction
* Straightforward implementation choices that do not affect the contract
* Updating traceability after an approved behavior is completed

The process should concentrate human judgment where consequences are durable and delegate mechanical execution to agents.

---

## 18. End-to-End Framework

```text
Classify the work
    ↓
Explore the problem and repository
    ↓
Clarify facts, terminology, and decisions
    ↓
Draft requirements
    ↓
Draft the provisional interface contract
    ↓
Create the behavior ledger
    ↓
Approve scope, exclusions, and testing seams
    ↓
Decompose into vertical slices
    ↓
For each behavior:
    refine requirement
    → write failing test
    → verify intended failure
    → review contract implications
    → implement minimally
    → obtain green
    → run relevant regression checks
    → update traceability
    ↓
Run the complete Definition of Done
    ↓
Conduct independent conformance and engineering review
    ↓
Resolve or explicitly accept findings
    ↓
Version, document, and release
    ↓
Route future defects back through the same controlled loop
```

---

## 19. Governing Statement

AI coding agents may design and implement the software, but they do not independently determine what the software promises, whether those promises may change, or whether the resulting evidence is sufficient for release.

The program manager governs intent, contracts, acceptance, dependencies, and change.

The agents produce implementation and evidence within those boundaries.

[1]: https://github.com/mattpocock/skills/blob/main/skills/engineering/tdd/SKILL.md?utm_source=chatgpt.com "skills/skills/engineering/tdd/SKILL.md at main · mattpocock/skills · GitHub"
[2]: https://code.claude.com/docs/en/best-practices?utm_source=chatgpt.com "Best practices for Claude Code"
[3]: https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents?lid=1f0n56CPItf3Nm9NR&utm_source=chatgpt.com "Effective harnesses for long-running agents \ Anthropic"
[4]: https://github.com/mattpocock/skills/blob/main/docs/engineering/grill-with-docs.md?utm_source=chatgpt.com "skills/docs/engineering/grill-with-docs.md at main · mattpocock/skills · GitHub"
[5]: https://github.com/mattpocock/skills/blob/main/skills/engineering/to-spec/SKILL.md?utm_source=chatgpt.com "skills/skills/engineering/to-spec/SKILL.md at main · mattpocock/skills · GitHub"
[6]: https://github.com/mattpocock/skills/blob/main/CHANGELOG.md?utm_source=chatgpt.com "skills/CHANGELOG.md at main · mattpocock/skills · GitHub"
[7]: https://code.claude.com/docs/en/sub-agents?utm_source=chatgpt.com "Create custom subagents - Claude Code Docs"
[8]: https://code.claude.com/docs/en/memory?utm_source=chatgpt.com "How Claude remembers your project - Claude Code Docs"
[9]: https://code.claude.com/docs/en/hooks-guide?utm_source=chatgpt.com "Automate actions with hooks - Claude Code Docs"
[10]: https://github.com/mattpocock/skills?utm_source=chatgpt.com "GitHub - mattpocock/skills: Skills for Real Engineers. Straight from my .claude directory. · GitHub"
