# Improvement journal

Where DANEEL's own weaknesses are recorded as they are observed in
use. Its reader is whoever next edits DANEEL's rule corpus: the
provenance below is what a later session needs to decide whether a
lens has earned its place, and the firing log is what the
cut-candidate rule reads. This is a **maintenance file** — a write
target, never loaded by `SKILL.md`, `phases/` or `references/`, and
never a read dependency of a run.

Each entry carries four slots: **incident + basis** · **class** ·
**rule/fix text** · **consumer + drain seam**. An observation that
names no rule says so and says why — not every finding wants prose.

Firings are appended under their entry as dated lines naming what
the rule caught. An entry with no firing since the last
consolidation is a cut candidate, not a fixture.

---

## 1. Reproduction-first

**Incident + basis.** The operator reported an observation taken
under mmgp profile 3; the run opened directly on improvement
hypotheses under profile 1 and measured that configuration for nine
hours. Every row was internally valid and
none of them answered the question asked. When reproduction finally
ran, it confirmed the operator's OBSERVATION and contradicted the
CAUSE attached to it — the arm's first encode was already slow
before the enhancer it blamed had loaded. Basis: the DANEEL run
tracker `.daneel/runs/2026-08-23-h3-encode-after-generation.md` in
the `wan2gp` repo (cited, not read from here — separate repo).

**Class.** Frame set once, never re-examined. The opening move
decides what every later measurement is about, and nothing in the
run re-asks it.

**Rule text.** The `Reproduction-first` lens (`lenses.md`,
`spec/lens-set.md`) — landed.

**Consumer + drain seam.** DANEEL cycle 1, via the standardized
inspection pass. Drains at the fire-rate review.

## 2. Measurement-floor

**Incident + basis.** Seven configurations were compared at one run
each, producing a non-monotonic "threshold" that was reported as a
finding. The repeat-unchanged control that would have graded it ran
twentieth and returned 1.0x within-process spread — which rescued
the comparison by luck, not by design; later evidence of
between-process bimodality put it back in doubt. Basis: same run
tracker as entry 1.

**Class.** Comparison without a variance baseline. Arms measured
once each produce a curve whatever the noise is, and the curve
reads complete.

**Rule text.** The `Measurement-floor` lens — landed.

**Consumer + drain seam.** As entry 1.

## 3. Arrangement-parity

**Incident + basis.** Entry 1's incident at row grain: every
measured finding was internally valid, and none recorded the
configuration it was taken under beside the configuration the
operator's own report pinned. Basis: same run tracker as entry 1.

**Class.** A measurement's claimed scope silently wider than the
arrangement that produced it.

**Rule text.** The `Arrangement-parity` lens — landed.

**Consumer + drain seam.** As entry 1.

## 4. Replicate-count visibility

**Incident + basis.** A root cause was recorded [VERIFIED] on a
SINGLE run and had to be invalidated an hour later when a replicate
exposed between-run bimodality. The ledger line read identically at
n=1 and at n=6 — the thinness of the evidence was invisible to any
later reader of the artifact. Basis: same run tracker as entry 1.

**Class.** Confidence in the wording not degrading with the
thinness of the evidence; the artifact's shape offers nowhere for
the replicate count to show.

**Rule text.** The basis rule's replicate-count clause
(`foundations.md`, "The basis rule") — landed: a basis that is a
behavioral test result or an execution trace carries how many
observations it rests on.

**Consumer + drain seam.** The tracker's basis field, at every
recorded verdict. Drains at the fire-rate review.

## 5. Reciting a discipline is not following it

**Incident + basis.** Across the same run, the AI could recite
every discipline it had violated — articulately, into a skill
document — within an hour of violating it. The knowledge was
present and retrievable on demand; it did not retrieve itself at
the moment of action. Basis: same run tracker as entry 1.

**Class.** Knowing and doing are separate properties. Latent
knowledge does not surface itself at the decision moment; something
in the environment has to summon it.

**Rule text.** NONE, deliberately. This is diagnosis, not
instruction. A prose rule minted from it would read insightful and
change no behaviour — which is precisely the failure it describes,
committed one level up. Its actionable residue is entries 1-4,
which are mechanisms with firing moments rather than exhortations.

**Consumer + drain seam.** Whoever next weighs whether a gap wants
prose or a mechanism. This entry is the standing argument for the
mechanism, and it drains only by being cited in that decision —
never by being turned into a rule.

## 6. Testimony-discharge — HYPOTHESIS, validate by use

**Incident + basis.** The run's scope stayed coherent and stable
for nine hours while aimed at a configuration the operator does not
use. The contradicting datum — the operator's own observation under
a different profile — sat in the tracker as a [VERIFIED] finding
the entire time: true, recorded, and permanently inert. Nothing
ever asked whether it had been ACTED on. DANEEL re-opens scope when
the failure surface GROWS, and the convergence cycle demands NEW
surfaces; both trigger on the investigation EXPANDING, and neither
fires on a frame that was wrong from the start and never grew.
Basis: same run tracker as entry 1.

**Class.** Hypothesis, validate by use. The incident grounds that
the lens CATCHES this failure; it says nothing about the rate at
which it fires on correct work, and that rate is what decides
whether the lens survives.

