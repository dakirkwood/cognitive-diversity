# Focus Group

A simulated cognitive profile focus group skill for Claude Code. Spawns
a team of persona agents representing different cognitive profiles to
evaluate a URL, document, or scenario for usability and accessibility.

## What This Does

`/focus-group` simulates how people with different cognitive profiles
experience a task. A facilitator agent manages a team of persona agents,
each running as an independent process, and synthesizes their findings
into a structured usability report.

Three interaction modes:

- **`browser`** — navigates a live URL using Playwright
- **`document`** — evaluates a markdown, PDF, or text file
- **`scenario`** — assesses a described situation without a concrete artifact

```
/focus-group https://example.edu "Find spring 2026 enrollment information"
/focus-group --mode document onboarding-email.md "Follow these instructions to set up your account"
/focus-group --personas methodical-user,adhd-profile https://example.com "Complete the checkout process"
```

### Persona Roster

**Big Five Composites:**
- `methodical-user` — High Conscientiousness: reads instructions, follows expected flows
- `impatient-scanner` — Low Conscientiousness: skims, wants fastest path
- `anxious-navigator` — High Neuroticism: hesitant, needs reassurance
- `curious-explorer` — High Openness: tries unusual paths, tolerates complexity

**Neurodivergent Advocates:**
- `adhd-profile` — Attention/executive function challenges
- `autism-profile` — Pattern/literal processing, frustrated by ambiguity
- `dyslexia-profile` — Reading/decoding challenges, relies on visual cues

## Installation

Copy the command, agent, and skill files into your project's `.claude/`
directory:

```bash
# From the root of your target project:

# Command (registers /focus-group)
mkdir -p .claude/commands
cp focus-group/commands/run.md .claude/commands/focus-group.md

# Facilitator agent
mkdir -p .claude/agents
cp focus-group/agents/focus-group-facilitator.md .claude/agents/

# Skill reference and personas
mkdir -p .claude/skills/focus-group/personas
cp focus-group/skills/focus-group/SKILL.md      .claude/skills/focus-group/
cp focus-group/skills/focus-group/REFERENCE.md   .claude/skills/focus-group/
cp focus-group/skills/focus-group/personas/*.md   .claude/skills/focus-group/personas/
```

### Configuration

Focus Group uses Claude Code's experimental Agent Teams feature. Enable
it in your project's `.claude/settings.json`:

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

### Dependencies

- **Playwright MCP** — required for `browser` mode. The Playwright MCP
  server must be configured in your Claude Code settings.
- No dependency on other skills in this collection.

## Output

Each run produces a report at
`docs/focus-groups/[target-slug]-[YYYY-MM-DD].md` containing consensus
findings, individual persona journeys, severity ratings, and actionable
recommendations.

## File Structure

```
focus-group/
├── commands/
│   └── run.md                           <- Command definition
├── agents/
│   └── focus-group-facilitator.md       <- Facilitator agent
└── skills/
    └── focus-group/
        ├── SKILL.md                     <- Synthesis patterns and templates
        ├── REFERENCE.md                 <- Mode-specific behavioral guidance
        └── personas/
            ├── fg-adhd-profile.md
            ├── fg-anxious-navigator.md
            ├── fg-autism-profile.md
            ├── fg-curious-explorer.md
            ├── fg-dyslexia-profile.md
            ├── fg-impatient-scanner.md
            └── fg-methodical-user.md
```
