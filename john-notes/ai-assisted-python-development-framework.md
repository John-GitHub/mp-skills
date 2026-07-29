# AI-Assisted Python Development Framework

## A skill-driven operating model for software project managers

**Status:** Working draft

**Audience:** Software project managers directing AI coding agents
**Use case:** Python modules, command-line tools, and small applications

---

## 1. Purpose

This framework keeps human product authority and agent execution in balance.

The human owns:

- product intent and scope;
- consequential interface promises;
- requirement decisions;
- compatibility and release decisions;
- acceptance of material risk.

The agent owns:

- discovering facts from the repository and trusted sources;
- identifying ambiguity, contradictions, and dependencies;
- recommending technical choices;
- implementing one behavior at a time;
- producing tests and other verification evidence;
- reporting conflicts rather than silently changing the product.

Facts should be discovered. Decisions should be put to the human. Mechanical work should be delegated.

The framework is built around one consistency model:

```text
working interface ↔ confirmed requirement ↔ executable behavior test
```

Development generally moves from left to right, one behavior at a time. After every change, it looks back in both directions to ensure the three still agree.

---

## 2. The consistency model

### 2.1 The three parts

| Part | Meaning |
|---|---|
| **Working interface** | The current whole-capability design: everything callers must know about inputs, outputs, errors, invariants, side effects, ordering, configuration, and compatibility. |
| **Confirmed requirement** | One approved, observable behavior or constraint, including why it matters. |
| **Unit test** | A fast executable test of one behavior through a pre-agreed public seam. Prefer the highest existing seam that demonstrates the behavior. Test scope and speed do not grant permission to reach into private helpers. |

The interface describes the intended whole, but only at the resolution current understanding justifies. It is a design hypothesis, not a license to invent details.

Mark its parts mentally or explicitly as:

- **Provisional** — a current design proposal, cheap to revise.
- **Confirmed** — supported by an approved requirement.
- **Implemented** — supported by passing evidence.

Requirements express intended behavior. The interface expresses where callers observe it. Tests provide evidence. None is authoritative in isolation.

### 2.2 The governing loop

```text
Draft the whole interface at useful resolution
    ↓
Select one capability
    ↓
Confirm one requirement
    ↓
Reconcile the requirement with the interface
    ↓
Write one failing test at an agreed seam
    ↓
Confirm that it fails for the intended reason
    ↓
Implement the minimum coherent behavior
    ↓
Obtain green
    ↓
Run affected prior tests
    ↓
Review interface ↔ requirement ↔ tests for consistency
    ↓
Select the next capability
```

After every slice, ask:

1. Does the interface still express the confirmed requirement?
2. Does the test faithfully demonstrate the requirement?
3. Did the implementation reveal a defect or ambiguity in either?
4. Would a proposed revision affect an earlier requirement, test, caller, or compatibility promise?
5. Have all affected prior tests been rerun?

A discovery may revise the working interface or a requirement. It may not revise either silently.

### 2.3 Evidence beyond unit tests

The unit-test loop governs individual behaviors, but the appropriate evidence follows the seam.

Python software may also require:

- CLI contract tests for arguments, exit codes, `stdout`, and `stderr`;
- integration tests for filesystems, databases, or external services;
- package build and clean-environment installation checks;
- type checking, linting, and formatting;
- smoke tests of the installed entry point.

These checks supplement the requirement–unit-test loop; they do not replace it.

---

## 3. Route the work before starting

Run [`/setup-matt-pocock-skills`](../skills/engineering/setup-matt-pocock-skills/SKILL.md) once per repository, or whenever the tracker and domain-document configuration is missing. If the correct route is unclear, invoke the user-facing [`/ask-matt`](../skills/engineering/ask-matt/SKILL.md) router.

