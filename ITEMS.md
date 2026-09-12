schema: 2
baseline: 10
added: 1
compacted: 0

## da-1
grade: NEW
requirement: PARKED — The loop has a termination condition but no CONVERGENCE signal — record: BACKLOG.md:15
goal: UNKNOWN
write-set: UNKNOWN
done-criterion: UNKNOWN
evidence: BACKLOG.md:15-49
blocked-by: decision regrade: fill goal, write-set, done-criterion and evidence, or drop

## da-2
grade: NEW
requirement: PARKED — Auto-battle mode documentation — record: BACKLOG.md:50
goal: UNKNOWN
write-set: UNKNOWN
done-criterion: UNKNOWN
evidence: BACKLOG.md:50-57
blocked-by: decision regrade: fill goal, write-set, done-criterion and evidence, or drop

## da-3
grade: NEW
requirement: PARKED — Effectiveness unmeasured against a bare model — record: BACKLOG.md:58
goal: UNKNOWN
write-set: UNKNOWN
done-criterion: UNKNOWN
evidence: BACKLOG.md:58-123
blocked-by: decision regrade: fill goal, write-set, done-criterion and evidence, or drop

## da-4
grade: NEW
requirement: READY — Narrow DANEEL to explicit invocation — record: BACKLOG.md:124
goal: UNKNOWN
write-set: UNKNOWN
done-criterion: UNKNOWN
evidence: BACKLOG.md:124-148
blocked-by: decision regrade: was READY under the old carrier: READY is judged, never inherited

## da-5
grade: NEW
requirement: READY — Replay the 2026-08-23 batch once a run has used it — record: BACKLOG.md:149
goal: UNKNOWN
write-set: UNKNOWN
done-criterion: UNKNOWN
evidence: BACKLOG.md:149-160
blocked-by: decision regrade: was READY under the old carrier: READY is judged, never inherited

## da-6
grade: NEW
requirement: PARKED — Mechanize the discharge-presence half of the [READY] gate — record: BACKLOG.md:161
goal: UNKNOWN
write-set: UNKNOWN
done-criterion: UNKNOWN
evidence: BACKLOG.md:161-178
blocked-by: decision regrade: fill goal, write-set, done-criterion and evidence, or drop

## da-8
grade: READY
requirement: Three dated JOURNAL entries sit in the frozen BACKLOG.md '## Open' section and have no home in the new carrier — record: docs/audits/migration-report-2026-09-12.md, 'Non-entry bullets — prose, not migrated'
goal: tend
write-set: dev-notes/OBSERVATIONS.md, BACKLOG.md (deletion)
done-criterion: the three bodies are in dev-notes/OBSERVATIONS.md under a lens section, byte-identical to the source, and BACKLOG.md is deleted
evidence: BACKLOG.md:179, :198, :239 — each a date-led bullet (2026-08-23) whose bold begins AFTER the date, so it is neither bold-led nor grade-word-led and was correctly read as prose by the migration. Content is release/batch narrative with commit refs (4ee1840, 894aa3e, 6c0f432, 074737d) and extracted class lessons — records of completed work, not work items. Bullet identity in the report: 12 top-level bullets = 9 entries + 3 prose + 0 cut, HOLDS. The journal is lens-structured (## 1..## 9), not chronological, and the three span measurement-floor, instrument-fitness and the stale-premise class.
blocked-by: decision which lens section of dev-notes/OBSERVATIONS.md each of the three entries belongs under
amend-reason: 2026-09-12 2026-09-12 Drainage desk ruling, and the lens question is DISSOLVED rather than answered. The blocker asked which lens section each of the three bodies belongs under. Measured at the artifact today: five of the six content halves in those three bullets ALREADY have a home in dev-notes/OBSERVATIONS.md, placed by the very commits the bullets themselves cite. Copying them in byte-identically would set a second body beside each existing one, in the file whose reader decides whether a lens has earned its place -- two bodies for one fact, which is paraphrase-drift with divergence guaranteed. So the move is DROPPED. The measurement did find one half with no home anywhere in either repo, and that is the only real work this item carries.
amended-write-set: 2026-09-12 BACKLOG.md (deletion), ITEMS.md (cross-repo booking pointer), CLAUDE.md (deletion record)
amended-done-criterion: 2026-09-12 The move is DROPPED -- no body is copied into dev-notes/OBSERVATIONS.md, because five of six halves are already homed there and the sixth belongs to another repo. What this item now delivers: (1) the coverage mapping in the evidence slot above, recorded in-entry so the deletion is provably lossless for daneel own content; (2) the one uncovered half booked as a cross-repo item whose write-set names skill-craft own carrier, a pointer left on both sides per the deployment-vs-system backlog split -- daneel cannot write skill-craft, so that write stays the SENDER obligation and is named as such rather than assumed delivered; (3) BACKLOG.md deleted behind the arc standing preconditions: citations re-rooted to carry the pinned source blob inline, inbound dependents search stated with command and hits, deletion record in CLAUDE.md naming the git cat-file resolution path. Verifier for (1): each mapping line opened at its cited OBSERVATIONS.md range. Verifier for (3): a citation resolved after the delete, plus a deliberately wrong blob shown NOT to resolve, so the resolution path is proven live rather than asserted.
amended-evidence: 2026-09-12 COVERAGE MEASURED 2026-09-12 at dev-notes/OBSERVATIONS.md, per content half, each sweep run with a positive control. BACKLOG.md:179 (0.2.64, the F47 re-citation and the basis-rot class) -> OBSERVATIONS.md:800-817, carrying both the F47 replacement and the sentence that evidence can be overturned underneath a rule that stays true. BACKLOG.md:198 firing half (the false-zero grep whose phrase spanned a hard wrap) -> OBSERVATIONS.md:1036-1050, logged there as the first live firing of the 2026-08-23 batch. BACKLOG.md:198 rule-defect half (zero firings means two different things; the discriminator is run count, not firing count) -> OBSERVATIONS.md:18-38, the journal HEADER, exactly where the bullet says 074737d placed it. BACKLOG.md:239 accounts half -> OBSERVATIONS.md:1063+ (section 14, Elimination never produces an explanation). BACKLOG.md:239 vantage half -> OBSERVATIONS.md:345+ (section 9). NOT COVERED ANYWHERE, in either repo: the BACKLOG.md:198 cross-repo half -- that skill-craft Durability-classes carries the same cut-candidate predicate and inherits the same defect, surfaced deliberately and not patched there under skill-craft own Reflexivity rule. Sweep over skill-craft for the defect formulation (zero runs / unmeasured / has anything exercised / run count / entered its scope) returned only unrelated senses of unmeasured; the positive control on the same instrument (cut candidate) returned 6 hits, so instrument and scope are both sound. skill-craft dev-notes/OBSERVATIONS.md:1240-1242 records the opposite verdict -- the cut-candidate gap CLOSED-ALREADY-SATISFIED because the rule text exists -- which is precisely the predicate daneel measured as defective. RELEASE PROVENANCE residue, named rather than left silent: none of the seven cited commit refs appears in OBSERVATIONS.md and 0.2.65 appears nowhere, so the version-to-commit mapping lives only in these bullets and in git history; the arc standing precondition (citations re-rooted carrying the source blob inline) keeps it resolvable by git cat-file, so nothing is lost that the precondition does not already cover.
amended-blocked-by: 2026-09-12 NONE
