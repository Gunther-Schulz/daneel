# The tracker

The structured state record of a DANEEL investigate-design run —
its findings (the verification map: component states) and design
decisions (the hypothesis list: root-cause verdicts and fix
approach), each carrying a status tag. investigate-design produces
the tracker, implement works from it, verify records its results
into it. It carries the run's state across phases: a run
interrupted mid-flight resumes from the tracker.

## The two tracks

A status tag records the state of a **finding** (verification-map
entry: component) or a **design decision** (hypothesis-list entry:
candidate root cause or fix approach) — the tracker's two tracks.
Three tags appear in both, track-scoped, one consistent sense
each: **[PENDING]** (recorded, not yet at a terminal),
**[VERIFIED]** (a verified terminal), **[INVALIDATED]** (a
verified terminal contradicted by later evidence).

Each tracker entry — finding or design decision — is a
**fixed-shape ledger line**: a status tag, a summary, and an
evidence-or-basis field, and **nothing else**. The
evidence-or-basis field holds a basis-rule artifact
(`foundations.md`) — a file:line read, a grep command (for a
completeness claim), an execution-trace excerpt, a behavioral
test result — **not prose describing one**. A free-text account
of having looked is not a basis; a bare count with no command
behind it is not a basis; each is a malformed field. The summary
is one sentence by default; multi-sentence is permitted where
each additional sentence carries a cited rationale or premise
(the basis-rule discipline applies within the summary, not only
at the basis field). An entry has **no narrative field**:
floating uncited reasoning does not belong in the tracker — the
tracker is the run's ledger, not its investigation narrative.
**Derivation walk-throughs** — multi-step prose tracing *how* a
conclusion follows from citations ("test X at file:A failed because
of Y at file:B, which means Z, therefore root cause is…") — are
working context, not basis: record the conclusion, omit the steps. The
standardized-pass artifact (below) is a separate per-cycle
artifact, persisted alongside the tracker, not part of it.

A design decision may carry, as an optional sub-line under its
peer entry, a structured **considered** field — one line per
alternative hypothesis in the shape `considered: <hypothesis>
(rejected: <evidence>)`. The rejection evidence carries a cited
basis (`foundations.md`); loose narrative does not belong here.
The field is optional — fabricating alternatives degrades the
ledger. Use it where alternative hypotheses were genuinely
weighed and naming them informs a later reader.

### Findings (the verification map)

A finding is an observation recorded by inspection — typically a
component or system state being verified during the
root-cause hunt. Each is a fixed-shape ledger line: a status
tag, a summary of what was observed, and its verification evidence
(a basis-rule artifact, `foundations.md`, not prose). It moves
through:

1. **[PENDING]** — recorded; not yet verified.
2. **[PARTIALLY VERIFIED]** — verification begun but incomplete.
   A finding verified in one step moves [PENDING] → [VERIFIED]
   directly; [PARTIALLY VERIFIED] holds one whose verification
   spans cycles.
3. **[VERIFIED]** — verification complete: the finding's content
   is established on evidence — whether the component behaves
   correctly or produces the wrong behavior. Which it is lives in
   the summary (e.g., "F3 [VERIFIED] component X behaves
   correctly per spec" or "F4 [VERIFIED] component Y produces
   wrong output for MONEYLINE rows — divergence point").
4. **[INVALIDATED]** — a [VERIFIED] finding contradicted by later
   evidence. It reopens to [PENDING] for re-verification and
   holds the phase until it does. Only a [VERIFIED] finding
   becomes [INVALIDATED]; one contradicted before [VERIFIED] is
   corrected by an appended line, no status change.

A verified-incorrect finding (a component established as
producing wrong behavior) is the divergence point that hypotheses
trace back from.

### Design decisions (the hypothesis list + fix approach)

A design decision is the AI's resolved verdict on a candidate
root cause OR its committed fix recommendation — a **committed
position**, including a choice to defer. As a fixed-shape ledger
line it is a status tag, a summary of the committed position, and
its **basis** (`foundations.md`) — and nothing else. It is never
an open question or a choice posed to the operator: a posed
question is the absence of a resolution and yields no valid
artifact (`foundations.md`), so the design-decision track holds
none. The basis is the evidence the verdict rests on — a
basis-rule artifact, not prose describing one — or, where it
rests on an assumption, that assumption named. The basis is
mandatory; a verdict whose basis is an assumption cannot reach
[VERIFIED]. A verdict the operator could resolve is recorded
[CONDITIONAL] — the AI's committed recommendation carrying the
operator-resolvable assumption — never a posed choice; the
operator overrides it free-form from the tracker.

The verdict moves through:

1. **[OUTLINED]** — a hypothesis named (candidate root cause);
   investigation not yet started.
2. **[PENDING]** — a concrete hypothesis being investigated; its
   verdict still requires evidence.
3. **[CONDITIONAL]** — a concrete verdict resting on an
   unverified assumption (e.g., "root cause is X assuming Y holds
   in production"); the assumption is recorded with it.
4. **[VERIFIED]** — the verdict locked, basis is evidence; for a
   hypothesis, this is the established root cause; for a fix
   approach, this is the committed fix.
5. **[AUTO-ACCEPTED]** — a [CONDITIONAL] verdict the AI's
   committed recommendation was taken as default when the
   operator did not override. In interactive mode, by the
   operator selecting proceed at the closed-artifact presentation
   without overriding any open [CONDITIONAL]; in auto-battle, by
   mode-absence-of-operator. The recommendation stands; the
   assumption it rested on was not verified.
6. **[INVALIDATED]** — a [VERIFIED] or [AUTO-ACCEPTED] verdict
   contradicted by later evidence. Holds the phase until
   re-formed. For DANEEL, a [VERIFIED] root cause [INVALIDATED]
   by new evidence reopens hypothesis investigation.

An [OUTLINED] hypothesis becomes concrete as [PENDING] or
[CONDITIONAL]. A [PENDING] hypothesis found to rest on an
unverified assumption becomes [CONDITIONAL]; a [CONDITIONAL]
hypothesis becomes [VERIFIED] when its assumption is verified,
and reverts to [PENDING] if disproved. A [CONDITIONAL] hypothesis
still resting on its assumption when the design reaches [READY]
becomes [AUTO-ACCEPTED] — the AI's committed recommendation taken
as default — in both modes (`SKILL.md`).

**Contradiction includes amendment of recorded resolution.** Any
change to a [VERIFIED]/[AUTO-ACCEPTED] verdict's resolution —
root-cause classification, fix-approach scope, completeness counts
(e.g., pattern-repetition instance count), or basis source — flips
through [INVALIDATED]→[PENDING] (forces cycle-another). Silent
in-place edit (same status, changed resolution) is a malformed
artifact. Basis-only refinement (sub-annotation form above,
resolution unchanged) is not amendment.

**Hypothesis-list completeness.** DANEEL's hypothesis-enumeration
discipline (`lenses.md`) makes the hypothesis list itself a
completeness claim: every plausible root cause for the observed
symptom is enumerated and assigned a status. A [READY] state with
the hypothesis list incomplete fails the basis rule for the
root-cause classification — only one hypothesis can remain
[VERIFIED]; all others must be [INVALIDATED] or [AUTO-ACCEPTED]
with cited basis.

A design decision's basis can be broken by another decision, not
only by external evidence: a verdict that contradicts what
another verdict's basis depends on reopens that other verdict by
the rule above — [INVALIDATED] if it reached [VERIFIED] or
[AUTO-ACCEPTED], otherwise revised — incremental against the
run's existing decisions.

## The standardized-pass artifact

Each cycle's standardized inspection pass emits a findings
artifact: one line for each lens whose scope the cycle touched
(`lenses.md`) — a finding, or a one-line cited-clean reason. A
lens out of scope that cycle is not lined; the standardized set
is not re-attested every cycle — it is accounted for whole once,
at [READY].

The artifact is **persisted** and carries its **cycle number** —
a per-cycle run artifact kept alongside the tracker, not filed
into it. Persistence is what lets the working context, at
[READY], account for the standardized set whole — every lens
applied in the cycle(s) its scope was touched, the last cycle's
pass clean — without it, that [READY] condition is unauditable.

## The impl plan

implement-phase produces an impl plan at phase start: dispatch
units derived from the locked fix design, dependency-ordered,
parallel-eligibility marked with a search-established
disjointness basis (`foundations.md`). For DANEEL's simple-fix
path the plan typically has one unit (the fix itself); the
complex-fix path hands off to Clippy with the locked root cause
+ fix approach as Clippy's investigate-design starting input. The
plan is persisted alongside the tracker. Detail is in
`phases/implement.md`.

## [READY]

[READY] criteria + judgment + fresh-session-implementability test
per `phases/investigate-design.md` §[READY]. The tracker carries
the recorded state — hypothesis terminals, design-decision
terminals, lens-pass results — that the [READY] judgment weighs.

## Persistence

Each run has its own tracker — an append-only markdown file
under `.daneel/runs/`, named for the run — holding a header (the
run's status, the current phase, a one-line task summary, the
run's mode) and the two tracks, and **nothing else**. The
standardized-pass findings artifact (above) is persisted
alongside the tracker — a per-cycle run artifact — not filed
into it.

**Status** is one of (closed enum):

- `in-progress` — the run is active in some phase
- `[READY]` — investigate-design terminal (root cause located,
  fix designed); awaiting operator transition to implement
- `PASSED` — verify terminal, no [ISSUES FOUND]
- `FAILED` — verify terminal, [ISSUES FOUND] not resolved within
  the run
- `COMPLETE` — run ended (post-PASSED, after closed-artifact
  presented)

**Phase** is one of (closed enum): `investigate-design`,
`implement`, `verify`.

Two malformed annotation shapes: within-field qualifier on a bare
enum value (e.g. "PASSED (1st pass)" — the common-word-qualifier
symptom, skill-craft `references/anti-patterns.md` Naked-judgment);
and cross-field conflation (e.g. "verify [PASSED]" — embedding the
Status value inside the Phase field, or vice versa). Run-context
that might tempt annotation (the verify sub-pass number, the
convergence-cycle outcome, the current cycle number) belongs in
a separate **Cycles complete** optional field, not embedded in
Status or Phase.

The tracker is **append-only** at the **ledger** layer. A new
entry, and every later change to an entry — a new status, a
corrected summary — is a new fixed-shape ledger line appended to
the file; existing ledger lines are never edited or rewritten.
The header (named above — run status, current phase, task
summary, mode) is mutable run-state distinct from the append-only
ledger: updates to header fields at phase transitions or terminal
moments (status in-progress → PASSED, phase investigate-design →
implement) are not edits to ledger lines. Each ledger line
carries its entry's identifier (`F1`, `D3`, …); an entry's
**current state is its latest line**. Where current state is
needed — at [READY], the closed artifact, a resume — read the
tracker reduced to the latest line per entry. Appending rather
than rewriting is cheaper, and the un-rewritten ledger history is
the run's audit trail. This holds in both modes.

A **basis-only refinement** on a terminal-status entry
([VERIFIED], or [AUTO-ACCEPTED] in auto-battle) — same status and
summary, strengthened basis (an execution trace now grounds what
previously rested on a code read) — is appended as a
**sub-annotation** under the entry, not as a new peer-level
ledger line. Format: an indented bullet beneath the entry's
latest peer line, carrying the strengthened basis on one line —
e.g.

```
- D2 [VERIFIED] root cause: filter in component C drops MONEYLINE rows — basis: file:line + execution trace at run R
  - basis strengthened (cycle 3): regression test confirmed, F17
```

Reduce-to-latest: an entry's current state is its latest
peer-level line together with any sub-annotations attached. The
append-only property holds — no past line is rewritten, the
sub-annotation is itself a new appended line — preserving the
audit trail without the duplicate-line tax.

**Sub-annotations are scoped to basis-only refinement of the
parent entry's own claim** (same status, same summary,
strengthened basis). Cross-run observations, recurrence-
confirmations (e.g. "the pattern observed in F-N also appears in
Run X's data"), and findings that surface a new claim — even one
related to or confirming a prior entry — appear as peer-level
F-entries with their own basis, not as sub-annotations under the
related entry. The discriminator: does the new evidence
strengthen the parent's own claim verbatim, or does it constitute
its own observation? Verbatim-strengthening → sub-annotation;
new observation → peer-level entry.

A completed run's tracker file is kept; `.daneel/runs/`
accumulates as the project's DANEEL history — no run overwrites
another. `.daneel/` is local run state; DANEEL gitignores it on
first write. DANEEL's run lifecycle (`SKILL.md`) specifies how a
run start finds an in-progress run to resume, or opens a fresh
tracker.
