# implement

The second phase of a DANEEL run: applies the locked fix recorded
in the tracker, producing the corrected behavior. Two paths:
**simple fixes** complete inline; **complex fixes** hand off to
Clippy's implement phase via cross-instance handoff.

## Path classification (locked at [READY])

The fix approach decision (`tracker.md`, design-decision track)
records which path applies. Classification is mechanical, not
naked-judgment:

- **Simple fix** (stay in DANEEL): 1-3 components affected;
  logic correction, state fix, or parameter adjustment; no
  architectural change; no new patterns or components; impact
  trace shows SAFE (per `references/debugging-disciplines.md`
  D6.3).

- **Complex fix** (hand off to Clippy): any of the following
  triggers handoff —
  1. Touches a file or contract not in the locked fix's listed
     scope (>3 components reached during impact trace)
  2. Requires new patterns or components
  3. Pattern-repetition audit found multiple instances requiring
     coordinated change
  4. Impact trace shows REQUIRES_COORDINATION (architectural
     change)

A fix classified as complex transitions to Clippy via the handoff
below; a fix classified as simple proceeds with the inline
implementation.

## Simple fix path

The locked fix is the authority. The existing code and surrounding
conventions are context, not authority — where they diverge from
the locked fix, the fix governs.

Discovery during implement is minimal. A new finding during
implementation is **major new scope** (and triggers loopback) if
any of:

1. Touches a file or contract not in the unit's listed scope
2. Changes a locked contract's signature, types, or error path
3. Introduces a new design decision (per `tracker.md`)
4. Crosses a sibling unit's scope (breaks the disjointness basis)

Otherwise it is a **local clarification** — recorded in the
tracker; the unit proceeds. No work is lost when loopback fires:
the tracker carries the state, and the run re-enters the
investigate-design cycle loop.

### The impl plan

implement opens with an impl plan: the locked fix's components
grouped into **dispatch units**, dependency-ordered, with a
parallel-eligibility marker on each. For DANEEL's simple-fix path
the plan typically has one unit (the fix itself). A dispatch
unit's entry cites the [VERIFIED] decisions it implements by
their tracker identifiers. The impl plan is a planning artifact,
not a new tracker construct.

Parallel-eligibility is a load-bearing claim per the basis rule
(`foundations.md`): a unit's file and contract scopes are listed
and the disjointness from sibling units' scopes is
search-established. A unit whose disjointness is not
search-established is sequential.

The plan is persisted alongside the tracker — a phase-start run
artifact at `.daneel/runs/` — kept for the run's history and for
resume.

### Dispatch

When the impl plan has two or more dispatch units, each unit's
work is dispatched to a subagent isolated from the run's working
context; a single-unit plan (typical for DANEEL simple fixes) is
implemented in the working context.

The subagent is briefed artifact-driven, mirroring verify
(`verify.md`): brief it to load this skill's `references/
foundations.md`, `references/tracker.md`, and this file; provide
the tracker (default: full; reduction to the unit's in-scope
decisions is permitted only with a cited cause — e.g., tracker
size exceeds the subagent's context budget — recorded as the
basis for the reduction per the basis rule (`foundations.md`))
and the locked contracts the unit honors. The subagent
implements the in-scope decisions; it does not design — major
new scope halts it (below).

### Self-check at dispatch boundary

Before returning state, the dispatched subagent (and the working
context, for a single-unit plan) applies the standardized lenses
most relevant to write-time issues — **Coupled-change**,
**Failure-path** (`lenses.md`) — to its diff against the unit's
in-scope locked design decisions. Findings are entered as
fixed-shape ledger lines (`tracker.md`) and returned with state;
the orchestrator appends per the Tracker writes discipline
below.

The self-check compounds with the design-time forcing function
for delete/replace/amend decisions (`foundations.md`, True-unit
basis): the basis enumerates references as of [READY]; the
self-check catches references and behaviors introduced
post-design.

A self-check finding of major-new-scope shape triggers the
loopback (below); an in-scope concern is appended for verify's
later sweep but does not halt the unit.

### Tracker writes

The orchestrator owns the tracker append. A dispatched subagent
does not write directly; on completion or halt it returns
state — findings, the unit's commit SHA, a loopback signal where
applicable — and the orchestrator appends in deterministic
order. The append-only model (`tracker.md`) is preserved without
concurrency machinery.

### Loopback across the subagent boundary

A subagent finding major new scope halts and returns a
loopback-required result with four fields:

- **trigger** — the code site or signal the subagent encountered
- **scope** — what's outside the locked fix (specific element,
  contract, or behavior)
- **basis** — the re-runnable search or read that surfaced it
  (per the basis rule, `foundations.md`)
- **affected_decisions** — locked design decisions invalidated
  or extended (cited by tracker identifier)

On receiving it, the orchestrator halts other in-flight parallel
subagents — required because their work may rest on the
disjoint-scope claim the new finding contradicts — preserves
committed work and tracker state, and returns the run to
investigate-design with the four-field result feeding the new
cycle.

If investigate-design's new cycle reclassifies the fix as
complex (per the path classification above), the next [READY]
transition hands off to Clippy.

### Checkpoint

A dispatch unit's code is committed to git on completion; the
orchestrator appends the commit SHA to the tracker. The commit
plus tracker line is the unit's persistence artifact: a run
interrupted mid-implement resumes from the tracker — the
last-completed unit, the next per the impl plan (see `SKILL.md`,
Run lifecycle).

### Completion

Report completion when every unit in the impl plan is completed.
The implement → verify transition is not gated: verify is itself
the check on implement, so an incomplete implementation surfaces
as verify findings — it does not pass silently.

## Complex fix path (handoff to Clippy)

When the fix approach is classified as complex (per the path
classification at [READY]), DANEEL hands off to Clippy's
implement phase rather than implementing inline.

### Handoff artifact

The handoff is a structured return passed to Clippy as its
investigate-design starting input:

- **root_cause** — the [VERIFIED] root cause from DANEEL's
  tracker (the surviving hypothesis with its basis)
- **fix_approach** — the [VERIFIED] fix recommendation from
  DANEEL's tracker (with its basis)
- **scope** — the file and contract scope identified in DANEEL's
  impact trace (`references/debugging-disciplines.md` D6.3)
- **pattern_instances** — if the root cause is architectural,
  the pattern-repetition audit results (`debugging-disciplines.md`
  D6.4): the enumeration of all instances and which are SAME BUG
  vs DIFFERENT CONTEXT
- **failure_case** — the concrete example from
  investigate-design that demonstrates the wrong behavior, used
  for verify to confirm the fix

Clippy receives this handoff as its task input. Clippy's
investigate-design starts with these as design premises (basis:
DANEEL's tracker artifacts); Clippy designs the implementation
plan and proceeds through its own implement and verify phases.

### Cross-instance loopback

If Clippy's investigate-design surfaces evidence that DANEEL's
root cause classification was incorrect (a [VERIFIED] root cause
[INVALIDATED] by new evidence in Clippy's investigation),
Clippy halts and surfaces the contradiction to the operator. The
operator decides whether to re-open DANEEL's investigation or
proceed with Clippy under a revised root cause.

### Completion (handoff path)

DANEEL's implement phase reports completion when the handoff
artifact is produced and Clippy is invoked. DANEEL's verify
phase does not run on the handoff path — Clippy's verify
governs.
