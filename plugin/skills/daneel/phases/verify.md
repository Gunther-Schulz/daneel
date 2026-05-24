# verify

The final phase of a DANEEL run (simple-fix path only): it
checks the applied fix against the locked root cause and fix
approach, confirms the observed wrong behavior is now corrected,
and applies the standardized lenses. Ends [PASSED] or
[ISSUES FOUND].

Note: when DANEEL's fix approach is classified as complex, the
implement phase hands off to Clippy (`phases/implement.md`) and
Clippy's verify governs; DANEEL's verify does not run on the
handoff path.

## Isolation

verify is conducted in a context **isolated** from the run's
working context — the one that ran investigate-design and
implement. The agent that wrote the fix does not check it: an
actor checking its own work carries its anchoring and blind
spots into the check. verify works from artifacts alone — the
tracker, `lenses.md`, the code, the failure case — so the
isolated context is fully equipped. The isolation is the
load-bearing property; the orchestrator (`SKILL.md`) establishes
that isolation (in DANEEL, via a subagent spawn) each time
verify runs. The property is unconditional, not a per-task
judgment; the mechanism for achieving it is instance-specific.

verify's recorded result names the context it was conducted in
(isolated, or without isolation), so a [PASSED] carries whether
the check was independent. If an isolated context genuinely
cannot be established — no subagent can be spawned — verify is
still conducted, without isolation, and its result records that;
an un-isolated verify is never silently taken as though it were
independent.

## The three checks

verify runs three checks over the applied fix:

1. **Planned vs actual.** Check every locked design decision
   (`tracker.md`) against what the code actually does. For
   DANEEL, the key decisions verify against are: the
   [VERIFIED] root cause (does the diff actually address it?),
   the fix approach (does the diff match the recorded scope and
   shape?), and the pattern-repetition results (if architectural:
   are all SAME BUG instances actually fixed in this diff?).
2. **Standardized lenses.** Apply the standardized lens set
   (`lenses.md`) to the produced code. For DANEEL's fix-time
   verify, the most active lenses are **Coupled-change**,
   **Failure-path**, **Regression-awareness**, and
   **Thorough-fix-default**.
3. **Executing the verification.** Run the project's test suite,
   build, and linters, and show their output. For DANEEL
   specifically: re-run the concrete failure case from
   investigate-design — the originally-observed wrong behavior
   must now produce correct output. verify does not pass on
   static inspection alone.

The concrete failure case re-run is DANEEL's load-bearing
external truth — the fix that doesn't correct the observed wrong
behavior is not a fix, regardless of what the diff looks like or
what static analysis says.

## Account for every check

verify accounts for every check — no check silently absent. Each
planned-vs-actual check, and every standardized lens (applied
where it is in scope, or given a one-line cited reason it is
not), either holds — recorded as a cited-clean line — or yields
a divergence from the locked design or a lens issue. A failed
run of the executable verification is likewise an issue. A
re-run of the failure case that does NOT produce correct output
is an issue (the fix does not address the symptom; the run
returns to investigate-design via [ISSUES FOUND]).

Any non-failure output the executable verification surfaces is
also a finding unless the project has explicitly de-prioritized
that output class. **De-prioritization** requires an artifact: a
project config naming the class, or a tracker entry recording
the class + reason. De-prioritization without an artifact is
the silent-substitution shape (`foundations.md`) the rule
rejects.

Record every divergence or issue as a finding (`tracker.md`),
entering the finding track at [PENDING].

## Regression check

DANEEL's **regression-awareness** lens (`lenses.md`) applies
specifically at verify: compare BEFORE (the originally-observed
failure case behavior) vs AFTER (the same case post-fix). Three
outcomes:

- **Better** — the failure case now produces correct output;
  fix addresses the root cause. verify proceeds with the other
  checks.
- **Same** — the fix did not change observable behavior; the
  fix did not address the root cause. [ISSUES FOUND], return
  to investigate-design.
- **Worse** — different symptom emerged; the fix exposed a
  pre-existing issue OR introduced a new bug. [ISSUES FOUND]
  with a note: check earlier components per the
  **regression-awareness** lens (the worse outcome signals an
  earlier root cause than the fix targeted).

## Terminal result

verify ends in one of two results:

- **[PASSED]** — every check accounted for, the failure case
  now produces correct output, and no finding short of
  [VERIFIED].
- **[ISSUES FOUND]** — otherwise. This returns the run to
  resolve those findings; verify then re-runs after
  investigate-design completes another cycle and implement
  applies the revised fix.

The terminal result is recorded as an **evidence-bearing
artifact** (`foundations.md`): the result line is paired with a
**finding-status ledger** enumerating every recorded finding's
current status ([VERIFIED] / [PENDING] / [INVALIDATED]). A
[PASSED] declaration alongside any finding short of [VERIFIED]
in the ledger is a malformed artifact — the contradiction is
mechanically detectable at operator review or by a checker.

The recorded result names the context verify was conducted in —
isolated, or without isolation. A [PASSED] is recorded with that
context, so it carries whether the check was independent; a
[PASSED] without isolation is never recorded as though it were
independent.

## Re-run scope

DANEEL re-runs after [ISSUES FOUND] default to **fresh verify
pass** — full re-attest of the three checks. The framework's
delta-verify mechanism (`core.md` §4.3) is parameterized by an
instance-bound "behavior-preserving" classification of the
closing fix; for DANEEL the closing fix is by design a
behavior-changing fix (the bug fix changes what the code does —
that is the whole point). Behavior-preserving re-verify
therefore rarely applies in DANEEL's domain. Default fresh
verify pass; do not invoke delta-verify without explicit operator
direction.
