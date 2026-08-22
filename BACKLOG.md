# Backlog

Work items for the DANEEL plugin, graded by decision-completeness —
not by intent to build. Closed items move to commit history (this
repo's declared closure home): entries leave by commit ref or by a
deliberate one-line drop, never by a DONE grade left sitting here.

Grade vocabulary, closed set: **READY** — design decided, verifier
named, done-criterion stated, write-set named; a fresh context could
execute it. **PARKED** — carries its named missing evidence,
decision, or trigger.

## Open

- **PARKED — Auto-battle mode documentation.** DANEEL inherits the framework's
  interactive / auto-battle mode pair, but the README only describes
  interactive use. Decide whether auto-battle is in DANEEL's scope
  (debugging is higher-risk than building — auto-accepting hypothesis-
  elimination decisions may be the wrong default) or whether it's a
  doc gap to fill. Cross-reference with Clippy's README, which does
  document both modes.

- **PARKED — Effectiveness unmeasured against `diagnosing-bugs`.**
  Missing: the operator decision on the interactivity confound, named
  at the end of this entry. The pilot below is READY and needs it not
  at all. Whether
  DANEEL outperforms the machine-local `~/.claude/skills/`
  `diagnosing-bugs` skill — or a bare model with no skill at all — on
  real diagnosis is ungraded in both directions. Basis: operator
  impression, raised twice on 2026-08-22 (this repo's session, and
  earlier the same day in `-home-g-wan2gp` session `c9cb011f`: "is it
  actually a good helpful skill or doesn't add anything to what a
  model would"); no paired evidence exists either way, and an
  impression formed across *different* bugs has no shared coordinate.
  Design decided:
  - Replay bugs whose cause is ALREADY recorded (fix commits, journal
    incident entries, `.clippy/runs/` artifacts), selected by a rule
    fixed before inspection; symptom handed over in the original
    reporter's own words, never a summary written once the cause was
    known; recorded cause sealed from the arms.
  - Three arms — bare model, `diagnosing-bugs`, DANEEL. The bare arm
    is not optional: it is the coordinate the operator's actual
    question ("adds anything over a model unprompted") is asked in.
    Separate sessions, same tier, worktree each, blind to one another.
  - Criteria pre-registered before any arm runs: cause
    correct/partial/wrong against the sealed record; executed red
    repro present BEFORE the first hypothesis (`diagnosing-bugs`
    makes this a hard gate — "no red-capable command, no Phase 2");
    turns to verdict; symptom-fixed-not-cause count.
  - Grader runs fresh-context on transcript + sealed truth, blind to
    which arm produced it, and is proven red on a deliberately wrong
    diagnosis before any real grading. An ungraded grader measures
    nothing.
  - Report as a behaviour SIGNATURE across 5–8 bugs of differing
    shape (crash, wrong output, perf regression, intermittent). n=1
    decides nothing and 8 will not reach significance — do not
    report it as a result.
  Blocking decision, operator's: DANEEL is interactive by design
  ("User Steers Direction" is one of its four stated principles;
  menu-driven) while `diagnosing-bugs` runs unattended. Scoring both
  in an unattended harness measures DANEEL's checkpoints as dead air,
  not as discipline — so either the operator plays their part
  identically in every arm, or the design is invalid before it
  starts. Settle this first; everything else is decided.
  Tooling: `skill-craft:eval-skill` already implements Tier 1
  triggering and Tier 2 behaviour-delta — use it, do not hand-roll.
  Write-set: this repo (design, results, verdict). No plugin or spec
  change until the measurement lands. Run the pilot below first.

- **READY — Pilot: grade the two `diagnosing-bugs` runs on disk.**
  Runnable without settling the confound above, and recommended
  before any arm is spent. Grade what actually happened in the wan2gp
  perf diagnosis (session `48c328f5`, 2026-08-22) and the CachyOS
  mouse issue (`86cb95f3`, 2026-08-21) against what turned out to be
  true. Verifier / done-criterion: for each run, a yes-or-no on
  whether an executed red repro existed before the first hypothesis —
  the skill's own Phase 1 gate — plus correct/partial/wrong on the
  cause. That single question separates "the skill was ignored" from
  "the skill is insufficient", which take opposite repairs. Costs no
  new runs, and sharpens the criteria before the full matrix: criteria
  that first meet reality during the real run are the ones that turn
  out unmeasurable. Write-set: this repo (results + the sharpened
  criteria, folded back into the entry above).

- **READY — Trigger collision with `diagnosing-bugs`.** Verifier:
  `skill-craft:eval-skill` Tier 1 over both descriptions. Done when a
  bare "this is broken" routes deterministically to the intended
  skill. DANEEL's SKILL.md
  description claims `"debug X"`, `"investigate why X is wrong"`,
  `"find the root cause of Y"`; the machine-local `diagnosing-bugs`
  claims `"debug this"` plus broken/throwing/failing/slow. On a bare
  "this is broken" which one fires is undetermined. DANEEL is also
  the outlier in its own family: statiker, clippy, begehung and
  kaemmung all restrict themselves to explicit operator invocation —
  DANEEL does not. Latent, not live, while DANEEL is uninstalled on
  this machine (registered and removed again 2026-08-22); it becomes
  real on install. Recommendation: narrow DANEEL to explicit
  invocation, matching its siblings, leaving the ambient
  symptom-report lane to the lightweight skill — but the
  effectiveness item above may reverse which skill should own that
  lane, so sequence after the pilot. Measurable TODAY with
  `skill-craft:eval-skill` Tier 1, no debugging runs needed.
  Write-set: `spec/bindings.md` → re-render, never a hand-edit of
  `SKILL.md` (CLAUDE.md, "The skill content is rendered, not
  authored").