| Situation | Skill route |
|---|---|
| Ordinary work that can be understood in one session | `/grill-with-docs` → direct `/implement` |
| Plan or design with no codebase | `/grill-me` |
| Multi-session build | `/grill-with-docs` → `/to-spec` → `/to-tickets` → fresh `/implement` session per ticket |
| Huge, foggy effort whose route is not yet visible | `/wayfinder` → resolve decision tickets → `/to-spec` → `/to-tickets` |
| One design question needs a runnable answer | `/handoff` → `/prototype` → `/handoff` back |
| Hard bug or performance regression | `/diagnosing-bugs` |
| Incoming issue, external request, or external pull request | `/triage` |
| A decision depends on external facts | `/research` |

Do not force every piece of work through every artifact. Choose the smallest route that preserves clarity, evidence, and context.

The orchestration skills in these routes—including `/ask-matt`, `/grill-with-docs`, `/wayfinder`, `/to-spec`, `/to-tickets`, `/implement`, `/handoff`, `/triage`, and setup—are user-invoked. The human types the command; the agent does not silently start it. Supporting skills such as `/grilling`, `/domain-modeling`, `/codebase-design`, `/prototype`, `/tdd`, `/code-review`, `/diagnosing-bugs`, and `/research` may also be selected by the model when their descriptions match the work.

**Toy example**

```text
/ask-matt

I want to build a reusable Python CSV-cleaning module with a CLI.
The intended behavior is still fuzzy, but I expect the work to fit
within one development session. Which flow should I use?
```

For this example, assume the router selects `/grill-with-docs`, followed by direct implementation.

---

## 4. The organic development loop

The same toy project runs through every step: a reusable `csv-clean` module with a CLI adapter.

### Step 1: Clarify intent and draft the whole interface

**Applicable skills**

- [`/grill-with-docs`](../skills/engineering/grill-with-docs/SKILL.md) runs the one-question-at-a-time [`/grilling`](../skills/productivity/grilling/SKILL.md) process while using [`/domain-modeling`](../skills/engineering/domain-modeling/SKILL.md).
- `/domain-modeling` sharpens terms, checks them against the code, updates `CONTEXT.md`, and records only qualifying ADRs.
- [`/codebase-design`](../skills/engineering/codebase-design/SKILL.md) supplies the vocabulary for the module, interface, implementation, and testing seam.
- [`/prototype`](../skills/engineering/prototype/SKILL.md) answers a question that conversation cannot settle.

Start with the user-visible problem. Let the agent inspect the repository for facts. Resolve decisions one at a time.

```text
/grill-with-docs

I want a reusable Python CSV-cleaning module and CLI. Interview me one
decision at a time. Help me form a working interface for the complete
capability, but leave unresolved behavior visibly provisional.
```

Then design the module shape:

```text
/codebase-design

Using the decisions we have settled, propose a deep module with a small
public interface and a thin CLI entry point over it. Identify the highest
existing seam through which the module's behavior should be tested.
```

The first working draft might be:

```python
from typing import TextIO


def clean_csv(
    source: TextIO,
    destination: TextIO,
    *,
    trim_whitespace: bool = True,
    drop_blank_rows: bool = False,
) -> CleanResult:
    ...
```

```text
csv-clean INPUT --output OUTPUT [--keep-whitespace] [--drop-blank-rows]
```

This draft covers the intended capability, but not every detail is equally settled. For example, whitespace trimming may be confirmed while the definition of a “blank row” remains provisional.

If quoted-field behavior cannot be settled on paper, use a prototype to answer only that question:

```text
/prototype

Build a throwaway terminal prototype that shows how Python's CSV parser
treats leading and trailing whitespace in quoted and unquoted fields.
The question is which distinctions our public interface must preserve.
```

Keep the answer. Do not promote the prototype code into production. Capture the prototype as a primary source on a throwaway branch outside `main`, link it from the implementation issue, and implement the validated decision through the normal production workflow.

### Step 2: Pin down the next requirement

**Applicable skills**

- `/grilling` resolves one decision at a time and recommends an answer.
- `/domain-modeling` resolves ambiguous domain terms.
- `/prototype` or [`/research`](../skills/engineering/research/SKILL.md) supplies evidence when conversation alone cannot answer the question.

Select one observable capability—not a layer of implementation.

```text
/grilling

Help me pin down the first requirement for default whitespace trimming.
Ask one question at a time, recommend an answer, and do not implement
anything until I confirm that we share the same understanding.
```