**Known risk, recorded at minting.** Legitimate cycles exist that
should NOT touch operator testimony — chasing a mechanism several
levels below the reported symptom is proper investigation. So the
lens will fire on correct behaviour, and a justification line
demanded every cycle is exactly the shape that gets filled
reflexively. A guard that fires on legitimate work trains the
override reflex that kills it. If the firing log below fills with
entries where the named reason was routine, the lens is wrong in
its current form — retire it or narrow its scope; do not soften the
predicate.

**Rule text.** The `Testimony-discharge` lens (`lenses.md`,
`spec/lens-set.md`), marked in its own text as a hypothesis. Its
enforcement half — [READY] blocks while any operator-observation
finding is OUTSTANDING (`foundations.md`, "Operator testimony") —
is GROUNDED by the same incident and is not part of this
hypothesis: an enforcement gate does not depreciate and carries no
firing log.

**Widened on its own firing data, and the data is the argument.**
The replay (entry 12) put this lens at TWO fires in FOURTEEN
cycles, and the gate dry-run (entry 12, gap C) found `discharge:`
sub-lines at ZERO across the tracker. One cause under both
numbers, and it is not laziness.

The lens asked whether the cycle touches "AN outstanding operator
observation" — indefinite, singular. Once a run settles into a
frame it IS touching one, so the answer is yes every cycle and
the lens stops producing findings. It is satisfied by a
REPRESENTATIVE and never asks for the LIST, and it reads as
compliance every time precisely because one observation genuinely
is being investigated. Measured consequence: F2 — the operator's
INT8-to-AWQ FLIP, their strongest evidence class, one variable
changed with the effect seen to come and go — sat at entry two of
the tracker from cycle 1, undischarged through all fourteen.
`INT8` appears exactly once in the file: no arm ever ran it.

Two changes, one mechanism seen from both ends. EACH, never AN:
the lens answers against the outstanding LIST. And the discharge
state is BORN ON THE ROW at OUTSTANDING rather than checked at
the end — because a state added only when someone notices is a
state nothing keeps, and everything keyed to it then reads an
empty set. That is what the dry-run measured: zero sub-lines, so
the [READY] gate had nothing to block on and this lens had
nothing in scope, both looking correctly written while switched
off.

Why the pair matters more than either half: the only list-walking
mechanism was the [READY] gate, and [READY] is a moment a run
need never reach — this one did not. An obligation placed there
alone sits at the one moment that may never arrive. Born on the
row, the outstanding set is readable at every cycle by anyone,
with no audit and no per-cycle enumeration, so the operator's
one-line ceiling on the standardized pass is not reopened.
[READY] stays the final check and stops being the only one.

Cost check, since this lens's own recorded risk is over-firing:
EACH costs a line per outstanding row, and the list is small by
construction — six operator-observation findings on the run that
minted it. Where a row is not touched this cycle, the existing
"is the reason named" clause already covers it; it simply never
got asked per row.

Landed in `fe5a201` (both lens homes, and `foundations.md`'s
birth rule) and in `tracker.md`'s discharge-field shape. This
entry is that commit's journal record, written after it rather
than with it — the repo's rule is that a minted rule lands its
entry in the same commit, and this one did not.

**Consumer + drain seam.** The standardized inspection pass, every
cycle. Drains at the fire-rate review, which reads the log below.

**Firing log.**

- 2026-08-23 (replay, entry 12) — cycle 1 of the H3-encode run:
  F4 and F5 are operator observations, both OUTSTANDING, and the
  cycle's whole investigation was profile 1 (F6) with no reason
  named for not touching them. Correct fire.
- 2026-08-23 (replay, entry 12) — cycle 2 of the same run: the
  cycle amended scope (D1) and opened D12/F17 without touching F4
  or F5 and without naming a reason. Correct fire.
- Rate so far: 2 fires, 0 on legitimate work. The known risk
  recorded at minting — firing on a cycle properly chasing a
  mechanism below the reported symptom — did not materialise in
  this run, where the cycles that tripped it were the ones
  measuring the wrong configuration. n=1 run; the Path-2 mark
  stands.

## 7. Instrument-fitness

**Incident + basis.** The single artifact that made every finding
in the run possible was a purpose-built harness — launches the app
under any configuration, drives the failing operation, records nine
quantities per run, cleans up, runs unattended. DANEEL graded the
findings that harness produced and contributed nothing to building
it. The run then stalled at a wall that is purely instrumental: two
process startups whose launcher logs are byte-identical — 34 lines,
same models, same pinning, same VRAM plan — with first encodes of
19.6 s and 56.0 s, the enhancer not loaded at that point in either.
A run-order hypothesis was raised and killed by measurement. The
discriminating variable is real and recorded by no instrument in
place, and the response reached for a ninth A/B rather than for
wider tracing. Basis: same run tracker as entry 1.

**Class.** No instrument-building step. Every mechanism in the
corpus assumed the observations already existed and graded how they
were weighed; nothing said that when the evidence needed to
separate the live hypotheses does not exist, building the thing
that produces it is the cycle's work.

**Rule text.** The `Instrument-fitness` lens (`lenses.md`,
`spec/lens-set.md`) — landed, with its completion criterion folded
in: the extension is complete when it produces an observable on
which the live hypotheses DIFFER, each predicted value named
BEFORE the run. That bound replaces "add some logging", which is
the vague criterion that invites premature done.

