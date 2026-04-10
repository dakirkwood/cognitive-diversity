# Canvas

A meta-discussion skill for Claude Code that runs multiple parallel
discussion sessions on the same topic, each seeded with a distinct
cultural and psychological profile, then synthesizes findings across
all groups.

## What This Does

`/canvas` orchestrates N parallel `/discuss` (arbitrate) instances,
each seeded with a different cultural archetype and set of Big Five
personality profiles. After all instances complete, an Orchestrator
subagent reads the reports and produces a cross-group synthesis
identifying convergence, divergence, and emergent patterns.

Canvas depends on the **discuss** skill for its parallel instances.

Two output modes:

- **`synthesize`** (default) — Orchestrator identifies patterns without
  offering its own position
- **`interpret`** — all of synthesize, plus the Orchestrator adds its
  own assessment and may propose directions

```
/canvas microservices vs. monolith for our new platform
/canvas interpret --instances 4 --mode plan API versioning strategy
/canvas --seed docs/canvas/previous-run/seed-profiles.json same topic again
```

## Installation

Copy the skill files, orchestrator agent, and the required arbitrate
dependency into your project's `.claude/` directory:

```bash
# From the root of your target project:

# Canvas skill files
mkdir -p .claude/skills/canvas
cp canvas/SKILL.md                    .claude/skills/canvas/
cp canvas/archetypes.json             .claude/skills/canvas/
cp canvas/behavioral-translation.md   .claude/skills/canvas/
cp canvas/seed-schema.json            .claude/skills/canvas/

# Canvas orchestrator agent
mkdir -p .claude/agents
cp canvas/canvas-orchestrator.md      .claude/agents/
```

### Dependencies

Canvas dispatches `/discuss` instances, so the **discuss** skill and
its perspective agents must also be installed:

```bash
# Discuss skill files
mkdir -p .claude/skills/discuss
cp discuss/SKILL.md      .claude/skills/discuss/
cp discuss/propose.md    .claude/skills/discuss/
cp discuss/plan.md       .claude/skills/discuss/

# Perspective agents
mkdir -p .claude/agents/perspectives
cp discuss/perspectives/*.md .claude/agents/perspectives/
```

## Output

Each run creates a directory at
`docs/canvas/[topic-slug]-[YYYY-MM-DD]/` containing:

```
docs/canvas/topic-slug-2026-04-10/
├── seed-profiles.json          <- Reproducible seed (written before dispatch)
├── reports/
│   ├── group-a-report.md       <- Individual group decision documents
│   ├── group-b-report.md
│   └── group-c-report.md
├── transcripts/
│   ├── group-a-transcript.md   <- Raw perspective outputs
│   ├── group-b-transcript.md
│   └── group-c-transcript.md
└── canvas-synthesis.md         <- Cross-group synthesis
```

Pass `--seed` with a previous run's `seed-profiles.json` to reproduce
the same cultural/personality configuration.

## File Structure

```
canvas/
├── SKILL.md                   <- Run coordinator protocol
├── archetypes.json            <- Cultural archetype pool
├── behavioral-translation.md  <- Maps seed values to agent behavior
├── canvas-orchestrator.md     <- Orchestrator subagent definition
└── seed-schema.json           <- Seed profile JSON schema
```
