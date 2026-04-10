# Perspectives

A management skill for creating, listing, and validating custom
perspective agents used by `/discuss` and `/canvas`.

## What This Does

The `/discuss` skill ships with five built-in perspective agents
(Strategist, Architect, Delivery Lead, Experience Advocate, Innovator).
These work well for general software and business discussions, but many
domains benefit from specialized viewpoints — a clinician and regulator
for healthcare, a risk analyst and compliance officer for fintech, etc.

`/perspectives` lets you generate domain-specific perspective agents
that slot into `/discuss` and `/canvas` sessions alongside or in place
of the built-ins.

### Subcommands

- **`/perspectives list`** — show all available perspectives across
  project, user, and built-in locations
- **`/perspectives generate <domain>`** — create 3-5 domain-specific
  perspectives (e.g., `/perspectives generate healthcare`)
- **`/perspectives validate [path]`** — check perspective files against
  the required contract

### Perspective Precedence

Perspectives are loaded from three locations, highest precedence first:

1. `.claude/perspectives/` — project-local overrides
2. `~/.claude/perspectives/` — user-global custom perspectives
3. Built-in plugin defaults — the five standard perspectives

A perspective at a higher-precedence location shadows any with the same
name at a lower one.

## Installation

Copy the skill file into your project's `.claude/` directory:

```bash
# From the root of your target project:

mkdir -p .claude/skills/perspectives
cp perspectives/SKILL.md .claude/skills/perspectives/
```

Alternatively, install globally:

```bash
mkdir -p ~/.claude/skills/perspectives
cp perspectives/SKILL.md ~/.claude/skills/perspectives/
```

After installation, the skill is available as `/perspectives` in Claude
Code sessions.

### Dependencies

- **discuss** — perspectives are consumed by the `/discuss` skill (and
  by `/canvas`, which dispatches `/discuss` instances). The discuss skill
  and its built-in perspective agents should be installed for the
  generated perspectives to be useful.

## File Structure

```
perspectives/
└── SKILL.md    <- Skill definition with list, generate, and validate subcommands
```

Generated perspectives are written to `~/.claude/perspectives/` by
default, or `.claude/perspectives/` with the `--project` flag.
