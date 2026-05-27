# Repository housekeeping for the DANEEL plugin

Rules for maintaining the DANEEL plugin itself.

## The skill content is rendered, not authored

DANEEL is an *instance* of the Anneal framework — parallel to
Clippy. The skill files — `plugin/skills/daneel/SKILL.md`, `phases/`,
and `references/` — are **rendered** from the framework spec (the
`anneal-framework` repo, `spec/`). They are not authored here, and
are never where a behavior change originates.

A change to how DANEEL behaves goes to the framework spec first:
committed there, then re-rendered into these files and verified in a
separate context — the procedure is `development-process.md` in the
`anneal-framework` repo. Hand-editing a skill file as if it were
source breaks re-derivability: the spec and the instance drift, and
the change cannot be reproduced for another instance.

DANEEL-specific content — the bindings, the lens set, and the
debugging disciplines — lives in this repo's `spec/` directory
(`spec/bindings.md`, `spec/lens-set.md`,
`spec/debugging-disciplines.md`), and renders into the instance's
plugin files. The framework spec governs the structural discipline;
DANEEL's `spec/` carries the domain binding. Per contract 2 in
anneal-framework `development-process.md` "The three levels", every
load-bearing rule in the plugin originates in one of two specs —
framework spec OR DANEEL's `spec/`. A plugin edit without a cited
spec origin is drift.

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

## Rule-corpus edits

When editing skill-craft, anneal-framework spec, or instance
skills (clippy / daneel / etc.): invoke the `skill-craft` skill
via the Skill tool BEFORE the edit.

Apply Edit-as-Pareto-improvement: name what the edit reduces or
consolidates. If nothing — the addition is suspect per the
Additive reflex anti-pattern (skill-craft/references/anti-patterns.md).

**Recursion check**: rule-edit subagent PASS may self-validate.
Pause + re-read before push.
