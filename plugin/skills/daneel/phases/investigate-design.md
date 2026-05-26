# investigate-design

The first phase of a DANEEL run: a loop of cycles that builds
understanding of the observed wrong behavior, enumerates and
eliminates candidate root causes, and produces a locked root
cause + fix approach. It ends at [READY].

## The cycle

investigate-design runs as a loop of cycles. Each cycle has two
passes, in order:

1. **Investigation pass.** Investigate the surfaces the wrong
   behavior touches — ad-hoc, by a method derived from the
   target, not a prescribed one. The standardized lens set
   (`lenses.md`) is known going in and informs what to attend
   to, not how to investigate. Record every observation as a
   finding (verification-map entry) in the tracker.
2. **Standardized inspection pass.** Apply each standardized
   lens whose scope this cycle's investigation touched —
   incremental, over what this cycle produced. Emit the
   standardized-pass artifact (`tracker.md`): one line for each
   in-scope lens — a finding, or a one-line cited reason it is
   clean. A lens out of scope this cycle is not lined; the set
   is not re-attested every cycle — it is accounted for whole
   once, at [READY].

The standardized inspection pass runs every cycle — not only the
first, not only when something feels off.

**Cycle numbering** is continuous across the run. A loopback from
a downstream phase (implement major-new-scope, verify [ISSUES
FOUND]) returns to investigate-design at the next cycle number,
not at cycle 1. Each cycle number is unique within the run.

## Hypothesis enumeration

When a symptom is identified (an observable wrong behavior with
expected vs actual divergence located), DANEEL's
**hypothesis-enumeration** lens (`lenses.md`) requires the
complete hypothesis list be generated BEFORE investigating
mechanisms. Each candidate root cause is recorded as a design
decision in the tracker:

- [OUTLINED] for hypotheses named but not yet investigated
- Status moves toward [VERIFIED] (this IS the root cause) or
  [INVALIDATED] (eliminated) through evidence

A cycle that locks a hypothesis verdict ([VERIFIED] /
[INVALIDATED]) does so on evidence per the basis rule
(`foundations.md`). A theory ("might be," "could be") triggers
the **theory-resolution** lens: stop investigation, add the
theory to the hypothesis list as [OUTLINED], surface for next
target.

The investigation order is governed by the **source-before-result**
and **origin-not-symptom** lenses: verify state at the source
before investigating downstream transformations; trace backward
to where wrongness enters, not investigate where the symptom
surfaces.

## Design decisions

Across the cycle, form and update design decisions
(`tracker.md`) from the cycle's findings. For DANEEL, design
decisions include:

- **Hypothesis verdicts** — candidate root causes with their
  status (eliminated / verified / conditional)
- **Root-cause classification** — once the surviving hypothesis
  is locked, classify per `references/debugging-disciplines.md`
  D6.1 (incorrect implementation / invalid state / component
  mismatch / missing operation / wrong model)
- **Fix approach** — the committed fix recommendation
  (thorough-fix-shaped per `foundations.md` Recommendation
  discipline) and its scope (simple → stay in DANEEL, complex →
  hand off to Clippy per `phases/implement.md`)
- **Pattern-repetition** — for architectural root causes, the
  enumeration of all instances and which are SAME BUG vs
  DIFFERENT CONTEXT

The locked design is the body of these decisions; it locks as
each reaches [VERIFIED] or [AUTO-ACCEPTED].

Every design decision is **self-resolved**. For each one:

- [ ] Is this decision recorded in the tracker as a committed
  resolution — a chosen direction with its basis
  (`foundations.md`)?
  - NO → CANNOT proceed. A design decision is a committed
    position, never an open question and never a choice posed to
    the operator. A posed choice is the absence of a resolution
    — it yields no valid design-decision artifact, so the
    design-decision track cannot hold one. Resolve the decision:
    commit to a recommendation, record it with its basis.
  - YES → Evidence: the tracker entry, its committed resolution,
    and its basis.

Do not pose design decisions to the operator as choices to make.
A choice to defer, exclude, or not investigate further is itself
a committed decision — "defer X, because Y" is recorded with its
basis, not left as an absence.

A decision that rests on an assumption — including one only the
operator could confirm — is still committed, not posed: record
it [CONDITIONAL], the AI's committed recommendation carrying the
operator-resolvable assumption as its named basis
(`foundations.md`). At [READY], a [CONDITIONAL] decision still
resting on its assumption becomes [AUTO-ACCEPTED] (`tracker.md`).

## Scope

