# DANEEL

Evidence-based debugging for Claude Code: systematic root-cause
investigation with verification map, hypothesis-elimination, and
user-controlled scope.

DANEEL is the [Anneal framework](https://github.com/Gunther-Schulz/anneal-framework)
bound to debugging. For building new behavior, see
[Clippy](https://github.com/Gunther-Schulz/coding-clippy).

## Mission

**Root cause found, first time.** DANEEL's guiding principles:

1. **Understand Before Investigating** — know what "correct" means
   before exploring what's wrong
2. **Find Origin, Not Symptom** — trace backward to where wrongness
   enters
3. **Evidence Over Theories** — mark [VERIFIED] only with evidence;
   theories become hypotheses, not investigation directions
4. **User Steers Direction** — AI proposes next investigation, user
   approves/redirects

## Installation

```bash
claude plugin marketplace add Gunther-Schulz/daneel
claude plugin install daneel@daneel
```

Run `/reload-plugins` to activate.

## Updating

```bash
claude plugin marketplace update daneel
claude plugin update daneel@daneel
```

Then `/reload-plugins`.

## Usage

Trigger DANEEL with phrases like:
- "daneel" / "use daneel"
- "debug X" / "investigate why X is wrong"
- "find the root cause of Y"

DANEEL runs through three phases (investigate-design / implement /
verify) mirroring the framework's pipeline, specialized for debugging
flow:

- **investigate-design** — hypothesis enumeration + systematic
  elimination, ending at [READY] with root cause classified and fix
  approach selected
- **implement** — simple fixes complete inline (1-3 components,
  no architectural change); complex fixes hand off to Clippy
- **verify** — confirm fix corrects observed wrong behavior

## Components

| Component | Purpose |
|---|---|
| `plugin/skills/daneel/` | The DANEEL skill (rendered from framework spec) |
| `legacy/DANEEL.md` | Prior standalone protocol (reference only) |

## Architecture

```
anneal-framework (spec/)
        ↓ renders to
   ┌────────────┐
   │   Clippy   │  building new behavior
   ├────────────┤
   │   DANEEL   │  debugging wrong behavior
   └────────────┘
        ↓
   coding (domain)
```

Both instances inherit the framework's structural discipline (basis
rule, status-state machine, phase pipeline). They differ in their
domain binding and lens set.

## Development

The skill content is rendered from the framework spec, not authored
here. See `CLAUDE.md` for repository housekeeping rules and the
render workflow.

## License

MIT
