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

- **PARKED — The loop has a termination condition but no CONVERGENCE
  signal.** [READY] says stop when exactly one hypothesis is
  [VERIFIED] and the rest [INVALIDATED]. Nothing anywhere asks
  whether the run is getting CLOSER to that. Incident (2026-08-23
  H3-encode run, `.daneel/runs/2026-08-23-h3-encode-after-generation.md`
  in the `wan2gp` repo): sixteen hypotheses died across nine hours and
  each death spawned a successor, so the live count churned at roughly
  constant size rather than shrinking toward one. Every individual
  cycle was disciplined and produced real findings on real bases; the
  SERIES went nowhere, and nothing in the loop could see it, because
  each cycle grades only itself. The signal that eventually fired was
  the OPERATOR saying they could not continue — a human noticing a
  flat trend across rounds, which is precisely the part auto-battle
  removes.

  What a check would read is already in the tracker and computable,
  not judgement: hypotheses opened versus closed per cycle, and
  whether closures concentrate in the newest cycle's own material. A
  run killing one and opening one per cycle is not converging however
  good its evidence.

  MISSING, and why this is parked rather than ready — two decisions,
  neither of which a fresh context can make from the record:
  (1) how it must not double-fire with the `Instrument-fitness`
  lens: a churning hypothesis count is sometimes the SYMPTOM of an
  instrument that cannot separate the candidates, in which case the
  cure is the instrument and a convergence alarm would fire on top of
  a lens already firing. Whichever is built has to yield to the other.
  (2) the false-fire question: a convergence alarm on a genuinely hard
  bug fires on legitimate slow progress, and a guard that fires on
  correct work trains the override reflex that kills it. That needs
  thinking through, not a quick landing — which is the whole reason
  this is a booking.
  Write-set and verifier: undecided, and dependent on (1).

- **PARKED — Auto-battle mode documentation.** DANEEL inherits the framework's
  interactive / auto-battle mode pair, but the README only describes
  interactive use. Decide whether auto-battle is in DANEEL's scope
  (debugging is higher-risk than building — auto-accepting hypothesis-
  elimination decisions may be the wrong default) or whether it's a
  doc gap to fill. Cross-reference with Clippy's README, which does
  document both modes.

