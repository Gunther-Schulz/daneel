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

*Scope:* the first cycle of any run whose symptom is an
operator observation rather than a failing check.

## Testimony-discharge

*Question:* does this cycle's investigation touch an outstanding
operator observation — and where it does not, is the reason
named? A frame set wrongly at the start never grows, so every
mechanism that re-opens scope on a GROWING failure surface stays
silent on it; each cycle reads internally coherent while the run
measures a system nobody reported a problem with.
(*Hypothesis, validate by use.* This lens is known to catch that
failure; how often it fires on correct work is unmeasured —
investigating a mechanism several levels below the reported
symptom is proper, and trips it.)

*Scope:* any cycle in a run holding at least one
operator-observation finding whose discharge state
(`foundations.md`) is OUTSTANDING.

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

## Instrument-sufficiency

*Question:* has what the instrument ALREADY records been
exhausted, and can it then discriminate the live hypotheses?
Read before building: a discriminating observable is often
already collected and unread, because an instrument reports
prominently what it was built to watch and carries the rest
quietly. An unread field is indistinguishable from an absent one
from inside the run — the instrument does not go silent, it
keeps returning confident, correct numbers about the wrong
quantity. The remedy there is a query, not a build.

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

*Scope:* any cycle whose live hypotheses are not separable by
the evidence the current instruments produce.

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
hypothesis list is missing or has fewer than 2 candidates
without cited reason.

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