The recurrence is the load-bearing half: the instrument is built
once and improved every cycle, because the questions sharpen
faster than the tracing does. A one-time build obligation would
have missed the stall this entry records — the harness already
existed and was excellent; it was simply not extended as the
questions moved past what it could see.

**Precondition clause, added on a second incident.** The bench
recorded a sample block on every arm all day — machine state at
`start`, at `after_model_load`, at `end`, and a
`model_load_seconds` figure per run. Nine hours treated the ENCODE
as the slow thing, because that is what the instrument reported
prominently. The discriminating observable was
`model_load_seconds`, present in every row from the first run and
never compared across arms. Read at last: 53.6 s against 31.9 s
for the same ~32.4 GB — the process was already slow before any
encode, before the enhancer, before any configuration under test
could apply. That comparison invalidated the run's whole framing
and cost one query over data already on disk.

The instrument never went silent. It kept returning confident,
correct numbers about the wrong quantity while carrying the right
one in a field nobody read — which is why "read before building"
belongs INSIDE this lens as a precondition rather than beside it
as a rule: it fires at the same moment, and its answer decides
whether the lens's own remedy is even the right one.

Stated precisely, and this is the sharpest form of the gap:
DANEEL's tracker discipline is what CAUSED the discriminating
data to exist. The corpus made the run collect the observable and
never made it read one.

**Consumer + drain seam.** The standardized inspection pass, every
cycle. Drains at the fire-rate review.

## 8. Distribution-shape

**Incident + basis.** Repeated identical-spec runs produced two
tight clusters — roughly 16-21 s and 56-61 s — with nothing between
them, consistent WITHIN each process and switching BETWEEN
processes. The AI read that as "unexplained bimodality" and moved
toward discarding every between-process comparison in the run as
void. The operator read the same rows and named it differently:
consistent-and-bimodal is not randomness, it is a DECISION —
something deterministic reads a condition at startup and branches,
under conditions of the system's own making. That reframing turned
a dead end into a search, and the search immediately found a real
difference in inherited state (1893 MB of a file resident at one
run's first encode against 12 MB at another's). Basis: same run
tracker as entry 1.

**Class.** Reading a distribution's WIDTH and never its SHAPE. The
data had been in hand for hours; what was missing was the question
of what the shape implied about the cause.

**Rule text.** The `Distribution-shape` lens (`lenses.md`,
`spec/lens-set.md`) — landed: scatter indicates interference,
discrete modes indicate a branch, a monotone trend indicates
accumulation.

The distinction from `Measurement-floor` is why this is a separate
lens rather than a widening of that one. Measurement-floor asks
whether an observed difference is REAL; this asks what the
variation's shape implies once it is. Sequential questions with
different remedies — the first answered by repeating an
arrangement, the second by locating a branch — and neither subsumes
the other: a difference can clear the floor and still have its
shape unread, which is exactly what happened here.

**Consumer + drain seam.** The standardized inspection pass, every
cycle. Drains at the fire-rate review.

## 9. Instrument-fitness — the vantage clause

**Incident + basis.** The investigation instrumented a program
whose full source was on disk entirely FROM OUTSIDE: elapsed
timers, `/proc/diskstats`, device service time, page-cache
residency, per-process `read_bytes`. Every hypothesis it generated
was therefore environmental — disk speed, a sync daemon, swap,
contention, inherited cache state — and each was measured and
killed in turn. The operator asked directly whether instrumentation
was being improved; the answer was three MORE external instruments
in one hour, each of which SATISFIED the lens's completion
criterion (device-busy discriminated CPU from disk; per-process
bytes killed the contention theory) while the frame stayed wrong.
The operator had to name it a second time before the vantage
changed. The fix, when it came, was a `sys.addaudithook` recording
every file the process opens FROM INSIDE — available from the first
hour, on a program that is not a black box. Basis: same run tracker
as entry 1.

**Class.** The instrument's vantage bounding the hypothesis space.
The completion criterion is satisfied relative to the LIVE
hypotheses, and the live hypotheses are themselves generated by the
instrument in place: an external rig produces external hypotheses,
discriminates between them successfully and forever, and the set
never comes to include the internal ones. The loop closes on itself
and every measurement in it is valid.

**Rule text.** The vantage clause in `Instrument-fitness`
(`lenses.md`, `spec/lens-set.md`) — landed, sitting after the
read-before-build precondition and before the extend question.

Honesty note, because the entry would otherwise credit the lens
with a catch it did not make: this lens could not have caught this
incident, the incident being what produced the lens. What the entry
records is that the lens AS FIRST WRITTEN would ALSO not have
caught it — the completion criterion was met three times over
while the frame stayed wrong. That is why a clause is being added
rather than the lens being credited.

**The shape, third instance today.** "Complied with the letter
within the existing frame" — the operator's first prompt produced
three more instruments of the same kind rather than a change of
kind. The other two instances: a disclosure rule (basis-rule kind
(e)) satisfied while the very error it named recurred an hour
later, and a scoped read used to settle an unscoped absence (the
spec-origin claim, where the git hook was searched and the
PreToolUse hook was the emitter). Same shape three ways: the
demand is met exactly, inside a frame nobody re-opened.

