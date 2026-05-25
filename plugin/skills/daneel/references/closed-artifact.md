# Closed-artifact form

Load when preparing the closed artifact at any cycle or phase
boundary (`SKILL.md` Modes — interactive).

The closed artifact contains these named sections in this order.
Each section is required; missing or out-of-order sections make
the artifact malformed:

1. **State summary** — what the operator decides on first:
   counts (findings + decisions by status), the last cycle's
   standardized-pass status (clean or line items), the
   fresh-session implementability result line (PASSED / FAILED
   per `phases/investigate-design.md`), named blockers
   preventing [READY] (open [PENDING] decisions, weak-basis
   tracker entries, incomplete hypothesis list). Decision-
   relevant content first; detail follows.
2. **Inventories** — findings (verification map) and
   hypothesis verdicts + fix approach (design decisions) as
   scannable one-per-line ledger entries with status tag and
   identifier at the start; not paragraph-prose summaries.
3. **Persisted artifacts** — citations to where detail lives
   (the tracker file at `.daneel/runs/<run>.md`, the
   standardized-pass artifact alongside). The closed artifact
   references; it does not paragraph-summarize.
4. **Recommendation** — the AI's next-step proposal
   (thorough-fix-shaped per `foundations.md` Recommendation
   discipline); rationale and any "not recommended because…"
   callouts in this section. **For each open [CONDITIONAL]
   decision, the AI's committed default surfaces crisply** —
   what the operator accepts by selecting proceed; the
   [CONDITIONAL] then records [AUTO-ACCEPTED] (`tracker.md`).
   For [READY] specifically, the recommendation includes: the
   established root cause, the selected fix approach (simple
   inline / complex handoff to Clippy), and the impact-trace
   assessment. Operator's free-form override against the
   tracker is available at any moment.
5. **Menu** — continue / proceed only; plain; no inline
   annotations except a single `← recommended` tag on the
   option matching section 4's recommendation. The tag is
   mandatory: every menu has exactly one tagged option.

**Presentation.** Search commands, file paths, tracker
citations, code locations are presented as code, not buried in
prose.

Operator review at presentation is the catcher.
