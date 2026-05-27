# DANEEL Specification — README

This directory is the DANEEL specification. DANEEL is the Anneal
framework instantiated for debugging.

The framework's method — the model, the mechanisms, the grounding
discipline, phase specs, status-state machine, modes, artifact
formats — is specified in the `anneal-framework` repo (`spec/`).
It is domain-agnostic. This spec supplies what the framework
leaves to the instance:

- `bindings.md` — the DANEEL bindings (each domain-general
  framework term given its debugging value) and the run-artifact
  persistence mechanism.
- `lens-set.md` — the debugging standardized lens set: DANEEL's
  recurring debugging blind-spots.
- `debugging-disciplines.md` — DANEEL-specific elaboration of
  the framework's investigate-design phase (D-sections: purpose
  understanding, ground truth establishment, domain modeling,
  concrete example tracing, root cause analysis) — the
  debugging-specific disciplines the lens set enforces.

## Conventions

The framework spec's conventions apply unchanged — the
fixed-decision rule, the prescription discipline, the entry
conventions, the operational/analytic term distinction. See the
framework spec's `README.md`.

Uncertain framework decisions are tracked in the framework's
`dev-notes/validation-watch.md`. DANEEL-specific uncertain
decisions, should any arise, would be tracked alongside this
spec.
