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
