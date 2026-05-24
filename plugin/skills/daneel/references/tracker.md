# DANEEL tracker

Render pending. Source: `diligence-framework/spec/modules.md` §3 +
`spec/core.md` §5.

DANEEL's tracker carries two tracks rendered onto the framework's
findings + design-decision tracks:

- **Verification map** (renders onto findings track) — component states:
  [PENDING] = unverified, [VERIFIED] = component matches truth source,
  [INVALIDATED] = component produces wrong results
- **Hypothesis list** (renders onto design-decision track) — root-cause
  alternatives: [PENDING] = untested, [VERIFIED] = confirmed cause,
  [INVALIDATED] = eliminated, [AUTO-ACCEPTED] = surviving hypothesis at
  [READY] (one-remaining case)

Render to be authored in Phase 2 of scaffold.
