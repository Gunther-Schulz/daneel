# Post-run review

An optional final step: after verify reports [PASSED] (or after
handoff to Clippy on the complex-fix path), surface what the
protocol missed or got wrong, to inform the next iteration. It
is the framework's empirical-validation procedure (specified in
the framework spec `modules.md` §4); DANEEL operates it as
below.

## When and how

**Post-run review is a debugging tool, not a routine check.**
Normal interactive runs and auto-battle runs do not invoke it.
The operator invokes it only when investigating a specific
issue, auditing a release's behavior, or empirically validating
spec changes. The Q-set therefore does not need to fire on every
run — it fires on suspicion. Mechanisms that need to fire on
every run (e.g., catching ad-hoc impl decisions via verify's
design-completeness audit, `phases/verify.md`) belong in-phase,
not here.

**At any point during or after a run, at the operator's
discretion** — at completion (verify [PASSED] simple-fix path or
Clippy-handoff complete complex-fix path), mid-run after a
[READY] presentation, after a verify result, after a cycle that
surfaced unexpected findings, or after an interruption. The
post-run review is an analysis tool for the *protocol's
execution*, not a phase-gated artifact — invocable at any state
the operator wants to inspect.

Especially valuable after a release that changed DANEEL's
behavior (the run is the change's empirical test) and after
runs where the operator overrode a phase transition (e.g.,
selected `another cycle` at [READY]) — the override itself is data.

The operator **may** request a review — a free-text instruction
like "post-run review" or "review the run." Do **not** prompt
for it; the operator decides when a run warrants the analysis.

Q's that depend on a phase not yet reached (e.g., Q6 verify-
phase audit before verify has fired) classify as **not yet
applicable to this run state** rather than skipped silently.

## Output, no persistence

Conduct the review **in the session** — output to the operator.
Do **not** persist the review or its findings to a file in the
project. Persisting prior-run analysis to the project directory
would pollute future runs (a later run reading prior-run
conclusions is biased by them) and clutter the project tree.
The operator carries forward what matters by their own means.

## Standing questions

Questions **surface observable artifacts** (counts,
classifications, quotes, walks, named symptoms); they do **not**
prescribe actions on those artifacts. Action emerges from
operator+AI judgment in the session, applying the framework's
existing disciplines (the gap-triage and three-form enforcement
enumeration in `anneal-framework/development-process.md`). A
Q that prescribes "ship X" / "observe Y" over-reaches the
review's purpose — the session decides.

Ask all seven. Phrase each **artifact-forcing**. Four outcome
dimensions — misses (Q1), cost (Q3), gaps (Q4), attribution (Q2)
— complemented by three protocol-mechanism dimensions: tracker
(Q5), verify (Q6), validation-watch (Q7).

### Q1. Investigation defects — misses (the keystone)

List every investigation defect the run produced. Classify each
as an **escape** (implement, verify, or the fix's behavior in
production recorded it past [READY]) or an **operator catch**
(the operator caught it at the [READY] presentation, before
the run proceeded). For each: name which investigate-design
check (a specific lens — source-before-result,
hypothesis-enumeration, evidence-over-theories,
origin-not-symptom, etc.; the basis rule; the [READY]
judgment) should have caught it and why it didn't; or state
"no existing check covers this class."

Q1 measures the investigation phase's true failure rate. Zero
defects means the investigation phase is working autonomously;
an operator catch means the investigation phase missed but the
[READY] presentation caught (protocol-as-designed); an escape
means both missed (caught only by implement-loopback, verify,
or production — the most expensive catch). Each non-zero count
names a concrete blind spot in the debugging discipline.

For DANEEL specifically, common escape shapes to watch for:
- Hypothesis-list was incomplete at [READY] (a candidate root
  cause that should have been enumerated was missing)
- Theory accepted without verification (jumped from "might be
  X" to fix without [VERIFIED])
- Symptom fix mistaken for root-cause fix (the wrong
  divergence point was treated as origin)
- Pattern-repetition audit skipped or incomplete (architectural
  fix that missed instances)

### Q3. Grind — cost

Where did this run's effort go: token-heavy or time-heavy
regions, repeated work, fixed-shape attestation? Identify any
work the protocol forced but the run didn't need — quote the
specific procedure parts that felt unnecessary. Specific
examples, not impressions.

Cost matters. A protocol that catches everything but costs many
times what it should is broken in its own way. Artifact-forcing:
name the procedure parts, quote the tracker lines or pass
artifacts, don't generalize.

For DANEEL specifically, watch for: per-cycle standardized
inspection passes that produced cited-clean for lenses
genuinely out of scope (over-attestation); hypothesis
re-investigation that the verification map already eliminated
(re-doing work).

### Q4. Ad-hoc additions — gaps

Did you add any ad-hoc check, step, or instruction not in the
protocol this run? For each: quote the ad-hoc work verbatim,
name the gap it was covering, and say whether the gap is
particular to this task or generalizable across runs.

Ad-hoc additions are how protocol gaps surface — the AI noticed
something the protocol didn't cover and patched it. This is the
most directly-actionable signal for protocol completeness.

For DANEEL specifically, common ad-hoc shapes: improvised
debugging discipline not in `references/debugging-disciplines.md`,
heuristics for hypothesis prioritization beyond what
`lenses.md` specifies, cross-instance handoff details not in
`phases/implement.md`.

### Q5. Tracker integrity

For this run's tracker (`.daneel/runs/<run>.md`): was append-only
honored at the ledger layer (no edits to existing ledger lines —
quote any apparent edit); were cycle numbers continuous across
the run including any loopback re-entries (list the sequence);
were F# / D# identifiers unique with no collisions (list any
duplicates); did header updates stay within the header
carve-out (mutable run-state — status, phase, terminal — not
edits to ledger lines)?

