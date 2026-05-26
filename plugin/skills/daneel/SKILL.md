---
name: daneel
description: This skill should be used when the user invokes DANEEL on a debugging task — by saying "daneel", "use daneel", "debug X", "investigate why X is wrong", "find the root cause of Y", or requesting a structured investigate-design / implement / verify pass on observed wrong behavior.
license: MIT
---

# DANEEL

DANEEL conducts a debugging task through three phases —
investigate-design, implement, verify — holding their
transitions, honoring loopbacks, and surfacing anything it
cannot resolve. The implement phase has two paths: simple fixes
complete inline; complex fixes hand off to Clippy.

## Load this now

- [ ] `references/foundations.md`, `references/tracker.md`,
  `references/lenses.md`, and `references/debugging-disciplines.md`
  (paths relative to this skill's directory) all read this
  session?
  - NO → CANNOT proceed. Read each now.
  - YES → Evidence: name the four files.

## Run start

Detect the run's mode. A run is **interactive** by default; it
is **auto-battle** only when auto-battle is explicitly requested
at invocation. Then locate the run's tracker (see Run lifecycle)
and enter the run's phase — investigate-design for a fresh run,
the saved phase for a resumed one.

**Tracker is the source of truth for run state.** When the
harness provides task-tracking tools (TaskCreate, TaskUpdate, and
similar), they are not used for DANEEL run-state tracking — the
tracker file under `.daneel/runs/` is the source of truth.
Harness task tools may be used for harness-level work outside
the DANEEL protocol, but the tracker carries every finding,
hypothesis, design decision, and status that the protocol
depends on.

## The pipeline

A run advances through three phases in order —
investigate-design → implement → verify. Each phase has its own
procedure file; on entering a phase, load and follow it:

- investigate-design → `phases/investigate-design.md`
- implement → `phases/implement.md` — two paths: simple
  fixes complete inline; complex fixes hand off to Clippy via
  cross-instance handoff
- verify → `phases/verify.md` — conducted in isolation;
  runs only on the simple-fix path

Enter a phase only when its predecessor has reported completion.
The investigate-design → implement transition is [READY]
(`phases/investigate-design.md`): investigate-design ends at
[READY] when the working context judges the investigation
complete (root cause established, fix approach selected) and
presents it, and the operator, presented the result, decides to
proceed. The implement → verify transition is not gated on the
simple-fix path — verify is itself the check on implement. On
the complex-fix path, DANEEL's implement ends when the handoff
artifact is produced and Clippy is invoked; Clippy's verify
governs the fix's verification.

the impl plan (`phases/implement.md`) is produced at [READY]
presentation so the operator sees the unit list at the decision
moment: dispatch units derived from the locked fix,
dependency-ordered, parallel-eligibility marked with a
search-established
disjointness basis (`references/foundations.md`). For DANEEL's
simple-fix path, the plan typically has one unit (the fix
itself). When the plan contains two or more units, dispatch
each unit's work to a **subagent** isolated from the run's
working context — brief it to load this skill's
`references/foundations.md`, `references/tracker.md`, and
`phases/implement.md`, and provide the tracker (default: full;
reduction requires a cited cause per the basis rule). The
subagent implements the in-scope decisions; before returning
state, it self-checks its diff against the locked fix using
**Coupled-change**, **Failure-path** (`references/lenses.md`) —
findings returned with state as fixed-shape ledger lines (see
`phases/implement.md`, Self-check at dispatch boundary). The
subagent does not design.

The orchestrator owns the tracker append: a dispatched subagent
returns its state on completion or halt — findings, the unit's
commit SHA, a loopback signal where applicable — and the
orchestrator appends in deterministic order, preserving the
append-only model (`references/tracker.md`). A subagent finding
major new scope halts and returns a loopback-required result
with four fields (trigger / scope / basis / affected_decisions);
the orchestrator then halts other parallel subagents in flight,
audits the work-state at halt per `phases/implement.md` (parallel-
completed units preserve or revert by scope-disjointness audit
against the new finding; uncommitted working-tree changes preserve
for redo inheritance), preserves tracker state, and returns the
run to investigate-design with the four-field result feeding the
new cycle.

verify is conducted in **isolation** from the run's working
context. The agent that wrote the fix does not check it. At the
implement → verify transition (simple-fix path only), establish
the isolated context (in DANEEL, by spawning a subagent) and
brief it to load this skill's `references/foundations.md`,
`references/tracker.md`, `references/lenses.md`, and
`phases/verify.md`, then conduct verify per `verify.md` against
the run's tracker, the produced code, and the originally-observed
failure case — recording its findings to the tracker. It reports
[PASSED] or [ISSUES FOUND]; honor the loopback below. The
isolation is the load-bearing property and is unconditional —
not a judgment made per task; the subagent-spawn is DANEEL's
mechanism for achieving it.

Record verify's result with the context it was conducted in —
isolated, or without isolation — so a [PASSED] carries whether
the check was independent.

## Loopbacks

A phase may return the run to an earlier phase. Honor the
return rather than proceeding:

- implement returns to investigate-design when major new scope
  surfaces during implementation.
- an [INVALIDATED] finding or hypothesis verdict reopens
  investigation work.
- verify ending [ISSUES FOUND] returns the run to
  investigate-design — the single locus for fix resolution. The
  fix runs through the full procedure (investigate-design →
  implement → verify); no in-place shortcut at verify-terminal.

## Modes

In **interactive** mode the operator advances the run. After
each investigate-design cycle and at each phase boundary,
present a **closed artifact** — the tracker (its findings,
recorded hypothesis verdicts, root cause classification, fix
approach), a recommendation, and the menu — **and nothing
else**. Do not pose design decisions to the operator as
questions or choices: they are recorded in the tracker as
committed decisions (`phases/investigate-design.md`,
`references/tracker.md`), never surfaced as a choice for the
operator to make. The **menu** carries only loop control — the
operator selects one option: continue the loop, or proceed to
the next phase. The operator's input on a design decision is
free-form override against the recorded tracker — never an
answer to a posed choice. Posing a binary or n-ary choice ("A
or B?") is the line: it constrains the operator's input to
selected options and is not permitted.

The menu **persists**. It is the last element of every
response until the operator selects continue or proceed. The
operator may interject free-form instead of selecting — a
question, a comment, an override; answer the question or apply
the override, **then** re-present the menu as the last element
of that same response.

**Closed-artifact form.** Five required sections in fixed order
(state summary, inventories, persisted artifacts, recommendation,
menu) per `references/closed-artifact.md`. Load when preparing
the closed artifact at any cycle or phase boundary.

**Auto-battle is interactive minus the operator.** Every
protocol behavior — cycle loop, lens application, basis-rule
discipline, tracker shape, dispatch self-check, verify
isolation, [CONDITIONAL] handling, hypothesis-enumeration — is
identical to interactive. The single difference is the
operator slot at decision moments: in interactive, an operator
selects continue or proceed at the closed-artifact
presentation; in auto-battle, no operator is present, so the
AI's committed recommendation is taken as default
automatically.

In **auto-battle** mode the loop self-advances without
per-cycle operator input. Cycles run until the working context
judges the investigation complete ([READY],
`phases/investigate-design.md`). At [READY], where interactive
presents the closed artifact and waits for the operator's
proceed-selection, auto-battle skips the presentation and
proceeds directly — the same default-take of the AI's
recommendation, just without the operator-present-to-attest.

A [CONDITIONAL] decision still resting on its assumption when
the design reaches [READY] is recorded [AUTO-ACCEPTED]
(`references/tracker.md`) in both modes. Every [AUTO-ACCEPTED]
decision is, by its tag, surfaced in the tracker for the
operator's review of the completed run — never silently.

Verify [ISSUES FOUND] in auto-battle triggers automatic loopback
to investigate-design (Loopbacks above). The convergence
exception: a verify finding whose evidence field explicitly
cross-references an existing [AUTO-ACCEPTED] design decision by
its tracker ID — declaring the finding observes the same gap the
decision deferred (in the code, or in the failure-case re-run) —
does not trigger loopback. Append it to the tracker as a
re-surfacing notation alongside the original [AUTO-ACCEPTED] tag,
and complete the run; the AI's prior judgment to defer is
preserved for the operator's post-run review. Other halt
conditions — phases that genuinely cannot complete on causes
other than [ISSUES FOUND] — remain a separate, not-yet-undertaken
effort.

## Run lifecycle

A run starts at investigate-design and ends when verify reports
[PASSED] (simple-fix path) or when Clippy completes the handoff
fix (complex-fix path). Each run has its own tracker — an
append-only ledger file under `.daneel/runs/`
(`references/tracker.md`); a new entry or a status change
appends a line rather than rewriting, and a run never
overwrites another run's tracker.

At run start, scan `.daneel/runs/` for an **in-progress** run —
a tracker whose run has not reached terminal:

- an in-progress run for the current debugging task → resume it
  from its tracker and phase rather than restarting.
- an in-progress run for a different task → surface it to the
  operator (a prior run left unfinished), then start a fresh
  run for the current task.
- none in progress → start a fresh run.

A fresh run opens a new tracker file under `.daneel/runs/`.
Completed runs' tracker files are kept — `.daneel/runs/` is the
project's accumulated DANEEL history; nothing is overwritten.
On first creating `.daneel/`, add it to the repository's
`.gitignore` (creating that file if absent) and note the
change.

## Post-run review (optional)

After verify reports [PASSED] (simple-fix path) or after Clippy
completes the handoff fix (complex-fix path), the operator may
request a **post-run review** to surface what the protocol
missed or got wrong this run — a free-text instruction like
"post-run review" or "review the run." Do **not** prompt for
it; the operator decides when a run warrants the analysis.

On request, load `references/post-run-review.md` and conduct
the review per that procedure. Output goes to the operator in
the session — do **not** persist the review or its findings to
a file in the project.

## Halt and surface

When the run cannot advance — a phase cannot complete — halt
the run and surface the reason; do not advance past the
incomplete phase. A decision that needs the operator is not
such a case: in interactive mode it is held [CONDITIONAL]
until the operator resolves it; in auto-battle it becomes
[AUTO-ACCEPTED] and the run proceeds (see **Modes**).
