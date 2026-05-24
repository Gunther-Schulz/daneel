---
name: daneel
description: This skill should be used when the user invokes DANEEL on a debugging task — by saying "daneel", "use daneel", "debug X", "investigate why X is wrong", or requesting a structured investigation pipeline for finding root causes of observed wrong behavior.
license: MIT
---

# DANEEL

Evidence-based debugging for Claude Code. The debugging instance of the
diligence-framework — parallel to Clippy (building new behavior),
specialized for *investigating wrong behavior in coding* through
systematic root-cause hunt.

**Scaffold state (v0.1.0):** Skill files below are placeholders pending
render from the framework spec. The full render produces the
investigate-design / implement / verify phase files and the foundations
/ lenses / tracker / post-run-review / debugging-disciplines references.

## Load this now

When triggered, load `phases/investigate-design.md`,
`references/foundations.md`, `references/debugging-disciplines.md`,
`references/lenses.md`, and `references/tracker.md`. These are the
files the AI executes from during a DANEEL run.

## File dependencies

| Document | Purpose | Derived from |
|---|---|---|
| `phases/investigate-design.md` | Root-cause investigation cycles (hypothesis enumeration + systematic elimination) | framework `spec/core.md` §4.1 |
| `phases/implement.md` | Simple fixes inline; complex fixes hand off to Clippy | framework `spec/core.md` §4.2 |
| `phases/verify.md` | Confirm fix corrects observed wrong behavior | framework `spec/core.md` §4.3 |
| `references/foundations.md` | Basis rule, evidence-bearing artifacts, recommendation discipline | framework `spec/core.md` §§1-3 |
| `references/lenses.md` | DANEEL standardized lens set (source-before-result, evidence-over-theories, single-focus, regression-awareness, etc.) | framework `spec/modules.md` §2 + DANEEL.md D-sections |
| `references/tracker.md` | Verification map + hypothesis list + status-state machine | framework `spec/modules.md` §3 + `spec/core.md` §5 |
| `references/post-run-review.md` | Post-run review (auto-battle mode) | framework `spec/modules.md` §4 |
| `references/debugging-disciplines.md` | DANEEL-specific debugging principles (D2 Purpose, D3 Ground Truth, D4 Domain Modeling, D5 Concrete Tracing, D6 Root Cause Analysis) | `legacy/DANEEL.md` D-sections, adapted |

## Relationship to Clippy

DANEEL handles investigation up through root-cause identification + fix
selection. Simple fixes (1-3 components, no architectural change)
complete inline. Complex fixes (multi-component, architectural,
pattern-repetition audit needed) hand off to Clippy's implement phase
via explicit cross-instance handoff.

## Render status

Phase 2 of the scaffold (render from framework spec) is pending. See
`legacy/DANEEL.md` for the prior standalone protocol that this instance
replaces.