Q5 checks the tracker rules' actual adherence. The tracker is
the run's audit trail (`tracker.md`); silent violations corrupt
the record. Each clause is artifact-forceable (quotes, lists,
counts). A violation triages as adherence-class unless the
render or spec allowed the violating reading — then render or
spec gap respectively.

### Q6. Verify-phase audit

For this run's verify (simple-fix path): was the recorded
context "isolated" (quote the result line's context tag —
DANEEL spawns a subagent for verify); were all three checks
accounted for — planned-vs-actual (decision-matching including
root cause, fix approach, pattern-repetition where architectural,
AND design-completeness audit per `phases/verify.md`),
standardized lenses
(`lenses.md`, especially regression-awareness), executable
verification including failure-case re-run — with no check
silently absent (cite each); on [ISSUES FOUND], did the
loopback route to investigate-design (quote the next-cycle
entry); if in auto-battle and the convergence exception fired,
did the finding's evidence field cite the [AUTO-ACCEPTED]
decision by tracker identifier?

Q6 checks verify-phase discipline (`phases/verify.md`). Verify
is DANEEL's primary catcher; its discipline failing means the
catcher itself is failing. Mechanical checks; adherence vs
render vs spec triage applies. (Skip Q6 on the complex-fix
path — Clippy's verify governs there.)

### Q7. Validation-watch cross-check

For each V-N in `anneal-framework/spec/validation-watch.md`,
read the entry's Status line and respond per state:

- **WATCHING**: walk the production signal against this run's
  tracker. Classify as **confirms** (cite the evidence),
  **refutes** (cite the evidence), or **no relevant evidence in
  this run**.
- **FIX-SHIPPED**: walk for **load-bearing instances** of the
  mitigation. Classify as **mitigation load-bearing** (cite the
  finding the mitigation caught + counter-factual: would this
  have escaped the pre-mitigation protocol?), **mitigation not
  exercised this run** (failure shape didn't surface), or
  **mitigation evaded** (a finding escaped that the mitigation
  should have caught — INVALIDATED trigger).
- **RESOLVED**: skip — already validated; only re-walk if a
  later recurrence surfaces in Q1-Q4 findings.
- **INVALIDATED**: skip — already known to need new analysis.

Q7 actively closes the watch loop. The triage routing below
captures signals passively — it surfaces only Q1-Q4 findings
that happen to bear on a V-N. Q7 forces the per-entry walk so
signals accumulate systematically rather than waiting for
fortuitous correlations. "No relevant evidence" across many
WATCHING entries is itself useful signal — a watch entry nobody
exercises warrants its own scrutiny. A **mitigation load-bearing**
finding on a FIX-SHIPPED entry is the positive evidence needed
to promote it to RESOLVED. The operator decides which V-Ns to
transition from the walk's findings.

### Q2. Value attribution

Tag every finding F1..Fn by what surfaced it — a standardized
lens / ad-hoc investigation / the basis rule forcing a search /
a cycle re-examination / verify / the regression check. Give
the counts.

Descriptive data on which mechanisms produced findings this
run. This is **not** a verdict on whether a lens "earns its
rent" — a lens's job is the rare-but-critical catch, and
yield-counting undervalues insurance. A lens that catches a
disaster once in ten runs has terrible yield and pays enormous
rent. Judge a lens by its **misses** (Q1 — what its absence
let escape), not by its hit-rate (Q2).

## Artifact-forcing throughout

A generic "did DANEEL help" yields sycophantic mush. A forced
artifact yields data that can be wrong and checked. Use:

- **"Quote it verbatim"** — a specific tracker line, a lens
  output, a finding, a basis.
- **"Give the count"** — a number, not a hedge.
- **"Classify into one of N"** — a forced choice instead of
  waffle.
- **"Diff against retained run X"** — the retained
  `.daneel/runs/<run>/` tracker directories are available;
  compare this run against a prior one (especially the last run
  under a previous DANEEL version, to measure a release's
  effect).

## What outcomes mean

The review surfaces; the operator decides what to do. The
framework's triage applies (`foundations.md`,
anneal-framework `development-process.md` practice 1):

- A **render gap** — the plugin file does not faithfully carry
  the spec → re-render.
- A **spec gap** — the render is faithful and was followed, and
  it still broke → a finding for the framework or the DANEEL
  spec.
- An **adherence gap** — a faithful render of an unambiguous,
  evidence-bearing rule, violated anyway → failure indicator
  requiring practice 1's three-form structural-enforcement
  enumeration; residual accepted only after enumeration shows
  all three forms fail with cited per-form failure reasons.

## What this is not

A general code review. A confidence check ("did it help"). A
graded report. The review's only job is to surface what the
*protocol* missed or got wrong, in a form the operator can act
on.