**Interaction with the precondition clause, graded.** Sequential,
not overlapping. The precondition asks whether what the instrument
already records has been exhausted; this asks where it records
FROM. Neither subsumes: reading every field of an external rig
exhausts it completely and leaves the vantage unchanged — which is
this incident — while a vantage change with unread fields still
owes the cheap query first.

**Consumer + drain seam.** The standardized inspection pass, every
cycle. Drains at the fire-rate review.

## 10. A complete cause set spans layers

**Incident + basis.** Two instances, same run.

INSTANCE 1 — the environment layer crowding out the system layer.
Sixteen hypotheses died across the run. Every one concerned either
the program's CONFIGURATION or its ENVIRONMENT: profile, pinning,
VRAM budgets, co-tenancy, page cache, swap, run order, a sync
daemon, inherited process state. Not one concerned what the
program's own code DOES differently, though its full source was on
disk throughout. The list was exhaustively enumerated and
eliminated inside two layers while a third was never populated.

INSTANCE 2 — the instrument was never a candidate. Twice the true
answer was "the measurement is wrong" rather than "the system does
X": `/proc/diskstats` is SYSTEM-WIDE, so every disk figure
attributed to the program for nine hours included every other
process — caught only when one run reported a physically
impossible 232 GB; and a rate was computed as bytes over ELAPSED
and attributed to a device, twice, the second time an hour after a
rule against exactly that had been committed. In both cases the
hypothesis list held many candidates and none of them was "my
instrument is lying".

Basis: same run tracker as entry 1.

**Class.** Single-layer enumeration reading as complete. The
existing rules are satisfiable inside one layer:
`foundations.md`'s complete-cause-set rule demands the enumerating
search plus per-candidate elimination, and the
Hypothesis-enumeration lens demands the list before investigating
mechanisms — both hold perfectly while a whole class of cause goes
unpopulated. The list has candidates, they die on evidence, and
the discipline is visibly followed.

**Rule text.** The layer-spanning paragraphs in `foundations.md`,
"Complete cause-set as completeness claim" — landed. Placed there
rather than as a lens because the sentence it repairs is that
file's own: "all candidate origins have been enumerated and
evaluated" is exactly what a single-layer list satisfies
vacuously. Widening at the hollow sentence is amendment in place;
a new lens would have left it hollow.

**The instrument as a standing layer** is the half worth
protecting from later pruning. It never nominates itself, being
what every other candidate is graded against, so a wrong
instrument corrupts the list silently while every elimination in
it stays valid and worthless — the candidates really were killed,
by evidence that was not measuring what it claimed.

**Interaction with entry 9's vantage clause, graded.** Adjacent,
neither subsuming. The vantage clause is about the RIG: where an
instrument observes from bounds which hypotheses get generated at
all. This is about the LIST: it must span layers whatever the rig
generates, and it names the instrument itself as a standing layer
the rig can never nominate. A run could fix its vantage and still
never doubt its instrument; a run could doubt its instrument and
still measure only from outside.

**Consumer + drain seam.** The hypothesis list's basis artifact,
at every cycle that locks a verdict. Drains at the fire-rate
review.

## 11. An elimination names its re-entry condition

**Incident + basis.** The operator's question was "I thought the
DANEEL tracker would prevent this? why does it not?" What the
tracker held, verbatim from the run's file:

    D2 [INVALIDATED] (re-affirmed on stronger evidence) the
    enhancer as root cause — basis: F29; its 3/3 correlation was
    with a configuration that provably did not matter

Status, summary, basis pointing at a real measurement. Fully
compliant with the fixed-shape ledger line and with the basis
rule. Two structural failures underneath it:

FIRST, the basis rule asks whether evidence EXISTS, not whether it
ENTAILS the verdict. F29 was true — two byte-identical specs did
produce different results. The inference was invalid: identical
specs differing proves an ADDITIONAL variable exists; it does not
prove the configured variables are inert. "Not necessary" was
collapsed into "not causal". The basis field caught nothing
because there WAS a basis; the defect sat between the basis and
the verdict, where no field looks.

SECOND, eliminations are one-way. Measured consequence: the
strongest signal in the run's data — enhancer configured, 6 of 7
slow, versus a coin flip without — sat outside the hypothesis list
for the rest of the session, and was restored only when the
operator named it. Basis: same run tracker as entry 1.

**Class.** A compliant record of an invalid inference, plus a
terminal with no route back. The tracker performed exactly as
designed, which is what makes this an amendment rather than a
compliance failure.

**Rule text.** Two rules, landed in `tracker.md`, one per
structural failure above.

For the SECOND: the design-decision `[INVALIDATED]` definition. An
eliminated hypothesis carries a `re-entry:` sub-line naming the
observation that would return it to [PENDING], written AT the
strike, and self-refuting where that observation already holds.

For the FIRST: the basis rule in `foundations.md` gains a third
edge, entailment — see "Coverage limit, and its closure" below,
which is where that half is recorded, having been left open when
this entry was first written.