A confirmed first requirement might be:

> **R-001 — Default trimming:** By default, leading and trailing whitespace is removed from unquoted cell values so downstream comparisons are stable. Whitespace inside quoted values is preserved.

Before testing, reconcile it with the working interface:

- Does `trim_whitespace=True` express this behavior?
- Is “whitespace” defined precisely enough?
- Does the requirement change any earlier interface promise?
- Is the behavior observable through `clean_csv`?

If the requirement does not fit the interface cleanly, revise the provisional interface or reopen the requirement. Do not bend one silently to match the other.

### Step 3: Agree the seam and write one failing unit test

**Applicable skills**

- `/codebase-design` checks that the interface is a useful, testable seam.
- [`/tdd`](../skills/engineering/tdd/SKILL.md) enforces the red-green loop and tests behavior through public interfaces.

Matt's TDD skill requires seams to be written down and confirmed before tests are added. Prefer the highest existing seam that demonstrates the behavior, and minimize the number of seams.

```text
/tdd

For R-001, use clean_csv as the pre-agreed public seam. Write one test
that proves default trimming while preserving quoted whitespace. Run it
and show that it fails for the intended missing behavior. Do not write
production code yet.
```

The test might look like:

```python
import csv
from io import StringIO

from csv_clean import clean_csv


def test_clean_csv_trims_unquoted_values_but_preserves_quoted_spaces():
    source = StringIO('name,note\n Alice ," keep me "\n')
    destination = StringIO()

    clean_csv(source, destination)

    assert list(csv.reader(StringIO(destination.getvalue()))) == [
        ["name", "note"],
        ["Alice", " keep me "],
    ]
```

The exact syntax is less important than the relationship:

```text
interface behavior → R-001 → one independently meaningful test
```

Confirm red before proceeding. A syntax error, bad fixture, or unrelated failure is not the intended red.

### Step 4: Implement the minimum coherent behavior

**Applicable skills**

- [`/implement`](../skills/engineering/implement/SKILL.md) coordinates implementation from a spec or ticket, uses `/tdd`, runs checks, invokes `/code-review`, and commits.
- `/tdd` supplies each individual red-green slice.

For a small, single-context change, `/tdd` may be invoked directly. For a spec or ticket, use `/implement` as the coordinator.

```text
/implement

Implement R-001 through the agreed clean_csv seam. Use one TDD slice:
confirm the existing red test, add only the minimum coherent behavior
needed to pass it, then run the affected prior tests. Do not anticipate
R-002 or add speculative options.
```

Run:

1. the new test frequently;
2. affected prior tests after green;
3. type checking regularly;
4. the complete suite at the end of the implementation task.

Do not weaken a valid test to obtain green. If the implementation exposes a conflict, stop and return to the interface or requirement.

### Step 5: Perform the consistency review

**Applicable skills**

- `/grilling` reopens a product decision.
- `/domain-modeling` reconciles changed terminology.
- `/codebase-design` reviews a changed module interface or seam.
- `/tdd` checks whether the test remains valid behavioral evidence.

After green, review the three parts together:

| Check | CSV example |
|---|---|
| Interface ↔ requirement | Does `trim_whitespace=True` still describe R-001 accurately? |
| Requirement ↔ test | Does the test prove both trimming and quoted-space preservation without asserting implementation details? |
| Test ↔ implementation | Did the test go green because the required behavior exists? |
| Backward consistency | Did the discovery affect previous requirements, tests, callers, or compatibility promises? |

Use an explicit checkpoint:

```text
Before continuing, reconcile the clean_csv interface, R-001, its test,
and the affected callers. List any contradiction or newly provisional
decision. Do not change a confirmed requirement or interface promise
without asking me.
```

Suppose implementation reveals that `trim_whitespace` also changes header cells, but the interface and R-001 never settled header behavior. The agent must not choose silently or replace the test with an easier assertion.

Instead:

1. report the conflict;
2. present options and a recommendation;
3. obtain the human decision;
4. revise the interface, requirement, and test together;
5. identify affected earlier behavior;
6. rerun the relevant regression tests.

