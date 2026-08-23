# Repository housekeeping for the DANEEL plugin

Rules for maintaining the DANEEL plugin itself.

## DANEEL is independent; its skill content is authored here

**Operator decision 2026-08-23.** DANEEL derives historically from
the Anneal framework, and is now independent of it. The
`anneal-framework` repo is gone — measured, not assumed: it is
absent from the filesystem, so every citation into it was dangling,
not merely stale.

So there is no render contract and no upstream to route through.
All skill content — `plugin/skills/daneel/SKILL.md`, `phases/`,
`references/` — is **authored and maintained in this repo**. A
change to how DANEEL behaves is made here, invoking `skill-craft`
first per "Rule-corpus edits" below, and cites its own incident as
origin.

DANEEL's `spec/` (`spec/bindings.md`, `spec/lens-set.md`,
`spec/debugging-disciplines.md`) carries the domain binding and
renders into the plugin files; that relationship is internal to
this repo and unchanged. Every load-bearing rule in the plugin
originates either in this repo's `spec/` or in the plugin file
itself, with its incident cited. A rule with no traceable origin is
drift.

Anneal may be named as historical origin. It must never be named as
a live source: a pointer into a repo that does not exist reads as a
route a later session will try to take.

This rule covers the skill *content*. The plugin's packaging — this
file, the READMEs, `plugin.json`, `marketplace.json`, `legacy/` — is
repo-local, maintained here directly under the rules below.

## Description sync (single source of truth)

These three texts must match each other and reflect the current
top-level `README.md` framing:

- `.claude-plugin/marketplace.json` → `plugins[0].description`
- `plugin/.claude-plugin/plugin.json` → `description`
- `README.md` → opening paragraph

When one changes, all three change.

## Component inventory sync

When a skill, agent, command, or hook is added, renamed, or removed
under `plugin/`, update in the same commit:

- Top-level `README.md` → Components table

Discrepancy is a bug. Quick sanity check before any release:

```bash
ls plugin/skills/ plugin/agents/ plugin/commands/ plugin/hooks/
# Cross-check against the README table.
```

## Version discipline

Bump `plugin/.claude-plugin/plugin.json` → `version` before pushing a
release. The marketplace caches by version — a push without a version
bump won't propagate via `claude plugin update`.

## No specific model names in user-facing text

`README.md`, `marketplace.json`, and `plugin.json` must not name
Claude models (Sonnet, Opus, Haiku) in component descriptions. Agent
frontmatter pins models for runtime — that's internal and stays.

## Legacy content

Files under `legacy/` (`DANEEL.md`) are reference-only. Do not:

- Link from current docs (README, marketplace, plugin descriptions)
- Suggest "attach-to-chat" from these paths
- Update or treat as authoritative

The `legacy/DANEEL.md` carries the prior standalone protocol that
this plugin replaces. It serves as the source material for the
DANEEL-specific content (D2-D6 debugging disciplines, lens set
naming) rendered into the instance.

## Relationship to Clippy

DANEEL and Clippy are sibling instances of the same framework. They
share the framework's structural discipline (basis rule, status-state
machine, phase pipeline, closed-artifact form) and differ in their
domain binding:

- **Clippy**: building new behavior in coding
- **DANEEL**: debugging wrong behavior in coding

When DANEEL identifies a complex fix that requires architectural
changes, it hands off to Clippy's implement phase via explicit
cross-instance handoff. The framework's loopback semantics govern
this transition.

## Improvement journal

`dev-notes/OBSERVATIONS.md` is this repo's improvement journal: the
provenance of every minted lens and basis-rule clause, and the
firing log that later decides whether a capability patch has earned
its place (skill-craft, Lifecycle and Durability classes).

It is a **maintenance file — a write target, never a read
dependency.** No operational file (`SKILL.md`, `phases/`,
`references/`) points at it, and none may: the journal must not
load during a run. Naming it here, as a role, is not a pointer on
any operational read path.

Mint a rule, land its entry in the same commit.

## Rule-corpus edits

When editing skill-craft or instance skills (clippy / daneel /
etc.): invoke the `skill-craft` skill via the Skill tool BEFORE
the edit.

Apply Edit-as-Pareto-improvement: name what the edit reduces or
consolidates. If nothing — the addition is suspect per the
Additive reflex anti-pattern (skill-craft/references/anti-patterns.md).

**Recursion check**: rule-edit subagent PASS may self-validate.
Pause + re-read before push.