Found while placing it, and it is the deeper root: `[INVALIDATED]`
carried TWO senses and the tracker defined only one. The
definition said "a [VERIFIED] or [AUTO-ACCEPTED] verdict
contradicted by later evidence" — the OVERTURN sense, which
already had a route back. The ELIMINATION sense — the one
`lenses.md` demands ("all alternatives [INVALIDATED]"),
`tracker.md`'s own hypothesis-list-completeness paragraph
restates, and the run used — was defined nowhere. Eliminations are
one-way because elimination was not a defined state, so nothing
defined its re-entry. Both senses are now named; the re-entry
condition attaches to the second.
`investigate-design.md:52` already glossed "[INVALIDATED]
(eliminated)", so the sense was in use and unnamed rather than
invented here. The two senses sit two lines apart at
`investigate-design.md:147` and `:151`, one per track, which is
why the overload went unnoticed: each track predominantly uses one.

**Coverage limit, and its closure.** The re-entry condition fully
answers the second failure. It answers the FIRST only where the
condition is already satisfied at the strike — as it was here,
3 of 3 at that moment. An invalid inference whose re-entry
condition is not yet true still stands, so the re-entry rule was
never the answer to it.

The first failure is now answered in its own right, as a THIRD
EDGE of the basis rule in `foundations.md`: **a basis grounds a
claim only where it ENTAILS it.** The named shape is this
incident's own — evidence that an UNKNOWN variable EXISTS, read as
evidence that the KNOWN ones are INERT.

**Placement, corrected once.** It first landed in `tracker.md`, on
the argument that the gap is a relation between two FIELDS of the
ledger line and the ledger line is `tracker.md`'s. A reviewer
reading both homes put it in `foundations.md` instead, and the
deciding evidence is `foundations.md`'s own enumerating sentence:
"The rule has two edges — basis-naming and true-unit basis." Both
edges ask whether a basis EXISTS or whether it covers the claim's
UNIT; neither asks whether it SUPPORTS the claim. That sentence is
the hollow one, and it is the same amendment shape entry 10
recorded — widening at the hollow sentence is amendment in place,
while landing the rule elsewhere leaves it hollow. It now reads
"three edges".

The tracker keeps only what it owns: the basis FIELD carries what
the evidence establishes, so the entailment edge is readable off
the ledger line rather than reconstructed from it. Rule in
`foundations.md`, field shape in `tracker.md` — two different
facts, not one fact in two homes.

The correction also has a reach beyond placement. In
`tracker.md` the rule bound design-decision verdicts only; in
`foundations.md` it binds every load-bearing claim the basis rule
reaches — investigation premises, embedded claims, completeness
counts. The defect is not specific to the design-decision track,
and the narrower home would have left every other claim
unguarded.

Graded against the re-entry rule above, since they now sit
adjacent: neither subsumes. Entailment asks whether the verdict
was ever entitled to be written; re-entry asks what would return
it. A sound verdict still needs its route back, and an unsound one
is caught before it is written rather than after.

**Interaction with the convergence cycle, graded.** It does not
cover this, and more strongly than expected: that cycle requires
NEW SURFACES, defined as citing a file path or region no prior
cycle cited, and explicitly calls a cycle producing only
re-attestations of prior surfaces MALFORMED. Re-reading a struck
hypothesis is a re-attestation. So the convergence cycle would not
merely fail to catch this — counting such a re-read as its work
would make the cycle malformed under its own rule.

**Consumer + drain seam.** Every strike of a hypothesis, at the
moment the verdict is written. Drains at the fire-rate review.
Fourth instance today of restoration arriving from the operator
rather than the run.

## 12. Replay of the 2026-08-23 machinery against its own run

**Incident + basis.** Entries 1-11 all landed on 2026-08-23 and
none of them had ever fired. Three are enforcement, and a gate
that has never blocked anything is indistinguishable from one that
cannot. This entry is the replay: every mechanism applied to the
recorded run it was minted from, which is red-first by
construction — that run IS the defect each was built for, so a
mechanism silent there cannot fire anywhere.

