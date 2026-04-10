# Cognitive Diversity Skills

Multi-perspective deliberation protocols for [Claude Code](https://claude.ai/code). Five commands that use your existing Claude subscription to examine ideas, plans, and experiences through diverse cognitive lenses.

Cognitive Diversity creates productive deliberation in a synthetic environment about whatever topic you provide. Walk an idea through several modes intended to mimic the deliberation process from within one person's mind (`/brainstorm`), a presentation to several people (`/discuss`), to the reaction across different cultural compositions (`/canvas`). You can get initial thoughts on how people with different psychological profiles may respond to a single task (`/focus-group`). All commands produce a final report summarizing the activity to aid your own decision-making process.

**IMPORTANT:** This is not intended as caricature or a replacement for real people; it is simply a tool to help you surface considerations you may otherwise miss.

## Commands

| Command | Purpose |
|---------|---------|
| `/brainstorm` | Three-lens ideation: Dreamer, Realist, Arbiter |
| `/discuss` | Multi-perspective structured discussion with 5 perspective agents |
| `/canvas` | Culturally-seeded discussion sessions with cross-group synthesis |
| `/focus-group` | Cognitive profile usability screening with 7 persona agents |
| `/perspectives` | Generate, list, and validate custom domain-specific perspectives |

## Installation

Clone the repo:

```bash
git clone https://github.com/dakirkwood/cognitive-diversity-skills.git ~/plugins/cognitive-diversity-skills
```

Add to `~/.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "dk-cognitive-diversity": {
      "source": {
        "source": "directory",
        "path": "~/plugins/cognitive-diversity-skills"
      }
    }
  },
  "enabledPlugins": {
    "cognitive-diversity-skills@dk-cognitive-diversity": true
  }
}
```

## How It Works

### `/brainstorm` — One mind, three lenses

This is the deliberation process inside one person's head. Within a single context window, the LLM plays three roles: Dreamer, Realist, and Arbiter. The Dreamer represents the optimism of the idea and its possibilities. The Realist is the devil's advocate, reminding the Dreamer those possibilities are nothing if they can't be sustained. The Arbiter represents one's higher consciousness, tasked with finding the right balance.

Both the Dreamer and Realist have full access to the other's comments — every utterance is visible to all subsequent utterances. This creates a tightly integrated exchange where the voices genuinely inform each other, but less divergence by nature. When you're ready to pressure-test the idea more aggressively, the next move is to see what other people think.

```
/brainstorm We need a way to onboard new developers faster
```

Output: `docs/brainstorms/[topic-slug]-[YYYY-MM-DD].md` — an idea brief with strengths, open questions, needed research, and emergent threads.

### `/discuss` — Several people with competing priorities

This approximates the reactions of people related by a common mission but with competing priorities. The architecture shifts to a multi-agent design. The main window is a neutral Arbiter who creates subtopics for the group to discuss, decides which perspectives are most relevant to each subtopic, calls on those agents for their initial response, and determines whether there is convergence or if the decision needs to be made by the owner (you).

Each perspective is a subagent with its own context window and no direct access to the other agents' transcripts. The Arbiter summarizes the responses from one agent and feeds them to the others for their reactions. This is, by nature, a lossy process — but it is not unlike real conversation. In a real meeting room, if participants were asked to repeat what the last speaker said, none of them would repeat it word for word. They would paraphrase, and each would paraphrase differently.

The default perspectives are based on a technology company (Strategist, Architect, Delivery Lead, Experience Advocate, Innovator), but `/perspectives generate <domain>` lets you create perspectives for any domain — healthcare, legal services, education, and so on.

There are two modes:

- **`propose`** — the mindset of people discussing whether to buy into the idea. "Should we do this? How should we shape it?"
- **`plan`** — the mindset of how to implement it. "How do we execute this well?"

```
/discuss propose Should we adopt a monorepo for our frontend packages?
/discuss plan @docs/migration-plan.md
```

Output: `docs/decisions/[mode]-[topic-slug]-[YYYY-MM-DD].md` — decisions with rationale, alternatives considered, and items needing owner input.

### `/canvas` — The same idea across different cultures

How might the idea be received by groups guided by different cultural principles? Canvas adds variability by modifying the behavior of the Arbiter role with one of 9 included cultural archetype compositions. Each archetype is based on the Hofstede dimensions and affects how the Arbiter facilitates each discussion — call order, consensus vs. competition framing, tolerance for ambiguity, time horizon emphasis, and agenda discipline.

To deepen the variability, each perspective also receives a psychological score based on the OCEAN (Big Five) model. This added dimension affects how each agent argues their position — not what they care about, but how assertively, cautiously, or collaboratively they make their case.

The `/canvas` command creates 3-5 different `/discuss` prompts that you run individually, each in its own Claude Code session. Once all discussions have finished (each can last 15-30 minutes), you run `/canvas synthesize` referencing the output directory, and an Orchestrator agent pulls the results together into a single cross-group report identifying convergence, divergence, and patterns only visible across groups.

```
/canvas --output-mode interpret API versioning strategy
# ... run the generated /discuss commands in separate sessions ...
/canvas synthesize docs/canvas/api-versioning-2026-03-29/
```

Output: `docs/canvas/[topic-slug]-[YYYY-MM-DD]/canvas-synthesis.md`

### `/focus-group` — Cognitive profile screening

Uses Claude Code's Agent Teams feature. The main window spawns 7 agents representing 4 Big Five composites and 3 neurodivergent advocates, assigns them a task with randomized secondary attributes (device, urgency, tech comfort, etc.), and synthesizes the results.

Three modes: navigate a website (Playwright MCP required), evaluate a document, or reason through a scenario.

```
/focus-group --mode browser https://example.com "Complete the checkout process"
/focus-group --mode document onboarding-email.md "Follow these instructions to set up your account"
```

Output: `docs/focus-groups/[target-slug]-[YYYY-MM-DD].md` — consensus findings, profile-specific issues, severity ratings, and tied-to-evidence recommendations.

### `/perspectives` — Custom domain perspectives

The built-in perspectives suit a technology company. For other domains, generate a custom roster:

```
/perspectives generate healthcare
```

This creates 3-5 domain-specific perspectives (e.g., clinician, regulator, patient-advocate) that `/discuss` and `/canvas` will pick up automatically. Use `--roster` to select specific perspectives for a session:

```
/discuss propose --roster clinician,regulator,patient-advocate "EHR integration strategy"
```

Perspectives are loaded from three locations (highest precedence first):

1. `.claude/perspectives/` in the current working directory (project-local)
2. `~/.claude/perspectives/` (user-global)
3. `agents/perspectives/` in this plugin (built-in defaults)

## Architecture

- **Skills** (`skills/`) define the protocols — session structure, output formats, behavioral rules
- **Agents** (`agents/`) define perspective identities and subagent behaviors
- **Commands** (`commands/`) provide the `/command` entry points

The plugin uses only your Claude subscription. No external APIs, no MCP servers, no additional costs.

## Requirements

- Claude Code CLI, desktop app, or IDE extension
- For `/focus-group` browser mode: `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` enabled in settings

## License

MIT