The investigation scope — the set of components and surfaces in
scope to verify — is the foundational design decision.
Establish it first; every other decision is investigated within
it. By the basis rule (`foundations.md`) the scope is a
completeness claim: its basis is a repo-wide search of the
failure surface (call paths leading to the wrong behavior,
components in the symptom's domain), not a recalled model. It
reaches [VERIFIED] only when search-established. When a later
cycle grows the failure surface, the scope decision re-opens and
is re-searched. Because [READY] requires every design decision
[VERIFIED] or [AUTO-ACCEPTED], an unestablished scope holds the
phase at [NOT READY].

## [READY]

### Judgment criteria

[READY] is reached when the working context judges the
investigation complete — root cause identified, fix approach
selected, every concern resolved, every design decision at its
terminal, and the last cycle's standardized inspection pass
producing no load-bearing finding. The supporting facts are recorded
across the run:

- **Standardized coverage** — the standardized lens set is
  accounted for whole: every lens applied in the cycle(s) where
  its scope was touched, or carrying a cited reason it was out
  of scope for the run; no lens silently absent; the last
  cycle's pass left no load-bearing finding.
- **Tracker state** — no finding is [INVALIDATED], no
  load-bearing finding is below [VERIFIED], every design
  decision is [VERIFIED] or [AUTO-ACCEPTED] (`tracker.md`),
  exactly one hypothesis is [VERIFIED] (the established root
  cause) with all others [INVALIDATED] or [AUTO-ACCEPTED]
  (per the hypothesis-enumeration discipline), and every
  [VERIFIED] design decision's embedded target-naming and count
  premises carry a re-runnable basis (per
  `references/foundations.md` basis-naming for design-decision
  premises).

These are the status the tracker carries — a notebook of where
each concern stands — that the AI reads when it judges the
investigation complete. They are not gate-conditions a separate
evaluation re-derives.

The supporting facts above check that everything *recorded* is
at terminal. The judgment must also test whether the recording
is *complete*. The strongest single test is **fresh-session
implementability**: would a session with only the tracker (no
chat context, no current-session memory) implement the fix
without surfacing a new design decision? If not, another cycle
is warranted regardless of recorded statuses.

### Artifact-produced result line

**[READY]'s judgment is artifact-produced.** The fresh-session
implementability test produces a named result line in the closed
artifact at [READY] presentation: PASSED with **per-implementer-
step external evidence** — for each step a fresh implementer
would take to carry out the locked fix, cite the basis form per
the basis rule (`foundations.md`): verbatim content for a read;
executable query with output for a search. Or FAILED with the
specific gap identified. PASSED without per-step external
citation is a malformed artifact: the test answers from the
recall pool that wrote the design rather than from external
evidence, the failure shape that allows false-[READY]s (V-5 in
`diligence-framework/spec/validation-watch.md`). Without the
result line itself, the closed-artifact form
(`references/closed-artifact.md`) is also malformed and the
[READY] declaration unenforced. The result line is recorded in
the tracker for post-run review in both modes.

### Convergence cycle requirement

**[READY] requires a convergence cycle.** After the working
context judges the supporting facts above met and the
fresh-session implementability test produces a PASSED artifact,
the [READY] declaration requires one more cycle — a **convergence
cycle** — to produce **zero D-track deltas** (no new design
decisions or hypothesis verdicts, no amendments to existing
ones).

A convergence cycle is a full cycle (investigation pass +
standardized inspection pass), not a final lens application on
accumulated state. Its investigation pass must enumerate **new
surfaces investigated this cycle**, where each surface is
**new** by at least one of: (a) cites a file path not in any
prior cycle's artifact this run; (b) cites a grep query whose
query string differs verbatim from every prior cycle's; (c)
cites a file:line range with at least one line not covered by
any prior cycle's same-file citation. A convergence cycle that
produces no new-surface citations (only re-attestations of
prior surfaces) is malformed.

If the convergence cycle surfaces D-track deltas, the design is
not [READY]: the deltas feed into the next cycle and the loop
continues. [READY] is presented only after a convergence cycle is
observed clean. The convergence cycle's outputs (investigation
pass artifact + zero-D-delta status) form part of the [READY]
artifact alongside the fresh-session result line.

Per V-5 — the mechanism breaks the recall-pool failure shape that
allows false-[READY]s, by switching the working context from
self-assessment mode to fresh investigation mode. Fires in both
interactive and auto-battle.

### Cycle-another recommendation

The AI recommends cycle-another whenever the fresh-session
implementability test fails, or a lens in scope was not applied
in the cycle that touched its scope. Cost-asymmetry does not
enter the recommendation; per `foundations.md` Recommendation
discipline, the operator judges cost. **The recommendation's
justification enumerates observations** — open findings,
[PENDING] hypotheses, lens-applications still required,
fresh-session-implementability gaps — not costs (effort,
budget, time, "cheaper than"). A justification framed in cost
comparison is malformed.

At [READY] the AI does not certify itself ready: it presents the
investigation result — the tracker, the established root cause,
the recorded fix approach, the fresh-session-implementability
result line, and a recommendation — for the operator's
judgment. The operator's decision to proceed is the transition
to implement; until the operator proceeds, the phase continues
and the loop may run further cycles.

Until the working context judges the investigation complete, the
phase is [NOT READY] and the loop continues — another cycle.

## Advancing the loop

In **interactive** mode the operator advances the loop: after
each cycle, present the tracker — its findings and recorded
design decisions — with a recommendation, and the operator
selects from the menu (run another cycle, or proceed) or
overrides free-form. In **auto-battle** mode the loop
self-advances (`SKILL.md` Modes — auto-battle is interactive minus
the operator). In either mode
the operator, seeing the recorded decisions, retains free-form
override at any point.

In interactive mode the menu **persists**. It is the last
element of every response until the operator selects
run-another-cycle or proceed. When the operator interjects
free-form instead of selecting — a question, a comment, an
override — answer the question or apply the override, **then**
re-present the menu as the last element of that same response.
The operator never loses the advance choice.
