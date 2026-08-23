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

**Consumer + drain seam.** The standardized inspection pass, every
cycle. Drains at the fire-rate review, which reads the log below.

**Firing log.** No firings recorded. Per skill-craft's durability
classes a capability patch with no logged firing since the last
consolidation is a cut candidate; this lens is born as exactly that
and stays one until dated lines appear here naming what it caught.

## 7. Instrument-sufficiency

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

**Rule text.** The `Instrument-sufficiency` lens (`lenses.md`,
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