### Step 6: Repeat with the next capability

**Applicable skills**

- `/grilling` and `/domain-modeling` refine the next requirement.
- `/codebase-design` is revisited only when the module shape or seam changes.
- `/tdd` implements the next vertical slice.
- `/implement` coordinates the complete ticket or small feature.

The next slice might confirm:

> **R-002 — Optional blank-row removal:** When `drop_blank_rows=True`, rows whose cells are all empty after trimming are omitted. The default preserves them.

Then repeat:

```text
R-002
    → reconcile with drop_blank_rows
    → agree the clean_csv seam
    → one failing test
    → minimum coherent implementation
    → green
    → affected prior tests
    → consistency review
```

The working interface remains a view of the whole intended capability. Its confirmed surface grows one requirement–test pair at a time.

Do not write an entire speculative test suite first. Each completed slice should teach the next one.

---

## 5. Scale the flow across contexts

### Small, clear work

Remain in the current context:

```text
/grill-with-docs → agreed interface and seam → /implement
```

### Multi-session build

Use:

- [`/to-spec`](../skills/engineering/to-spec/SKILL.md) to synthesize the already-settled conversation. It does not conduct another interview.
- [`/to-tickets`](../skills/engineering/to-tickets/SKILL.md) to create narrow but complete tracer-bullet tickets with explicit blocking edges. The user approves their granularity and dependencies before publication.
- `/implement` once per unblocked ticket, each in a fresh context.

```text
/grill-with-docs
    → working interface and settled decisions
    → /to-spec
    → /to-tickets
    → fresh /implement session per ticket
```

Keep grilling, specification, and ticket creation in one unbroken context when possible. Clear context between implementation tickets so each agent works from the ticket rather than accumulated conversational noise.

**Toy example**

```text
/to-spec

Synthesize our settled CSV-cleaner discussion. Preserve the working
interface, confirmed requirements, testing seam, provisional decisions,
and out-of-scope items. Do not interview me again.
```

```text
/to-tickets

Break the approved CSV-cleaner spec into vertical tracer-bullet tickets.
Each ticket must deliver observable behavior through clean_csv or the
CLI adapter, declare its blocking edges, and fit one fresh context.
```

### Huge, foggy effort

Use [`/wayfinder`](../skills/engineering/wayfinder/SKILL.md) when the destination is visible but the route cannot fit coherently in one session.

Wayfinder creates and resolves decision tickets, not implementation slices. When the way is clear, hand the resulting map to `/to-spec`; do not jump directly from an unresolved decision map into implementation unless the effort has turned out genuinely small.

### Crossing a session boundary

Use [`/handoff`](../skills/productivity/handoff/SKILL.md) when a fresh session needs the current decisions. It writes a redacted handoff in the operating system's temporary directory, includes suggested skills, and references existing specs, ADRs, tickets, commits, and tests instead of duplicating them.

---

## 6. Review, verify, and commit

**Applicable skills**

- [`/code-review`](../skills/engineering/code-review/SKILL.md) reviews the diff from a fixed point along two independent axes:
  - **Spec** — did the change implement the originating requirement or spec without omissions or scope creep?
  - **Standards** — does the change follow repository standards and avoid material design smells?
- `/implement` invokes this review before committing.

If no fixed point is supplied, ask for one. Keep the Spec and Standards reports separate so one axis cannot mask the other.

```text
/code-review main

Review the CSV-cleaner diff against main. Use the approved feature
contract as the Spec source and the repository's documented standards
for the Standards axis.
```

Completion order:

```text
implementation green
    ↓
targeted checks and full test suite
    ↓
two-axis code review
    ↓
resolve material findings
    ↓
rerun affected checks and final verification
    ↓
commit
```

For a Python CLI or package, final verification normally includes:

- all tests;
- type checking;
- linting and formatting checks;
- package build;
- clean-environment installation;
- import smoke test;
- installed CLI smoke test;
- documentation examples where practical.

The agent reports commands and outcomes. “It looks done” is not evidence.

