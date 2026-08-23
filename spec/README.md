# DANEEL Specification — README

This directory is the DANEEL specification. DANEEL derives
historically from the Anneal framework and is now independent of
it; the framework repo no longer exists, so nothing here routes
through it.

The method DANEEL inherited — the model, the mechanisms, the
grounding discipline, phase specs, status-state machine, modes,
artifact formats — is carried in this repo: the grounding
discipline in `plugin/skills/daneel/references/foundations.md`, the
phase specs in `plugin/skills/daneel/phases/`, the run artifact in
`plugin/skills/daneel/references/tracker.md`. This directory
supplies the domain binding:

- `bindings.md` — the DANEEL bindings (each domain-general
  framework term given its debugging value) and the run-artifact
  persistence mechanism.
- `lens-set.md` — the debugging standardized lens set: DANEEL's
  recurring debugging blind-spots.
- `debugging-disciplines.md` — DANEEL-specific elaboration of
  the investigate-design phase (D-sections: purpose
  understanding, ground truth establishment, domain modeling,
  concrete example tracing, root cause analysis) — the
  debugging-specific disciplines the lens set enforces.

## Conventions

This spec was written under four inherited conventions — the
fixed-decision rule, the prescription discipline, the entry
conventions, the operational/analytic term distinction. Their text
is **UNRECOVERABLE**: it lived in the framework spec's `README.md`,
and that repo no longer exists. Only the four names survive here.

Treat this as an open gap, not as a settled convention set. What
the entries in this directory already DO is the best available
evidence of what those conventions required; a session that needs
one of them re-derives it from the entries and writes the result
here, rather than citing a rule nobody can read.

Uncertain DANEEL decisions are tracked alongside this spec.
