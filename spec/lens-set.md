# DANEEL Specification — Lens set

DANEEL's standardized lens set — the debugging-specific
inspection criteria the standardized pass applies. The set is
DANEEL's own; modes and artifact formats are specified in
`plugin/skills/daneel/references/`.

Each lens fills the lens-entry shape — Name, Question, Scope.
Built on the DANEEL bindings (`bindings.md`).

---

## Reproduction-first

*Question:* has the operator's own observed state been
reproduced, in their configuration and their sequence, before
any improvement is investigated? Their report is testimony
ABOUT a measurement, not the measurement — believed, and
verified. Reproduction is what converts it into a finding,
and gives every later comparison a baseline on the system
that matters; a run opening on an improvement hypothesis
measures a system nobody reported a problem with. Divergence
between the report and the reproduction is the run's most
informative first finding — a detail the report omitted, or a
cause it attributed wrongly — and it surfaces nowhere else.

Discharge is a WRITTEN PAIR, never a matching number: the
operator's sequence as steps, the arm's sequence beside it. The
pair lands as a tracker FINDING carrying both sequences, and the
cycle's pass line CITES that entry — the pass artifact is one
line per lens, which two step lists cannot be, and a requirement
with nowhere to be written is one nobody can comply with. Citing
an entry is the shape the pass already uses, so the pair also
survives into the tracker instead of living only in a per-cycle
artifact. A
symptom of the right size is what a wrong arrangement also
produces, so a number in the reported range reads as arrival and
stops the checking. Naming sequence in the question does not
survive that moment — an unwritten comparison leaves nothing to
tell a reproduction from a resemblance, and the lens then fires,
is answered, and certifies the wrong arm.

*Scope:* the first cycle of any run whose symptom is an
operator observation rather than a failing check.

## Testimony-discharge

*Question:* does this cycle's investigation touch EACH
outstanding operator observation — and for every one it does
not, is the reason named? A frame set wrongly at the start never
grows, so every
mechanism that re-opens scope on a GROWING failure surface stays
silent on it; each cycle reads internally coherent while the run
measures a system nobody reported a problem with.
(*Hypothesis, validate by use.* This lens is known to catch that
failure; how often it fires on correct work is unmeasured —
investigating a mechanism several levels below the reported
symptom is proper, and trips it.)

EACH, never AN: the answer is given against the outstanding
LIST, not a representative of it. An indefinite article is
satisfied by whichever observation the run already happens to be
pursuing, so the lens keeps returning yes while a SIBLING
observation goes untouched — and it reads as compliance every
time, because one of them genuinely is being investigated. The
list is small by construction, so EACH costs a line, not an
audit.

*Scope:* any cycle in a run holding at least one
operator-observation finding whose discharge state
(`foundations.md`) is OUTSTANDING — which is every such finding
from the moment it is written, since the state is born there.

## Source-before-result

*Question:* has the state at the source of the wrong behavior
been verified before investigating downstream transformations?
Source reveals origin; result shows symptom — verify the source
state exists and matches expectation BEFORE investigating how
transformations produce the wrong output.

*Scope:* any cycle that proposes investigating a transformation,
processor, or downstream component while the source state itself
is [PENDING].

## Origin-not-symptom

*Question:* does the investigation target the origin (where
wrongness enters) or the symptom (where wrongness is observed)?
Tracing backward to the first divergence point is the
discipline; investigating the location where the symptom
surfaces is the failure shape.

*Scope:* any investigation proposal targeting a component
downstream of an established [INVALIDATED] component without
first verifying components between them.

## Evidence-over-theories

*Question:* does the cycle conclude on evidence or on theory? A
theory ("might be," "could be," "possibly") triggers
stop-and-add-to-hypothesis-list; cannot continue investigation
based on unverified theory. Marking a hypothesis [VERIFIED]
requires evidence the hypothesis cannot be produced without
doing the verification work (per the basis rule,
`plugin/skills/daneel/references/foundations.md`).

*Scope:* any cycle that locks a hypothesis verdict ([VERIFIED]
or [INVALIDATED]).

## Instrument-fitness

*Question:* is the instrument CORRECT; has what it ALREADY
records been exhausted; and can it then discriminate the live
hypotheses? The three run in that order, and only the last has a
conditional trigger.

The correctness question fires on EVERY cycle, and the incidents
that minted it are the reason: all eight produced numbers that
looked FINE. A trigger keyed to suspicious measurements would
have fired on none of them. That is the whole difference from the
sufficiency question below — an insufficient instrument announces
itself by failing to separate the hypotheses; a defective one
does not announce itself at all, its output arriving already
shaped like an answer.

ONE LINE per cycle, covering the instruments THIS CYCLE LEANED
ON and not every instrument in the run. The line answers three
questions:

- **What is the number ABOUT?** The entity measured (this
  process, or the whole machine), the unit (a total, or a rate),
  the namespace an identifier belongs to, and the vocabulary the
  instrument emits — a status word carrying two senses is the
  case each reader meets only one of. A system-wide counter read
  as per-process contaminates every figure derived from it and
  stays plausible until one of them is arithmetically impossible.
