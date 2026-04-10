# Brainstorm

A single-process ideation skill for Claude Code that examines a topic
through three cognitive lenses: Dreamer, Realist, and Arbiter.

## What This Does

`/brainstorm` runs a structured brainstorming session where Claude plays
three roles within a single conversation. Unlike multi-agent skills,
all voices share the same context window, enabling fluid back-and-forth
where each lens directly builds on what the others said.

Two input modes are inferred automatically:

- **Problem Exploration** — broad input describing a need or challenge.
  Generates multiple viable solution directions.
- **Solution Deep-Dive** — specific input describing a particular
  approach. Fleshes the solution into a concrete shape with identified
  strengths and gaps.

### The Three Lenses

- **Dreamer** — explores possibility and vision at full potential
- **Realist** — tests feasibility against real-world constraints
- **Arbiter** — facilitates the session, gathers research, and
  synthesizes outcomes (neutral, no opinions on content)

## Installation

Copy the skill files into your project's `.claude/` directory:

```bash
# From the root of your target project:

mkdir -p .claude/skills/brainstorm
cp brainstorm/SKILL.md     .claude/skills/brainstorm/
cp brainstorm/dreamer.md   .claude/skills/brainstorm/
cp brainstorm/realist.md   .claude/skills/brainstorm/
cp brainstorm/arbiter.md   .claude/skills/brainstorm/
```

Alternatively, install globally so the skill is available across all
projects:

```bash
mkdir -p ~/.claude/skills/brainstorm
cp brainstorm/SKILL.md     ~/.claude/skills/brainstorm/
cp brainstorm/dreamer.md   ~/.claude/skills/brainstorm/
cp brainstorm/realist.md   ~/.claude/skills/brainstorm/
cp brainstorm/arbiter.md   ~/.claude/skills/brainstorm/
```

After installation, the skill is available as `/brainstorm` in any
Claude Code session.

### Dependencies

None. This skill is self-contained.

## Output

Each session produces an idea brief at
`docs/brainstorms/[topic-slug]-[YYYY-MM-DD].md` containing the
synthesized idea, strengths, open questions, recommended next steps,
needed research, and emergent threads worth further exploration.

## File Structure

```
brainstorm/
├── SKILL.md       <- Arbiter protocol and skill configuration
├── dreamer.md     <- Dreamer persona definition
├── realist.md     <- Realist persona definition
└── arbiter.md     <- Arbiter persona definition
```