Read, not cited: the run tracker
`.daneel/runs/2026-08-23-h3-encode-after-generation.md` in the
`wan2gp` repo, all 113 lines, 11 cycles, F1-F41 and D1-D20.
Entries 1-11 above cite that file without having read it (entry
1's own basis line says so); this entry read it.

**The mechanism list is DERIVED, not restated.** The backlog entry
commissioning this replay listed nine mechanisms as of 0.2.55, and
a later lane counted thirteen at 0.2.59 — a restated set going
stale between two readings is this corpus's own named failure
class, so the list below comes from the commits instead.
Derivation: `git log --date=short` bounds the 2026-08-23 work to
nine commits, `2ea235b..7e92fee`, versions 0.2.52 through 0.2.60;
`git diff f645e9a..HEAD -- plugin/skills/daneel/` gives their
hunks in the operational files. Grain rule, stated so the count is
reproducible: one mechanism = one separately-scoped rule with its
own firing moment, as landed in an operational file. That yields
**seventeen**, not nine and not thirteen; the earlier counts are
the same body at coarser grain, not a different body.

**Per-mechanism result.** Fixed-shape rows: mechanism, home,
verdict, moment, tracker row.

1. **Reproduction-first** (lens) — FIRED, cycle 1. F6 records that
   every measurement the cycle took was profile 1; F4 and F5 are
   the operator's own observations, taken under profile 3.
   Reproduction finally ran at cycle 3 (F18, F20).
2. **Measurement-floor** (lens) — FIRED, cycle 1, and again at
   cycle 4. F15's two-class budget result rests on arms measured
   in separate processes; the floor that graded it, F14, is four
   repeats inside ONE process, spread 1.0x. Cycle 4's F23/F24
   invalidated D8 on exactly that grain. See DEFECT A below.
3. **Arrangement-parity** (lens) — FIRED, cycle 1. F12 and F13 are
   behavioral findings recording their own configuration (profile
   1) and not the value the operator's report pins (profile 3);
   F6 is the row that makes the omission visible, one cycle's
   worth of measurement late.
4. **Testimony-discharge** (lens, Path 2) — FIRED, cycles 1 and 2.
   Firing log landed under entry 6. Two fires, neither on
   legitimate work.
5. **Instrument-fitness**, core discriminate question (lens) —
   FIRED, cycle 4. D14 pre-registers a ninth A/B while the live
   hypotheses were separable by no instrument in place; F27 is the
   wall it was run into — two startups whose logs are
   byte-identical for 34 lines with first encodes of 19.6 s and
   56.0 s.
6. **Distribution-shape** (lens) — FIRED, cycles 5-8. F25 and F26
   record two tight clusters with nothing between them, consistent
   within a process and switching between processes; the run read
   that as grounds to void every between-process comparison (D15,
   D16) rather than as a branch to locate. The operator supplied
   the reframing.
7. **Instrument-fitness, read-before-build precondition**
   (lens clause) — FIRED, cycle 1 onward, decisively at cycle 4.
   F28 states it outright: `model_load_seconds` "was recorded on
   EVERY bench row from the first run of the day and never read
   until now". Cycle 4 spent a pre-registered new measurement
   (D14) with the discriminating field already on disk.
8. **Instrument-fitness, vantage clause** (lens clause) —
   FIRED, cycles 5-10. F31, F33, F34 and F35 are four successive
   instruments, all external; F41 records the consequence — every
   hypothesis after D2's strike was environmental. The first
   internal instrument is D20, cycle 11.
9. **Hypothesis-enumeration, single-layer scope widening** (lens
   scope) — DID NOT FIRE. F41 is the incident this widening was
   minted from, and it records a list exhaustive within TWO
   layers, not one. See DEFECT B below.
10. **Basis rule kind (e), the derived-figure operation**
    (foundations) — FIRED, cycle 1. F12 and F14 carry rates
    ("250-257 MB/s") with no operation named beside them; F31
    later established that every such figure all day was bytes
    over elapsed.
11. **Basis rule kind (e), the resource-meter half**
    (foundations) — FIRED, cycles 5-8. F31 is both the fire and
    the correction: device service time "was never sampled by any
    instrument in this run", and F30's 1713 MB/s drive figure
    sat beside model-load rates a third of it.
12. **Basis rule, replicate-count clause** (foundations) —
    FIRED, cycle 3. D8 reached [VERIFIED] on one run per arm
    (F19, F21) and was [INVALIDATED] one cycle later by F23/F24.
    The ledger line read identically at n=1 and at n=6.
13. **Complete cause set spans layers** (foundations) — FIRED,
    cycles 5-10, twice over. Instance 1: F41. Instance 2: F34 —
    the list held many candidates and none of them was "my
    instrument is lying" until an arithmetically impossible
    232 GB forced it in.
14. **Operator-observation discharge state + its `discharge:`
    sub-line** (foundations + tracker) — FIRED, cycle 1. F2, F3,
    F4 and F5 are operator observations and none carries a
    discharge field. The run reached the vocabulary by hand at
    F32, cycles 5-8, four cycles after it was needed.
15. **[READY] gate: no operator-observation finding OUTSTANDING**
    (foundations, ENFORCEMENT) — DRY-RUN pair executed; the pair
    DIVERGED and found a defect. See GAP C below, now closed.
16. **[INVALIDATED]'s two senses** (tracker) — FIRED, whole run.
    D8 moved [VERIFIED] (cycle 3) → [INVALIDATED] (cycle 4): the
    OVERTURNED sense. D3, D4, D5, D6 and a dozen others were
    struck without ever having been [VERIFIED]: the ELIMINATED
    sense, which the pre-amendment definition did not cover, so
    the run spent the whole day writing a state the tracker did
    not define.
17. **`re-entry:` sub-line at the strike** (tracker,
    ENFORCEMENT) — FIRED, and red-proven on the run's most
    expensive defect. D2 was struck at cycles 5-8 with no
    re-entry condition; F40 is the observation that would have
    returned it, and per entry 11 that observation was already
    3-of-3 true at the moment of the strike. The self-refutation
    clause therefore blocks this exact strike. Cost of its
    absence: roughly a day, D2 restored only at cycle 11.

**Score: 15 of 17 fired on the run they were minted from. One did
not (defect B), one could not (gap C).**

**DEFECT A — Measurement-floor does not pin the floor's GRAIN to
the comparison's.** The lens asks for "one arrangement repeated
unchanged, at least three times, its spread recorded as a
finding". F14 satisfies that in full: four cold encodes, one
arrangement, spread recorded, 1.0x. And it graded the wrong thing
— F15 compares arms taken in SEPARATE processes. A within-process
floor read as the floor for a between-process comparison is a
premise the check does not pin, and it fails in the quiet
direction: the lens reads CLEAN while exercising less than it
claims. Fix landed in the lens: the floor is repeated at the same
grain as the comparison it grades.

**Basis re-cited, and the reason is this entry's own subject.**
The clause first cited the 3x of F25 against F29. Cycles 12-13
then established F45 and F46 — every multi-row sweep alternates
by POSITION, 12 of 12, and every row-1-vs-row-2 comparison in the
run is confounded by it. F25/F29 are such comparisons, so the
clause was resting on a figure the run had since invalidated: a
true basis for a rule that stayed true while its evidence was
overturned underneath it, which is the stale-premise shape this
corpus names.

The replacement is F47, and it is stronger than what it replaces
rather than merely intact: FOUR BYTE-IDENTICAL rows in one sweep,
pre-registered before the run, at 19.9 / 59.0 / 21.5 / 59.x s.
Configuration is held constant by construction there, so the 3x
is a between-process spread with nothing configured differing —
exactly the quantity a between-process floor has to clear, and
exactly what a within-process floor of 1.0x cannot supply.

**DEFECT B — the single-layer scope trigger misses its own
incident.** `lenses.md` Hypothesis-enumeration now fires where the
list "is exhaustive within a single layer". The incident that
minted the clause is F41, and F41 says two: "the list was
exhaustive within two layers and never populated a third."
`foundations.md` names three minimum layers — the system's own
behaviour, its environment, the instrument — and this run
populated one of them across sixteen hypotheses. Under a plain
reading of its own words the trigger does not fire on the run it
was built from, which is the definition of an unproven check. Fix
landed: the trigger fires on any of the named layers left
unpopulated, which is the invariant, rather than on a count of
layers occupied, which is the idiom of the one case in hand.

**GAP C — CLOSED by a dry-run pair, which found a defect.** The
gate's anchor is [READY], and the run has never reached it
(`Status: in-progress`, still, at cycle 14). Neither of the two
exits first proposed was taken: re-anchoring the gate to a moment
this run happened to produce would derive the rule from the
artifact meant to grade it — the parentage defect — and accepting
it unproven is what today's whole replay argues against. The third
way is the corpus's own: a criterion's red is a DRY-RUN against
cases in hand, so the predicate was evaluated against the tracker
as it stands.

INPUT A, the tracker as recorded. Operator-observation findings:
F1, F2, F3, F4, F5, F32. Recorded `discharge:` sub-lines: ZERO —
and the instrument is shown both ways, since the same sub-line
grep returns hits for `re-entry:`, so the form is in use and the
absence is real rather than a pattern that could never match. Two
readings of the predicate then diverge:
- read LITERALLY over recorded discharge states, nothing is marked
  OUTSTANDING, so the gate passes — **GREEN**;
- read by enumerating operator-observation findings and asking
  each for its discharge, F2 has never been reproduced,
  contradicted or deferred in fourteen cycles. Its subject appears
  exactly once in the whole file: the INT8-to-AWQ flip, which no
  arm ever ran, every measured arm being AWQ. **RED**.

**The divergence IS the finding.** A predicate reading only for
the OUTSTANDING value is satisfied by a tracker that records no
discharge at all — absence of the field reads as absence of the
problem, and an un-discharged observation is indistinguishable
from a discharged one. This is the same shape as defects A and B:
a check whose premise is unpinned, passing in the quiet direction.

INPUT B, the same tracker with F2 discharged —
`discharge: DEFERRED (the INT8 encoder is not the configuration
under investigation; the operator switched away from it
pre-session and every measured arm runs AWQ)` — plus discharge
lines on F1, F3, F4, F5, F32. Both readings **GREEN**. So the pair
discriminates, and a gate whose two readings disagree on the real
input was not a gate.

FIX landed in `foundations.md`: [READY] requires every
operator-observation finding to CARRY a discharge state AND none
to be OUTSTANDING. A missing discharge is a red.

Correction to the record, since it was carried into the booking:
the outstanding finding is **F2, not F32**. F32's row reads
"discharge OUTSTANDING → now CONTRADICTED-IN-PART" — an arrow
recording a transition, whose final state is CONTRADICTED-IN-PART.
Reading the pre-arrow value as the current one is the label-over-
body drift this corpus names, met in its own tracker.

Still not proven: the gate has never fired in a live run, and a
dry-run grades the predicate, not the moment. That half waits on a
run that reaches [READY].

**Class.** Machinery minted from one incident and never run
against it. The two things the replay found are both of the
shape the corpus already names elsewhere: a check whose premise
is unpinned (A) and a trigger keyed to the idiom of the members
in hand rather than to the invariant every member carries (B).
Neither is visible by reading the rule; both are visible the
moment the rule is executed against the defect.

**Rule text.** Two lens amendments, landed in `lenses.md` and
`spec/lens-set.md` — Measurement-floor's grain clause and
Hypothesis-enumeration's layer trigger. No new lens: both are
widenings of the rule that already owned the question.

**Consumer + drain seam.** The fire-rate review, which now has
data for sixteen of the seventeen mechanisms instead of none. Gap
C is the standing item: it drains when a DANEEL run reaches
[READY] with an operator-observation finding outstanding, and not
before.

## 13. The instrument itself may be DEFECTIVE, not merely thin

**Incident + basis.** Operator-raised, emphatic: *"instrument
itself being buggy!!! very important addition to daneel! needs to
be considered every cycle!!"*

Eight instrument defects in one investigation, one day,
`/home/g/wan2gp`. Each produced plausible output, and each was
caught by something other than the cycle that used it.

OBSERVED — read in the run tracker cited by entry 1, this session:

1. `/proc/diskstats` is SYSTEM-WIDE and was read as per-process
   for a whole day, so every disk figure was contaminated. Exposed
   only by an arithmetically impossible 232 GB model load. Tracker
   row F34.
2. A cumulative total read as a RATE, twice — "257 MB/s",
   "660 MB/s". Tracker row F31; the second figure is the worked
   example already sitting in `foundations.md`'s basis rule.

RELAYED — from the dispatch brief that commissioned this entry,
not independently read here, and weighed as testimony about
measurements rather than as measurements:

3. A `sed` expression with a greedy `.*is` printed IDENTICAL
   numbers for two different profiles — a broken parser emitting
   believable output.
4. A generated red-proof script was re-run STALE and its output
   read as fresh.
5. A contention guard matched earlyoom's own `--prefer` regex, so
   it would have fired forever, training the `--force` reflex that
   kills a guard.
6. `pgrep | head -1` grabbed a transient PID and the wait returned
   a false "finished".
7. A sweep APPENDED to a pre-existing results file and the monitor
   counted an earlier cycle's stale row as this cycle's first
   result. Caught at read time, one step before it entered a
   report.
8. Count-based contention assertions fired on legitimate work.

**Class.** Defect distinct from insufficiency, and invisible from
the same place. An INSUFFICIENT instrument fails to separate the
hypotheses and says so; a DEFECTIVE one separates them wrongly and
says nothing, because its output arrives already shaped like an
answer. `Instrument-fitness` as it stood asked only whether the
instrument could DISCRIMINATE — never whether it was CORRECT — and
its scope line was conditional on "any cycle whose live hypotheses
are not separable by the evidence the current instruments
produce". A defective instrument produces exactly the opposite
condition: confident, separable, false numbers. The lens was
therefore silent by construction on every one of the eight.

**Rule text.** The lens widened rather than a new lens added, and
the reason survived reading rather than being assumed. A second
lens would have had to carry a second scope line, and the
lens-entry shape is one Question and one Scope; more decisively,
the conditional scope is wrong for the READ-BEFORE-BUILD
precondition too, and a new lens would have left that standing.
Entry 12 row 7 is the proof: `model_load_seconds` sat unread from
cycle 1 through cycles whose hypotheses looked separable at the
time. Unconditional scope repairs both halves, so the merge is not
a compromise between them.

Renamed `Instrument-sufficiency` → `Instrument-fitness` in the
same edit. The old name had become narrower than its own body —
a label that stops anyone looking, which is the corpus's own named
drift shape one level up — and a lens reached for on sufficiency
grounds is not reached for on correctness grounds. Ten sites
renamed (`command grep -rn`, zero residual), this journal's
entries 7, 9 and 12 included, so a later editor searching for the
rule finds one name.

**Form settled by the operator**, and the two decisions in it are
what keep the lens alive rather than overridden.

FIRES EVERY CYCLE, unconditionally, and the grounding IS the
justification: all eight incidents produced numbers that looked
FINE. A trigger keyed to suspicious measurements would have fired
on NONE of them. That is the distinction from the sufficiency
half stated as a testable claim rather than as an intuition — an
insufficient instrument announces itself by failing to separate,
a defective one does not announce itself at all.

ONE LINE, not an audit. Compliance is a single cited line per
cycle covering only the instruments THAT CYCLE LEANED ON, and it
answers two questions: what the number is ABOUT (entity, unit,
namespace, and the vocabulary the instrument emits), and whether
the output is THIS RUN'S (file, script and process freshness;
whether an output file was appended to rather than replaced). A
full per-instrument audit was considered and rejected as
overkill — a guard costing more than the defect trains the
override reflex that kills it, which is this corpus's own rule
applied to its own new guard.