- **Is the output THIS RUN'S?** The file, the script and the
  process actually read — a generated probe re-run stale, a
  transient PID caught by a first match, an output file APPENDED
  to rather than replaced, so an earlier cycle's row counts as
  this cycle's first result. Each returns exactly what a fresh
  correct run returns.
- **What does the instrument's own text say it CANNOT do?** A
  limit written into a tool's header, docstring or comments is a
  claim about THIS cycle's question, not a disclaimer discharged
  by having been written. Stating a gap honestly makes it read as
  handled — to its author first, because the sentence is evidence
  of rigor — so it survives every later read as a credit rather
  than as an open finding, while the tool's own parameter names
  become the interface and the prose above them goes unread.
  Weigh the text as its author's TESTIMONY, in both directions:
  a stated limit is graded like any other claim and can be
  over-cautious or stale, and its ABSENCE clears nothing — no
  author enumerates the gaps they did not see. Where a named
  limit touches this cycle's question, the instrument does not
  answer it whatever number it returns.

A FRESH query, filter, sort or tally built this cycle carries in
addition a known-positive/known-negative PAIR drawn from the REAL
data: a case known to carry the property appears, one known not
to is absent. A self-built view presents as looking rather than
as measuring, so nothing else prompts the proof, and a
constructed row proves the expression parses rather than that it
separates. An instrument already exercised in an earlier cycle of
this run does not re-earn its pair.

Read before building: a discriminating observable is often
already collected and unread, because an instrument reports
prominently what it was built to watch and carries the rest
quietly. An unread field is indistinguishable from an absent one
from within the investigation — the instrument does not go
silent, it keeps returning confident, correct numbers about the
wrong quantity. The remedy there is a query, not a build.

Ask where the instrument observes FROM, not only what it
records. Extending along the axis it already has is a different
act from changing its vantage, and only the first feels like
progress: an instrument outside the system generates hypotheses
about the system's surroundings, discriminates between them
successfully, and never generates the internal ones — each round
reads as sufficient, against a hypothesis set the instrument
itself produced. Where the system's own source, symbols or
runtime are reachable, measuring only its environment is
choosing the weaker method: black-box technique is for black
boxes.
Environmental candidates stay legitimate and worth killing; what
the vantage decides is whether any other KIND ever enters the
set.

The vantage question has a FIRING MOMENT and an OUTPUT of its
own, because being in scope was not enough: on the SECOND
consecutive cycle whose arms come from the same instrument
without the live hypothesis set shrinking, the cycle's instrument
line names the SOURCE LOCATION that decides the outcome — file
and line — or states that no such location exists and why. Both
halves of the condition are countable off the tracker: arms per
cycle, live hypothesis count per cycle, instrument named per
cycle.

Keyed to REPETITION, not to suspicion, for the same reason the
correctness question is keyed to every cycle: no single round
looks wrong. Each separates something and returns a clean number,
so a wrong frame is visible only as a SERIES — and a series is
the one thing no individual cycle can see. Measured: eighteen
cycles asked what CONDITIONS produce an effect and none asked
what LINE decides it; reading the loader answered it in twenty
minutes once the question was finally put.

Where two hypotheses then still predict the same observable
under the current tracing, extending the instrument IS this
cycle's work — a counter, a finer log, a probe at the divergence
point — not a further experiment against an instrument that has
already returned everything it can. An instrument is built once
and improved every cycle: the questions sharpen faster than the
tracing does, and a static one silently caps the investigation
at the resolution it was born with. Either route is complete
when it produces an observable on which the live hypotheses
DIFFER, each one's predicted value named BEFORE the run — not
when more logging exists.

*Scope:* every cycle. The correctness question carries no trigger
condition — the cycle whose measurements look fine is the case it
exists for. Its artifact is the ONE line above, and a cycle whose
standardized-pass artifact lacks that line is visibly
non-compliant; a per-instrument audit is not what is being asked
for and would train the override reflex that kills the lens. The
exhaustion and discrimination halves are answered in one further
cited line where the live hypotheses are already separable by the
evidence in hand.

## Arrangement-parity

*Question:* does each measured finding record the configuration
it was taken under, and — for every variable the operator's own
report pins — their value beside it? A measurement establishes
something about the arrangement that produced it and nothing
about any other; a finding taken under a different configuration
than the operator runs reads complete and answers a question
nobody asked.

*Scope:* any finding whose basis is a behavioral test result or
an execution trace.

## Measurement-floor

*Question:* has the noise floor been established before two
measurements are compared? One arrangement repeated unchanged,
at least three times, its spread recorded as a finding — that
spread is what a claimed difference between arms must clear.
Arms measured once each produce a curve whatever the variance
is, and every row reads complete either way; the floor is what
separates a result from the floor itself.

The repeats span the same UNIT the comparison does. A floor built
by repeating inside one process grades comparisons inside that
process; arms taken in separate processes need a floor of
separate processes. The wrong-grain floor is the quiet failure —
it meets the repeat count, records a spread, and reads clean
while grading nothing the comparison rests on. Measured, with
configuration held constant by construction: four BYTE-IDENTICAL
rows in one sweep ran 19.9 / 59.0 / 21.5 / 59.x s — a 3x
between-process spread with nothing configured differing —
against a 1.0x spread for four repeats inside one process. The
floor that graded the comparison was the second number; the
comparison lived in the first.

