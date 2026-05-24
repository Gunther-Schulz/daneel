# Foundations

The verification discipline the phases rest on. DANEEL loads this
reference at activation, before any phase runs.

## The basis rule

Every load-bearing claim and every hypothesis carries a named
**basis** — the evidence it rests on: a grep result, a file:line
read, a state observation (database query, log entry, execution
trace), a behavioral test result. A free-text claim of having
looked is not a basis.

A basis that resolves to recall — "assumed," "inferred,"
"obviously so" — is an **assumption**, not a basis. An assumption
does not ground a verdict; it holds the run short of [READY] until
converted to evidence or the operator resolves it. The mechanical
tell of a blind spot is the basis the AI cannot produce.

**Load-bearing** means a claim or premise a hypothesis verdict, a
root-cause classification, a fix recommendation, or [READY] rests
on. A claim of bounded, contained cost does not carry the basis
apparatus.

The rule rejects **silent substitution**: missing or malformed
evidence defaulted to a plausible proxy that propagates as if it
were the basis. Surface the gap, do not substitute.

The rule has two edges — basis-naming and true-unit basis.

### Basis-naming

Every load-bearing claim and every hypothesis names its basis or
stands as an assumption. This reaches **investigation
premises**, not findings alone — such as the premise that an
observed wrong behavior originates in a specific component. A
premise with no named basis is an assumption and holds the run
short of [READY] until grounded.

Design-decision premises embed implicit claims: a decision
naming a target (e.g., path, filename, module, new helper
module for the fix) or asserting a completeness count (e.g.,
"all instances of this pattern are X") carries an implicit
premise about that target's current state. The implicit premise
is load-bearing; its basis is a re-runnable read or grep at
decision-lock time, not at recall. A target-naming or count
claim with no negative-evidence basis is missing its true-unit
basis and cannot reach [VERIFIED].

### True-unit basis

A basis must cover the claim's true unit, not a coarser proxy.

- A claim about a **complete set** (the set of components in a
  failure's call path, a symbol's consumers, an input's value
  classes, the instances of a failure pattern) has the whole set
  as its unit. Its basis is a repo-wide search recorded as the
  **re-runnable search itself** — the grep command, not the count
  it returned. A search narrowed by where the members are assumed
  to live (to other files, leaving out the file containing the
  observed wrong behavior) is a declared scope wearing a search's
  clothing — including a search of "everywhere else" that excludes
  the failure site, where co-located causes hide.
- A claim about a **construct** (a call, a statement, a function,
  a state value at a point in execution) has the whole construct
  as its unit. Its basis is a read to the construct's visible
  close, not a window that catches part of it.

A hypothesis claiming a **complete cause set** (all places where a
failure pattern could originate) carries an implicit completeness
claim: that all candidate origins have been enumerated and
evaluated. Its basis must include the re-runnable search
enumerating candidates AND the elimination evidence for each. A
hypothesis whose basis is just "X is the cause" without the
enumeration and elimination is missing its true-unit basis; it
cannot reach [VERIFIED]. This is the forcing function for the
Hypothesis-enumeration lens (`lenses.md`) — the lens's question
becomes load-bearing on the hypothesis's own basis artifact, not
on a separate cycle's standardized pass alone.

### Secondary sources

A sub-agent report or a prior session's notes is not itself a
basis. A direct citation it carries — a file:line with the
verbatim content, an execution trace excerpt — relays the artifact
and can stand as a basis. Its interpretation, synthesis, or
recommendation cannot: re-ground those against the actual code or
runtime state before anything rests on them.

## Evidence-bearing artifacts

Every load-bearing artifact the protocol requires — a mechanism's
output, and a recorded hypothesis verdict — must be
**evidence-bearing**: producing it requires doing the work it
represents, so a non-adherent AI cannot produce it by pattern
alone. An enumeration — "candidates eliminated: [list with
evidence each]" — requires having investigated. A bare claim —
"checked all candidates" — is satisfiable whether or not the
investigation happened.

Evidence-bearing is a gradient, not an absolute — no artifact an
AI produces is un-fakeable, since it can fabricate one. An
artifact's strength is how far faking it requires active
fabrication rather than mere omission, and how cheaply a checker
catches the fake. A **strong** artifact points at external,
re-checkable truth — a grep result, a file:line read, a test or
execution-trace output: faking it means fabricating something a
checker re-runs and catches. A **weak** artifact is a claim about
the run's own state — a status tag, a self-assessment that
elimination is complete: there is no external truth to check it
against.

- An inspection's finding, or its cited reason that a lens is
  clean, must cite evidence that required looking.
- A hypothesis verdict's artifact is its [VERIFIED]/[INVALIDATED]
  status and its basis (`tracker.md`). An untested hypothesis is
  not a verdict — so the design-decision track holds it as
  [PENDING] or [CONDITIONAL], not [VERIFIED].

A weak artifact is not self-enforcing — the protocol cannot rest
on the artifact alone. It is enforced by a **separate checker**: a
context that did not produce the artifact re-derives it, or the
operator inspects it. The artifact still earns its place — it
makes faking require fabrication, and gives the checker something
concrete to check — but the guarantee comes from the checker, not
the artifact.

This rule reaches DANEEL's behavioral rules, not its mechanisms
alone: a rule whose adherence cannot be read off an artifact is
not enforced. Following a load-bearing rule must produce an
artifact; not following it must produce none. An artifact a
non-adherent AI can produce by pattern alone enforces nothing.

## Recommendation discipline

Every fix recommendation the AI commits — every closed-artifact
fix proposal, every menu's next-step, every [CONDITIONAL]
default-take — defaults to the **thorough-fix shape**: the option
that addresses the root cause at its actual scope, not pre-clipped
on perceived cost. Cost is the operator's judgment, surfaced via
free-form override (interactive) or visible in the [AUTO-ACCEPTED]
tag at post-run review (auto-battle).

This is the structural enforcement on what goes IN every recorded
fix recommendation (`tracker.md`) and every recommendation in the
closed artifact (`SKILL.md`): the artifact's content is
thorough-fix-shaped, not cost-clipped. A cost-clipped
recommendation is a malformed artifact, catchable at operator
review (interactive) or at the Thorough-fix-default lens
application (`lenses.md`).

For DANEEL specifically: a "fix the symptom not the root cause"
recommendation is a thorough-fix violation — the fix scope is the
root cause, not the surface error. A pattern-repetition audit
finding multiple instances of the same root cause widens the fix
scope to all instances; fixing only the originally-observed
instance is a thorough-fix violation.

## Inspection

DANEEL's one reusable mechanism is **inspection**: it looks
through a lens at the code, the runtime state, and the work
produced so far, and yields a finding when the lens catches
something, nothing when it does not. Its lens is ad-hoc — derived
from the investigation target — or standardized: the pre-written
lens set in `lenses.md`.

The run's phase transitions — investigate-design's end at [READY],
the loopbacks — are not mechanisms of this kind. They are not a
mechanical check on accumulated state; they are governed by the
phase procedures and the run pipeline (`SKILL.md`), where what
each transition requires is specified directly.
