# DANEEL Specification — Bindings

DANEEL is the Anneal framework instantiated for debugging. The
framework's method — the model, the mechanisms, the grounding
discipline, phase specs, and the status-state machine — is
specified in the `anneal-framework` repo (`spec/core.md`). That
spec is domain-general; this document binds it to the debugging
domain.

DANEEL adds nothing to the framework's method. It supplies the
things the framework leaves to the instance: the debugging
**bindings** below, the debugging **standardized lens set**
(`lens-set.md`), the debugging-specific **investigate-design
disciplines** (`debugging-disciplines.md`), and the
**run-artifact persistence mechanism** below.

---

## Bindings

Each domain-general term in the framework spec takes its
debugging value:

| Framework term | DANEEL binding |
|---|---|
| the task | a debugging task (observed wrong behavior to investigate) |
| a verified outcome | a verified fix to the wrong behavior |
| the problem space | the code where the wrong behavior originates |
| the work; producing the work | the fix; identifying root cause + producing the fix |
| existing work | the buggy code |
| an element of the work product | a component (file, symbol) in the failure path; a fix |
| scope — the elements the work will modify | the components / files the fix touches |
| exhaustive search of an element's dependents | a repo-wide search of references |
| a located read of the source | a file:line read, an execution-trace excerpt, or a state observation |
| a construct taken whole | a call / function / state value read to its visible close |
| the domain's executable verification | the failing case re-run + regression tests + build / linters |
| the standardized lens set | the debugging lens set (`lens-set.md`) |
| design decision | hypothesis verdict OR fix-approach commitment (the D-track holds both) |
| finding | component / state observation in the verification map (the F-track) |

## Debugging-specific track content

The framework's two tracks (`anneal-framework/spec/core.md` §5)
carry domain-specific shapes in DANEEL:

- **F-track (findings)** is the **verification map** — components
  in the failure path, each carrying [PENDING] / [PARTIALLY
  VERIFIED] / [VERIFIED] (with the summary naming
  behaves-correctly OR produces-wrong-behavior). A
  verified-wrong finding is the **divergence point** —
  hypotheses trace back from it.
- **D-track (design decisions)** is the **hypothesis list +
  fix approach** — candidate root causes ([OUTLINED] →
  [PENDING] → [CONDITIONAL] → [VERIFIED] / [INVALIDATED]) and
  the committed fix recommendation. Hypothesis-list completeness
  is itself a completeness claim (`anneal-framework/spec/core.md`
  §3.2) — every plausible root cause for the observed symptom is
  enumerated and assigned a status.

## Lens set

The framework's inspection passes apply a standardized lens
set — the recurring blind-spots of the domain. DANEEL's is
debugging-specific, specified in `lens-set.md`.

## Debugging disciplines

DANEEL's investigate-design phase carries domain-specific
elaboration (the D-sections — purpose understanding, ground
truth establishment, domain modeling, concrete example
tracing, root cause analysis), specified in
`debugging-disciplines.md`.

## Run-artifact persistence

The framework requires the tracker to persist across
interruptions so a run resumes rather than restarts (framework
`core.md` §6), and specifies it as an **append-only** ledger
(framework `modules.md` §3.1). It further requires each
cycle's **standardized-pass findings artifact** (framework
`modules.md` §3.2) to persist. For these the instance supplies
the persistence mechanism; DANEEL supplies the file mechanism
below.

Each run's tracker is its own append-only markdown file under
`.daneel/runs/`, named for the run — holding a header (the
run's status, the current phase, a one-line task summary, the
run's mode) and the two tracks. A new entry, a status change,
or a correction appends a line; existing lines are never
rewritten. An entry's current state is its latest line; where
current state is needed — the [READY] gate, a resume, the
closed artifact — the tracker is read reduced to the latest
line per entry.

Alongside that tracker, the run's standardized-pass artifacts
are persisted under the same `.daneel/runs/` location — each
cycle-stamped — so the [READY] gate can confirm every cycle's
pass ran. They are per-cycle run artifacts kept beside the
tracker, not filed into it.

At run start the orchestrator scans `.daneel/runs/` for an
in-progress run — a tracker whose run has not reached
[PASSED]:

- an in-progress run for the current task → resume it.
- an in-progress run for a different task → surface it to the
  operator, then start a fresh run.
- none → a fresh run, in a new file.

Completed runs' tracker files are kept; `.daneel/runs/`
accumulates as the project's DANEEL history — no run
overwrites another. `.daneel/` is local run state, not source.
On first creating it, DANEEL adds `.daneel/` to the
repository's `.gitignore` (creating the file if absent) and
notes the change.
