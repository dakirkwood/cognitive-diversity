# Discuss

A structured multi-perspective discussion skill for Claude Code.

## What This Does

`/discuss` simulates a facilitated team discussion where five
perspective agents evaluate a topic from distinct, often competing
viewpoints. The skill produces a decisions document with clear
resolution states for each subtopic discussed.

Two modes serve different lifecycle stages:

- **`/discuss propose <topic>`** — Pre-commitment. Evaluates whether
  an idea should be pursued and how it should be shaped. Perspectives
  can challenge the premise itself.
- **`/discuss plan <topic>`** — Post-commitment. Stress-tests how
  to execute work that has already been accepted. Perspectives focus
  on execution risks and better implementation approaches.

## How It Works

The skill runs as a single orchestrated process. The main Claude Code
session acts as the Arbiter — a neutral facilitator that enforces
discussion protocol, manages turn order, and synthesizes outcomes. It
has no opinions on the content being discussed.

Five perspective agents are spawned as Claude Code subagents, each
operating in its own context window:

- **Strategist** — Business sustainability and client impact
- **Architect** — Long-term technical health and system reliability
- **Delivery Lead** — Executable delivery within real constraints
- **Experience Advocate** — End-user quality and accessibility
- **Innovator** — Unexplored opportunities and unexamined assumptions

For each subtopic, the Arbiter selects 2–4 relevant perspectives and
calls on them in a deliberate order designed to maximize productive
tension. After primary discussion, remaining perspectives get a brief
room check to flag cross-cutting concerns before the subtopic closes.

## The Tradeoff: Isolation vs. Fidelity

Each perspective subagent operates in its own context window. This is
a deliberate architectural choice — isolation keeps each perspective
committed to its lens without being tempted toward consensus by seeing
the orchestration or other agents' raw output.

The cost is that **no perspective ever sees another perspective's actual
words**. The Arbiter acts as intermediary, synthesizing what others said
into bridging prompts when resuming a subagent for subsequent rounds.
This synthesis is inherently lossy. Nuance, specific phrasing, and
rhetorical force may be reduced in translation.

This mirrors how facilitated discussions work in practice — participants
hear the facilitator's synthesis, not a transcript of every word. The
quality of the debate depends on how well the Arbiter relays positions.

Subagents are resumed (not re-spawned) for subsequent turns, so each
perspective retains its own chain of reasoning across rounds even as it
receives summarized versions of others' arguments.

### Comparison to Single-Process Simulation

An earlier prototype (`discussion.md`) ran the entire discussion as a
single-process simulation where Claude played all roles within one
context window. That approach trades isolation for shared context — all
voices can see everything, which enables more fluid back-and-forth but
risks perspectives bleeding into each other and gravitating toward
agreement.

The two approaches are complementary, not competing. Single-process
simulation suits early-stage ideation where fluid exchange matters more
than committed positions. Multi-agent arbitration suits evaluation and
stress-testing where distinct, unyielding perspectives are the point.

## File Structure

```
.claude/
├── skills/
│   └── discuss/
│       ├── SKILL.md        ← Arbiter protocol and skill configuration
│       ├── propose.md      ← Behavioral overrides for propose mode
│       ├── plan.md         ← Behavioral overrides for plan mode
│       └── README.md       ← This file
├── agents/
│   └── perspectives/       ← Subagent files with YAML frontmatter
│       ├── strategist.md
│       ├── architect.md
│       ├── delivery-lead.md
│       ├── experience-advocate.md
│       └── innovator.md
```

## Installation

Copy the skill files and agent files into your project's `.claude/`
directory:

```bash
# From the root of your target project:

# Skill files
mkdir -p .claude/skills/discuss
cp discuss/SKILL.md      .claude/skills/discuss/
cp discuss/propose.md    .claude/skills/discuss/
cp discuss/plan.md       .claude/skills/discuss/

# Perspective agents
mkdir -p .claude/agents/perspectives
cp discuss/perspectives/*.md .claude/agents/perspectives/
```

After installation, the skill is available as `/discuss` in any Claude
Code session within that project.

### Dependencies

None. This skill is self-contained.

If you plan to use `--seed` or `--seeded` flags (for cultural/psychological
profile injection), the **canvas** skill's `behavioral-translation.md`
must also be installed at `.claude/skills/canvas/behavioral-translation.md`.

## Output

Each session produces a decisions document at
`docs/decisions/[mode]-[topic-slug]-[YYYY-MM-DD].md` containing the
agenda, a structured decision record for each subtopic, a parking lot
for deferred items, and a closing summary.
