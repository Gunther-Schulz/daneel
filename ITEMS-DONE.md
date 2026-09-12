# This repo had NO legacy closure home. The absence was STATED at
# migration time (`--from-done NONE`), never inferred from a missing
# file: the archive below is empty because there was nothing to
# archive, which is a different fact from nothing having been read.

schema: 2

## da-7
grade: DONE
requirement: 1 tracked file(s) still name the migrated carrier(s) `BACKLOG.md`. A consumer left pointing at a carrier nobody writes any more reads as current until someone notices, and nobody is scheduled to — record: the migration report
goal: tend
write-set: dev-notes/OBSERVATIONS.md
done-criterion: no tracked file outside the migration's own outputs names `BACKLOG.md`, or each remaining one is recorded as a declared exemption
evidence: tracked files naming `BACKLOG.md` at migration time: dev-notes/OBSERVATIONS.md
blocked-by: NONE
blocker-moot: every consumer migrated or declared exempt
closed-reason: 2026-09-12 The single consumer was dev-notes/OBSERVATIONS.md:36, 'The successor entry that supplies those runs is booked in BACKLOG.md' — a LIVE pointer, not a historical mention, so it was retargeted rather than declared exempt. It now names da-5 in ITEMS.md, which is the entry it meant (READY — Replay the 2026-08-23 batch once a run has used it). Naming the item ID rather than only the file is deliberate: an ID survives the line shifts a regenerated carrier produces. Verified by probe pair: BACKLOG.md occurrences in that file 1 -> 0, da-5 occurrences 0 -> 1.

## Archive (pre-migration)


<!-- CLOSURES READ FROM THE `--from` CARRIER ITSELF (lc-18/lc-19). The
     design modelled closures as a separate `--from-done` FILE; a real
     carrier states them in its own `## Done` section or with a closure
     grade word. These bodies are VERBATIM from the source, at the line
     ranges named beside each one, and they are archived rather than
     written as items: a closure that migrates back as open work is the
     one migration defect that is silent. -->

<!-- BACKLOG.md:299-325 — §4 row 1 + lc-18: ungraded, under the carrier's own closure heading — the heading is the closure statement, and the body is archived verbatim -->
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


<!-- BACKLOG.md:326-353 — §4 row 1 + lc-18: ungraded, under the carrier's own closure heading — the heading is the closure statement, and the body is archived verbatim -->
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


<!-- BACKLOG.md:354-393 — §4 row 1 + lc-18: ungraded, under the carrier's own closure heading — the heading is the closure statement, and the body is archived verbatim -->
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