*Scope:* any cycle whose verdict rests on comparing measurements
taken in separate runs.

## Distribution-shape

*Question:* what does the SHAPE of a measurement's variation say
about its cause? Scatter indicates interference; DISCRETE MODES
indicate a branch — something read a condition and chose a path,
and the modes are its outcomes; a monotone trend indicates
accumulation. A result that is consistent within each run and
switches between them is not noise to average away, nor a
comparison to abandon: it is a decision to locate, and the modes
are evidence of where to look.

*Scope:* any cycle whose repeated measurements vary in a way
that is not scatter.

## Single-focus

*Question:* does the iteration investigate one target, or
multiple simultaneously? When user (interactive) or AI
(auto-battle) approves an investigation, exactly one target is
in scope per iteration. Multi-target investigation in a single
iteration is the failure shape.

*Scope:* any investigation proposal in a cycle.

## Hypothesis-enumeration

*Question:* when a symptom is identified, has the complete
hypothesis list been generated BEFORE investigating mechanisms?
Cannot conclude root cause without all alternatives
[INVALIDATED] except one [VERIFIED]. Investigating one
hypothesis and concluding without testing others is the failure
shape; the cycle would close on insufficient elimination.

*Scope:* any cycle where a symptom has been identified but the
hypothesis list is missing, has fewer than 2 candidates without
cited reason, or leaves any of the minimum layers unpopulated
(`foundations.md`, complete cause set) — a long list spanning
some layers and not others does not trip the count.

## Regression-awareness

*Question:* after a fix attempt, is the comparison BEFORE vs
AFTER checked? "Worse" signals earlier root cause (the fix
exposed a pre-existing issue); "same" signals the fix didn't
address root cause; "better" signals progress. A fix that
produces "worse" without returning to investigate-design is the
failure shape.

*Scope:* any verify cycle following an implement attempt that
changes the observable behavior.

## Pattern-repetition

*Question:* for an architectural / systemic root cause, has the
pattern-repetition audit been performed? An architectural bug
typically has multiple instances; fixing only the
originally-observed instance is a thorough-fix violation. The
audit enumerates all uses of the same pattern, classifies each
(SAME BUG / DIFFERENT CONTEXT), and widens the fix scope to all
wrong instances.

The enumeration is itself a completeness claim
(`plugin/skills/daneel/references/foundations.md`) — the basis
cites every
**plausible reference shape** the audit grep covered. A pattern
can hide behind **bare-form** (just the identifier or call
shape), **path-form** (qualified module path), **parent-module
name**, **string-target references** (`mock.patch`,
`monkeypatch.setattr`, `importlib.import_module` targets —
string, invisible to symbol-only greps), **docstrings**,
**comments**, and **config/data files**. A narrow-shape audit
grep (one form only) declaring the pattern enumerated is missing
its true-unit basis — unenumerated shapes are unsearched scope.

*Scope:* any fix proposal where the root cause classification is
architectural / systemic (not isolated typo / one-off mistake).
The enumeration spans src + tests + docs + config.

## Coupled-change

*Question:* does the fix force a coupled change elsewhere for
the system to stay coherent? A fix involving replacement,
removal, or amendment of an existing artifact couples to its
references and its load-bearing behaviors.

*Scope:* any fix that modifies an existing code artifact.

## Failure-path

*Question:* does the fix specify failure and
absent/invalid-precondition behaviors? A fix that introduces new
code paths must not let any error degrade without signal —
caught, the run continues, output looking like success.

*Scope:* any fix that introduces new components, branches, or
preconditions.

## Thorough-fix-default

*Question:* does each committed position the cycle produces —
hypothesis verdicts, root-cause classification, fix
recommendation — lead with the thorough-fix shape, with cheaper
alternatives weighed alongside (not as primary)? Pre-clipping on
perceived cost (session length, multi-file scope, "another
session" framing) is the failure shape; cost is the operator's
judgment, not the AI's pre-clip. For DANEEL: a "fix the symptom
not the root cause" recommendation is a thorough-fix violation.

*Scope:* any committed position the cycle produces — hypothesis
verdicts ([VERIFIED] or [INVALIDATED]) and the cycle's fix
recommendation in the closed artifact.

## Target-locality

*Question:* for a fix recommendation naming a new file, module,
or package as a target (new helper location, new module for
extracted logic, new file for the fix's added code), does the
fix's basis include a re-runnable grep of the target's parent
directory establishing that no existing module already serves
the purpose? A "new file" claim with no negative-evidence basis
is missing its true-unit basis — the implicit completeness
claim ("no existing nearby module is suitable") was not
grounded. The lens catches the clear-but-wrong-target failure
shape: a fix that would implement cleanly but lands in the
wrong location because the neighborhood was not surveyed.

*Scope:* any fix recommendation naming a new target (file,
module, package) for code being added, moved, or extracted as
part of the fix. Symmetric: when an existing file is named as
target, the basis includes the file's current contents (read),
not recall.
