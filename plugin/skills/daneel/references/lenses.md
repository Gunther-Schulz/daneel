# DANEEL lenses

Render pending. Source: `diligence-framework/spec/modules.md` §2 +
DANEEL.md D-sections.

DANEEL's standardized lens set (the debugging-specific instances of the
framework's lens mechanism):

- **Source-before-result** — verify source state before investigating
  transformations
- **Origin-not-symptom** — trace backward to where wrongness enters
- **Evidence-over-theories** — mark [VERIFIED] only with evidence;
  theories become hypotheses, not investigation directions
- **Single-focus** — one investigation target per iteration
- **Regression-awareness** — "worse after fix" signals earlier root
  cause
- **Hypothesis-enumeration** — generate complete hypothesis list before
  investigating mechanisms; cannot conclude without systematic
  elimination
- **Theory-resolution** — theory formation ("might be", "could be")
  triggers stop-and-list; cannot continue based on unverified theory

Render to be authored in Phase 2 of scaffold.
