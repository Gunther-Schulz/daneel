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