- **PARKED — Effectiveness unmeasured against a bare model.**
  Missing: the operator decision on the interactivity confound, named
  at the end of this entry.

  **Premise change, 2026-08-23: the `diagnosing-bugs` arm is dead.**
  The operator uninstalled that skill the same day and wants none of
  that author's skills. Measured, not assumed: `~/.claude/skills/`
  holds only `close-session`, and no `diagnosing-bugs` exists anywhere
  under `~/.claude/plugins/` (the same grep returns hits in other
  repos' prose, so it can match). A comparison arm that will never be
  installed grades nothing and its repairs are nobody's to make. What
  survives is the operator's actual question, which never needed it.

  Whether
  DANEEL outperforms a bare model with no skill at all — on
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
  - Two arms — bare model, DANEEL. The bare arm is the whole
    coordinate the operator's actual question ("adds anything over a
    model unprompted") is asked in.
    Separate sessions, same tier, worktree each, blind to one another.
  - Two recorded diagnoses are already on disk and are candidate
    replay bugs, selected under the rule above rather than because
    they are to hand: the wan2gp perf diagnosis (session `48c328f5`,
    2026-08-22) and the CachyOS mouse issue (`86cb95f3`, 2026-08-21).
    Both transcripts verified present. Their causes must be sealed
    from the arms before either is used.
  - Criteria pre-registered before any arm runs: cause
    correct/partial/wrong against the sealed record; executed red
    repro present BEFORE the first hypothesis (DANEEL's own
    `Reproduction-first` lens demands it, so it is a criterion both
    arms are graded on rather than one arm's imported gate);
    turns to verdict; symptom-fixed-not-cause count.
  - Grader runs fresh-context on transcript + sealed truth, blind to
    which arm produced it, and is proven red on a deliberately wrong
    diagnosis before any real grading. An ungraded grader measures
    nothing.
  - Report as a behaviour SIGNATURE across 5–8 bugs of differing
    shape (crash, wrong output, perf regression, intermittent). n=1
    decides nothing and 8 will not reach significance — do not
    report it as a result.
  Blocking decision, operator's, and the two-arm design changes its
  shape rather than removing it: DANEEL is interactive by design
  ("User Steers Direction" is one of its four stated principles;
  menu-driven) and the bare arm has no protocol for the operator to
  play a part in. So the confound is no longer "does every arm get the
  same operator" — it is what the bare arm gets instead: the same
  wall-clock and turn budget with an operator answering questions on
  request, or a genuinely unattended run. Those measure different
  things and only the operator can say which one their question means.
  Settle this; everything else is decided.
  Tooling: `skill-craft:eval-skill` already implements Tier 1
  triggering and Tier 2 behaviour-delta — use it, do not hand-roll.
  Write-set: this repo (design, results, verdict). No plugin or spec
  change until the measurement lands.

- **READY — Narrow DANEEL to explicit invocation.** Verifier:
  `skill-craft:eval-skill` Tier 1 over the sibling set. Done when a
  bare "this is broken" routes deterministically to the intended
  skill. DANEEL's SKILL.md
  description claims `"debug X"`, `"investigate why X is wrong"`,
  `"find the root cause of Y"`, so it competes for the ambient
  symptom-report lane. It is the outlier in its own family:
  statiker, clippy, begehung and
  kaemmung all restrict themselves to explicit operator invocation —
  DANEEL does not.

  **Premise change, 2026-08-23.** This was booked as a collision with
  the machine-local `diagnosing-bugs` skill, which is now uninstalled
  (evidence in the effectiveness entry above), so the named
  counterparty is gone and with it the sequencing dependency on a
  pilot that no longer exists. What survives stands on its own and is
  the reason to do it: sibling consistency, and an ambient trigger on
  a heavyweight interactive protocol that nobody asked for. No other
  entry gates this one now.
  Measurable TODAY with
  `skill-craft:eval-skill` Tier 1, no debugging runs needed.
  Write-set: `spec/bindings.md` → re-render, never a hand-edit of
  `SKILL.md` (CLAUDE.md, "The skill content is rendered, not
  authored").

- **READY — Replay the 2026-08-23 batch once a run has used it.** Same
  method as `4a25cd9`, which paid for itself: asking each mechanism WHERE in
  a recorded run it fired found two over-fitted triggers that reading could
  not. Design decided: after the first run that uses 0.2.61, walk the
  batch's mechanisms against that run's tracker, one row each — fired (cite
  the row and the moment), did not fire, or could not fire. Done-criterion:
  a per-mechanism table with no unexplained row. Verifier: the table itself,
  red-first by construction. Write-set: this repo — `dev-notes/OBSERVATIONS.md`
  plus any mechanism the replay finds defective.
  Named missing evidence for the grade: none — it needs a run, not a
  decision, and the next DANEEL run supplies it.

- **PARKED — Mechanize the discharge-presence half of the [READY] gate.**
  The gate now requires every operator-observation finding to CARRY a
  `discharge:` sub-line and none to be OUTSTANDING (`foundations.md`,
  landed `d9e4dbd`). The PRESENCE half is a computable predicate over a
  fixed token and is a real candidate for a mechanical check at [READY] —
  which matters because the dry-run that found this defect also showed the
  field has never been written in a real run.
  MISSING, and why this is parked: nothing marks a finding as an
  operator-observation, so a checker cannot tell which findings owe the
  field. Mechanizing presence alone would demand a discharge on every
  finding in the tracker — a guard firing on legitimate work, which trains
  the override reflex that kills it. The decision needed is whether
  operator-observation findings get a machine-readable marker of their own,
  which is a tracker-vocabulary change and touches the closed grade set.
  Write-set and verifier: dependent on that decision.


## Done

Closed items leave by commit ref. This section is the REF RECORD, not a
second live list — the bodies stay in the commits, one fact one home. It
exists because the machine's pre-push guard reads a file carrier
(`LEDGER.md`, `claude/JOURNAL.md`, `BACKLOG.md`) and cannot see commit
history, so "commit history is the closure home" and "an agent's commits
must be booked" collide unless the refs are written down. Booking them here
satisfies both; the override flag would have satisfied neither.

- 2026-08-23 — **The 2026-08-23 corpus batch, 0.2.52 → 0.2.61.** Fourteen
  commits, all agent-authored, all booked here: `2ea235b` reproduction-first
  / measurement-floor / arrangement-parity lenses + the basis rule's
  derived-claim kind · `b721f4f` replicate-count clause, journal opened ·
  `0e2eca9` testimony-discharge: the [READY] gate and the hypothesis-grade
  lens · `07c493d` independence from Anneal, instrument-sufficiency lens ·
  `b2a65ce` kind (e) binds, `discharge:` field, read-before-build
  precondition · `0ece92c` distribution-shape lens · `b6ea614`
  instrument vantage clause · `3377e6f` a complete cause set spans layers ·
  `7e92fee` an elimination names its re-entry condition, `[INVALIDATED]`
  gets its second sense defined · `970b8e7` bump to 0.2.61 · `4a25cd9` the
  REPLAY: 15 of 17 mechanisms fired, two defects found and fixed ·
  `0551d1c` the diagnosing-bugs pilot dropped on a refuted premise, its two
  dependent entries re-derived · `4ccaa84` the instrument may be DEFECTIVE,
  asked every cycle · `50cfbc1` a basis grounds a verdict only where it
  ENTAILS it.

  GROUNDING: one investigation, `/home/g/wan2gp`, tracker
  `.daneel/runs/2026-08-23-h3-encode-after-generation.md` — read in full by
  the replay lane, cited without reading by an earlier one, which is itself
  why the replay was owed.

  NOT PROVEN, and it is the honest state of the whole batch: no DANEEL run
  has exercised any of it. Everything is Path 1 on incidents and unproven in
  operation — which is exactly the standing the replay just demonstrated
  matters, one level up. The successor item is below.

- 2026-08-23 — **The operator-settled form and the gate dry-run, 0.2.62.**
  `e543f45` bump, after 0.2.61 was pushed mid-lane and the payload guard
  correctly refused a second batch on a released version · `c28abd7`
  Instrument-fitness reduced to ONE line per cycle covering only the
  instruments that cycle leaned on, the per-instrument audit rejected as
  overkill, the pair requirement carved out to freshly-built views, and the
  unconditional scope's grounding moved into the lens text (all eight
  incidents produced numbers that looked FINE, so a suspicion-keyed trigger
  would have fired on none) · `6901cb5` the entailment rule moved from
  `tracker.md` to the basis rule's THIRD EDGE in `foundations.md`, on the
  deciding evidence of that file's own "the rule has two edges" sentence —
  which widens its reach from design-decision verdicts to every
  load-bearing claim · `d9e4dbd` the [READY] discharge gate DRY-RUN, whose
  pair diverged and exposed the gate: a predicate reading only for the
  OUTSTANDING value passes on a tracker recording no discharge at all.

  CLOSES the gate entry booked above. Its named missing evidence — the
  dry-run pair — was produced. Correction carried back: the outstanding
  finding is **F2** (the INT8-to-AWQ flip, never reproduced in fourteen
  cycles), not F32, whose row records a transition ending at
  CONTRADICTED-IN-PART. The successor decision is the parked entry above.

  FIRST OPERATIONAL EVIDENCE, and it is not a replay: cycles 12-14 of the
  same run now open with an "Instrument check:" line in exactly the
  one-line form, and carry three conforming `re-entry:` sub-lines with the
  self-refutation clause exercised ("NOT satisfied at the time of
  striking"). The batch above is no longer wholly unexercised.

- 2026-08-23 — **The 0.2.63 batch: the machinery graded against its own
  run, and three defects it found in itself.** Six commits, booked here
  because the pre-push guard reads a file carrier and cannot see commit
  history: `7548e8a` bump to 0.2.63 — SUBJECT IS STALE, it reads
  "0.2.61 -> 0.2.62" and the commit actually changed 0.2.62 -> 0.2.63; the
  act is right and the message is not, recorded here rather than rewritten
  because history across a co-writer's commits is the more expensive fix
  and this is where a reader looks · `760f53a` Reproduction-first discharges
  on a written pair, Instrument-fitness gains the stated-limits question ·
  `e586ce5` the 0.2.62 batch booked, the [READY]-gate entry closed by its
  own dry-run and replaced · `fe5a201` the discharge state is BORN on the
  finding, and Testimony-discharge asks EACH rather than AN · `ecef684` the
  discharge field's ledger-line shape · `6ff353b` the written pair gets a
  recording site.

  THREE DEFECTS THE BATCH FOUND IN ITS OWN MACHINERY, each by executing it
  rather than reading it:
  1. Testimony-discharge's Scope was anchored to a state nothing writes.
     `discharge:` sub-lines = 0 across 101 entries of the grounding run,
     `re-entry:` > 0 in the same file under the same pattern — writable,
     never written. No finding read as OUTSTANDING, the scope was empty,
     and the lens switched itself off while looking correctly written. It
     fired twice in fourteen cycles.
  2. The [READY] gate's predicate read only for the OUTSTANDING VALUE, so a
     tracker recording no discharge at all satisfied it — absence of the
     field reading as absence of the problem. Found by the dry-run pair,
     never by inspection: a single red would have looked like success.
  3. Reproduction-first's written pair had NOWHERE TO BE WRITTEN — the pass
     artifact is one line per lens. Same shape as the [READY] gate's own
     nowhere-to-write defect recorded in `b2a65ce`, four commits earlier.

  WHAT IT COST, and why the batch exists: F2 in the wan2gp run — the
  operator's INT8-to-AWQ FLIP, one variable changed and the effect seen to
  come and go — sat at entry two, undischarged, through all fourteen
  cycles. `INT8` appears exactly once in 101 entries; no arm ever ran it.

  STILL UNPROVEN: no DANEEL run has exercised the EACH question, the birth
  rule, the recording site, or the stated-limits question. The successor
  replay above covers them.