The known-positive/known-negative PAIR stays required, but only
for a query, filter, sort or tally built FRESH that cycle: a
self-built view presents as merely looking rather than as
measuring, so nothing else prompts the proof. An instrument
already exercised in an earlier cycle of the same run does not
re-earn its pair. That carve-out is what stops the pair
requirement from becoming the audit the one-line rule just
rejected.

The artifact is what binds: a cycle whose standardized-pass
artifact lacks the line is visibly non-compliant.

The vocabulary clause inside question one has its own provenance,
carried over from the `[INVALIDATED]` two-senses finding: an
instrument's OUTPUT VOCABULARY is part of the instrument, and a
status word carrying two senses is invisible at every reading
site because each reader meets only one of them.

Anchored at `phases/investigate-design.md`, the standardized
inspection pass, which already runs every cycle. One sentence
added there makes an every-cycle scope visible at that seam and
makes its omission malformed, rather than leaving the reader to
infer it from a lens file's scope line.

**Consumer + drain seam.** The standardized inspection pass, every
cycle. Drains at the fire-rate review. Path 1 throughout: eight
measured incidents, two read here and six relayed. Watch the
false-fire rate — one line every cycle is deliberately the
cheapest form that still leaves an artifact, and even one line is
fillable reflexively; a guard filled reflexively is a guard that
has stopped reading. If the firing log fills with lines that
recite the questions rather than answer them about a named
instrument, the form is wrong and the repair is a narrower scope,
never a softer predicate.
