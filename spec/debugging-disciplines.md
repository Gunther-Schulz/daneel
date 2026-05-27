# DANEEL Specification — Debugging disciplines

DANEEL-specific elaboration of the Anneal framework's
investigate-design phase — the debugging-specific disciplines
the lens set (`lens-set.md`) enforces.

The framework's investigate-design phase
(`anneal-framework/spec/core.md` §4.1) is domain-general; this
document specifies the D-sections that DANEEL's investigation
runs through. Built on the DANEEL bindings (`bindings.md`).

---

## D2: Purpose understanding

Establish what the system is **supposed** to do before
investigating why it doesn't.

- **Purpose** — what the system operates on, what it produces,
  how it behaves
- **User goal** — what the user needs to accomplish
- **Correctness criteria** — what makes behavior correct vs
  incorrect (must be testable, not vague)
- **Constraints** — rules the system must satisfy (invariants,
  contracts)

If correctness criteria cannot be articulated, the run cannot
[READY]. A debug run with vague "it's broken" goal yields vague
investigations.

**Symptom vs root cause.** An explicit error message does NOT
mean the root cause is understood. Before accepting an error at
face value: WHY does this code exist? What assumption led to
this bug? Is this isolated or systemic? What's the correct
pattern?

**Causation verification.** When an error appears after your
changes, verify causation: check history (when did this last
work?), test isolation if feasible (revert and retest), ask the
user ("did this work before my changes?"). A pre-existing error
needs the foundation fixed first; building on broken foundation
wastes effort.

## D3: Ground truth establishment

**Source hierarchy** (priority order):

Direct observation > Specs > Working code > User observations >
Assumptions

When the AI has tools to inspect directly (read files, run
queries, check traces), it inspects rather than asks. Asking is
appropriate only when the source is outside what tools can
reach.

**Verify entire critical path before proceeding.** Reactive
debugging (fix → execute → next error → fix → execute → next
error) is the failure shape; analysis-finds-all is the
discipline. Map ALL components in the failure path, mark each
[PENDING] / [VERIFIED] / [INVALIDATED] in the verification map
(the F-track), then proceed.

**Verification map format** (rendered onto the F-track): each
component entry is a fixed-shape ledger line with status +
summary + basis. The map's implication: the problem is NOT in
[VERIFIED] components → it MUST be in [PENDING] components
between verified boundaries (or in components not yet
enumerated).

## D4: Domain modeling

For investigations where the wrong behavior involves logical
constraints rather than individual value correctness, document
the expected behavior model:

- **Entities** — what objects/concepts are involved, what
  properties they carry
- **Relationships** — constraints between entities (cardinality,
  ordering, dependency)
- **Logical rules** — invariants the combined state must satisfy

A failure can have all individual values correct (each
probability 0.1, 0.5, 0.6 is valid) but violate a constraint
(sum > 100% is impossible). The model surfaces these
constraint-level bugs that value-by-value inspection misses.

**Semantic validity** distinct from technical validity: a value
can be technically valid (correct type, in range) but
semantically invalid (incompatible units, lifecycle violation,
dangling reference). Both are bugs; only the semantic kind
needs the model to find.

## D5: Concrete example tracing

When abstract reasoning has produced "the system processes X
incorrectly," trace ONE specific case to find where it goes
wrong:

1. Pick ONE concrete instance with actual identifiers, values,
   inputs — not abstractions
2. Trace it through the system point by point (operation by
   operation, transformation by transformation)
3. At each point: document expected vs actual; mark MATCHES or
   DIVERGES
4. Stop at the FIRST divergence — that's where wrongness enters

**Format:**

```
CONCRETE CASE: [specific instance, real identifiers]
POINT 1 [system operation]: Expected [X] | Actual [Y] | MATCHES
POINT 2 [system operation]: Expected [X] | Actual [Y] | MATCHES
POINT 3 [system operation]: Expected [X] | Actual [Z] | DIVERGES
DIVERGENCE: Point 3 — [what was expected vs what happened]
```

The divergence point identifies the component to investigate
(transition that component's status to [INVALIDATED] in the
verification map; generate hypotheses for why this specific
transformation produces the wrong output).

## D6: Root cause analysis

Once the divergence point is identified, classify the root
cause and evaluate fix options.

### D6.1 Classification

Root-cause classes (one of):

- **Incorrect implementation** — code does the wrong thing
  given correct inputs and correct understanding
- **Invalid state** — code is correct but operates on state
  that shouldn't exist (race condition, broken invariant,
  dangling reference)
- **Component mismatch** — two components make incompatible
  assumptions; neither is wrong in isolation
- **Missing operation** — a required step is absent (no
  validation, no error handler, no cleanup)
- **Wrong model** — code implements a conceptual model that
  doesn't match reality (uses cached when fresh needed, uses
  primary when secondary needed)

Classification informs the fix shape and the
pattern-repetition audit (D6.4).

### D6.2 Fix-option evaluation

Present options with concrete trade-offs:

- **Option A**: [approach] — Pros / Cons / Scope
- **Option B**: [approach] — Pros / Cons / Scope
- **Recommendation**: [which and why], thorough-fix-shaped per
  Recommendation discipline (rendered into plugin
  `references/foundations.md`).

A "fix the symptom" option is not a valid recommendation; it
violates thorough-fix-default.

### D6.3 Impact trace

For any fix modifying existing code (not just adding new),
trace impact BEFORE implementing:

- **Dependents** — what code invokes this component? What data
  references its output? What constraints rely on current
  behavior? Search the codebase (per the basis rule's
  true-unit basis — re-runnable search, not recall).
- **Dependencies** — what must exist before this works? What
  initialization is required? What state must be valid?
- **Side effects** — what changes when you modify this? If
  changing identifiers → references; if changing contracts →
  invocations; if changing persistence → queries.

**Assessment** (one of):

- **SAFE** — no dependents, no contract change, no side
  effects → simple fix path
- **NEEDS_MIGRATION** — dependents exist but the change is
  backward-compatible or migrable
- **REQUIRES_COORDINATION** — dependents change with the fix,
  contract changes, or architectural shift → complex fix path
  (handoff to Clippy per `phases/implement.md`)

### D6.4 Pattern-repetition audit

For architectural / systemic root causes (NOT for isolated
typos or one-off mistakes), audit for repetition:

1. Identify the anti-pattern (what was used wrong?)
2. Search for all uses of the same pattern (basis-rule
   true-unit basis: re-runnable search)
3. Classify each instance: SAME BUG (root cause applies) vs
   DIFFERENT CONTEXT (the pattern is correct here)
4. Expand fix scope to all SAME BUG instances

Skipping this audit when the root cause is architectural is a
thorough-fix violation — the fix would address only the
originally-observed instance, leaving repeated bugs as
technical debt.

### D6.5 Verify logical constraints

After fix implementation, verify (per `phases/verify.md`):

- The concrete case from D5 now produces correct output
- Logical constraints (D4) now hold
- The user's correctness criteria (D2) are satisfied
- For architectural fixes: all SAME BUG instances are
  corrected

A fix that produces wrong output on the concrete case is not a
fix; verify returns [ISSUES FOUND] and the run re-enters
investigate-design.
