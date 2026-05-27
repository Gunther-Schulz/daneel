# DANEEL Specification — Lens set

DANEEL's standardized lens set — the debugging-specific content
for the Anneal framework's standardized lens set. The framework
leaves the *set* to the domain instance (`anneal-framework`
`modules.md` §2.2); modes and artifact formats are
framework-level and used as specified there.

Each lens fills the lens-entry shape — Name, Question, Scope —
from the framework's `modules.md` §2.1. Built on the DANEEL
bindings (`bindings.md`).

---

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
`anneal-framework/spec/core.md` §3.2).

*Scope:* any cycle that locks a hypothesis verdict ([VERIFIED]
or [INVALIDATED]).

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
(`anneal-framework/spec/core.md` §3.2) — the basis cites every
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