---

## 7. Defects re-enter through a feedback loop

**Applicable skills**

- [`/diagnosing-bugs`](../skills/engineering/diagnosing-bugs/SKILL.md) owns hard bugs and performance regressions.
- `/domain-modeling` is used when the defect exposes incorrect domain language.
- `/codebase-design` and `/improve-codebase-architecture` apply when no correct testing seam exists.

Do not start with a theory. Start with one tight, red-capable command that detects the user's exact symptom.

```text
tight, already-run red-capable command
    → reproduce
    → minimize
    → rank 3–5 falsifiable hypotheses
    → instrument one prediction at a time
    → regression test at the correct seam
    → fix
    → rerun the regression test and original unminimized signal
    → cleanup and post-mortem
```

```text
/diagnosing-bugs

The csv-clean CLI corrupts embedded newlines in quoted cells. First
build one fast command that reproduces that exact symptom. Do not form
hypotheses until the command is demonstrably red-capable.
```

A defect may reveal:

- an implementation defect;
- a missing or incorrect requirement;
- an interface defect;
- an environmental or dependency problem;
- an architectural seam problem.

If the requirement or interface is wrong, return through the consistency loop. If no correct seam can reproduce the real bug, record that finding and hand the architectural problem to `/improve-codebase-architecture` after the immediate defect is resolved.

---

## 8. Minimal artifact model

Keep one source of truth for each kind of knowledge.

| Artifact | Purpose |
|---|---|
| **Working feature contract or spec** | Whole interface draft, confirmed requirements, provisional decisions, open questions, and out-of-scope items. |
| **Tests** | Executable evidence for confirmed behavior. |
| **Tickets** | Vertical deliverables, acceptance criteria, status, and blocking edges for multi-session work. |
| **`CONTEXT.md`** | Domain glossary only. |
| **ADRs** | Hard-to-reverse, surprising decisions produced by real trade-offs. |
| **Code** | The implementation, not the authority on intended behavior. |

Do not create a separate behavior ledger that duplicates the spec, tickets, and tests. If a status view is useful, derive it from those artifacts or keep status beside the canonical requirement.

A compact feature contract may use:

```markdown
# CSV Cleaner

## Working interface

## Confirmed requirements

## Provisional decisions and open questions

## Testing seam

## Out of scope
```

---

## 9. Completion checklist

A Python module or CLI is complete when all applicable statements are true:

- [ ] The working interface matches the implemented behavior.
- [ ] Every implemented requirement has appropriate executable evidence.
- [ ] No confirmed requirement or compatibility promise changed silently.
- [ ] Relevant prior tests and the full suite pass.
- [ ] Type checking and repository quality checks pass.
- [ ] Spec and Standards review findings are resolved or explicitly accepted.
- [ ] Package build, clean installation, and installed smoke tests pass when applicable.
- [ ] Domain terminology and qualifying ADRs are current.
- [ ] Known limitations and deliberate deferrals are visible.
- [ ] The diff contains no unrelated work.
- [ ] Verification commands and outcomes are recorded.

---

## 10. Quick reference

```text
Route the work
    ↓
Clarify intent one decision at a time
    ↓
Draft the whole interface at useful resolution
    ↓
Choose one capability
    ↓
Confirm one requirement
    ↓
Reconcile it with the interface
    ↓
Agree the public testing seam
    ↓
Write and run one failing unit test
    ↓
Implement the minimum coherent behavior
    ↓
Obtain green and run affected prior tests
    ↓
Review interface ↔ requirement ↔ tests
    ↓
Repeat one capability at a time
    ↓
Run full verification
    ↓
Review Spec and Standards
    ↓
Resolve findings, verify again, and commit
```

The reusable principle is:

> Begin with a provisional interface for the whole intended capability. Implement one observable behavior at a time. For each slice, confirm the requirement, reconcile it with the interface, test it through an agreed public seam, and implement only enough coherent behavior to pass. Afterward, review the interface, requirement, new test, prior tests, and affected callers together. Discoveries may revise the working draft, but no confirmed behavior or compatibility promise changes silently.
