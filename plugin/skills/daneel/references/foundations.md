# Foundations

The verification discipline the phases rest on. DANEEL loads this
reference at activation, before any phase runs.

## The basis rule

Every load-bearing claim and every hypothesis carries a named
**basis** — the evidence it rests on: (a) a grep result with its
query, OR (b) a file:line range citation paired with **one
observable fact** about the cited range (count, identifier, type) that the citation grounds, OR (c) a state
observation (database query with its output, log entry, execution
trace) OR (d) a behavioral test result OR (e) for a **derived
figure** — a rate, ratio, average, or difference computed from
other evidence — the OPERATION over its inputs as well as the
inputs. Verify (`verify.md`) and
convergence cycles re-open citations to verify both the location
AND the observable fact. A free-text claim of having looked, a
paraphrase of what was read, or a summary without an observable
fact is not a basis.

A derived figure's cited inputs ground its terms, never the
figure: what the operation assumes about those inputs is part of
the basis too — a total divided by a time window is a rate only
where the window carried nothing but the measured work.

Disclosing the operation does not make it correct. "660 MB/s —
basis: 32.1 GB / 49.9 s model load" names its operation in full
and is still wrong, because elapsed carried every dequantization,
tensor conversion, pin and allocation the loader performed
between reads. So a rate attributed to a RESOURCE cites that
resource's own meter where one exists — the kernel's device
service time for storage, scheduler time for CPU, the engine's
own reported query time for a database. Bytes over elapsed
measures the whole operation's throughput, never the resource's.

A meter is not a substitute rate. Device-busy time sums
per-request service across parallel queues and can EXCEED
elapsed, so it does not divide into MB/s; it is a quantity
comparable BETWEEN runs reading the same bytes. Where no meter
exists, the figure is labeled for what it is — the operation's
throughput — and no subsystem is named.

That last clause is the load-bearing one. A rate naming a
subsystem is a claim about WHERE the time went, and it does not
fail by producing a wrong number — it fails by producing a wrong
SUBSYSTEM. "The disk is slow" points the next cycle at storage
while the cause sits above the device, and every measurement
taken afterwards is valid and irrelevant.

A basis that is a behavioral test result or an execution trace
carries HOW MANY observations it rests on. Otherwise a verdict
resting on one run and a verdict resting on six read identically,
and the confidence in the wording does not degrade with the
thinness of the evidence.

A basis that resolves to recall — "assumed," "inferred,"
"obviously so" — or to deferral — "will verify in cycle N,"
"impl-phase will produce," "TBD" — is an **assumption**, not a
basis. An assumption does not ground a verdict; it holds the run
short of [READY] until converted to evidence or the operator
resolves it. The mechanical tell of a blind spot is the basis the
AI cannot produce.

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

This rule also reaches **claims embedded within larger
statements** — implicit premises in target-naming decisions
(path/filename/module, new helper module for the fix), cited
rules or prior decisions, completeness counts asserted as facts
("all instances of this pattern are X"). Each embedded claim
carries the basis-rule requirement *separately* from the
surrounding statement: the surrounding claim's basis does not
cover the embedded one. An embedded claim with no separate
basis is an assumption and cannot reach [VERIFIED].

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

### Complete cause-set as completeness claim

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

A complete cause set spans **layers**, not only instances. An
enumeration exhaustive within one layer reads complete —
candidates named, eliminated on evidence, the discipline visibly
followed — while a whole class of cause was never populated. At
minimum the layers are: the system under test's own behaviour, its
environment, and **the instrument producing the evidence**.

The instrument is the layer that never nominates itself, because
it is what every other candidate is graded against. A wrong
instrument corrupts the list silently, and every elimination in it
stays valid and worthless: the candidates really were killed, by
evidence that was not measuring what it claimed. "The instrument
is lying" belongs in the set whenever the verdict rests on what
that instrument reports.

### Secondary sources

A sub-agent report or a prior session's notes is not itself a
basis. A direct citation it carries — a file:line range with an observable
fact, or an execution trace excerpt — relays the artifact and can
stand as a basis. Its interpretation, synthesis, or
recommendation cannot: re-ground those against the actual code or
runtime state before anything rests on them.

### Operator testimony

An operator's report of observed behavior enters the run as a
finding like any other, and carries in addition a **discharge
state** — REPRODUCED, CONTRADICTED, DEFERRED with its reason, or
OUTSTANDING. It is recorded in the finding's own `discharge:`
sub-line (`tracker.md`), which is where a later reader and the
[READY] check both find it.

Recording the observation discharges nothing. A finding can be
true, [VERIFIED], and never acted on, and a coherent run will not
disturb it: scope re-opens when the failure surface GROWS, and a
frame that was wrong from the start never grows.

**[READY] requires no operator-observation finding OUTSTANDING.**
A run does not close while something the operator reported has
never been acted on. This is the assumption rule at a second seam —
an assumption holds the run short of [READY] for want of evidence,
an outstanding observation for want of an ACT.

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
context that did not produce the artifact re-derives it (verify's
isolated subagent `phases/verify.md`, the convergence cycle per
`phases/investigate-design.md`). The artifact still earns its
place — it makes faking require fabrication, and gives the
checker something concrete to check — but the guarantee comes
from the checker, not the artifact.

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
on perceived cost. Cost is the operator's judgment (`tracker.md`
carries the mode-specific surfacing mechanics).

This is the structural enforcement on what goes IN every recorded
fix recommendation (`tracker.md`) and every recommendation in the
closed artifact (`closed-artifact.md`): the artifact's content is
thorough-fix-shaped, not cost-clipped. A cost-clipped
recommendation is a malformed artifact, caught at the
Thorough-fix-default lens application in verify (`lenses.md`).

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
