---
description: >
  Run a simulated cognitive profile focus group against a URL, document,
  or scenario.
---

# Focus Group Command

Simulate how people with different cognitive profiles experience a task, document, or scenario. Uses a team of persona agents to surface friction, accessibility issues, and usability gaps.

## Usage

```
/focus-group [flags] <target> "<task description>"
```

**Flags:**

| Flag | Values | Default | Purpose |
|------|--------|---------|---------|
| `--mode` | `browser`, `document`, `scenario` | Inferred from target | Interaction mode |
| `--personas` | `all`, comma-separated names | `all` | Subset of personas |
| `--depth` | `shallow`, `deep` | `deep` | Scope of exploration |
| `--output` | `summary`, `full` | `full` | Report detail level |
| `--compare` | Second URL or file | None | A/B comparison |

**Examples:**
```
/focus-group --mode browser https://example.edu "Find spring 2026 enrollment information"
/focus-group --mode document onboarding-email.md "Follow these instructions to set up your account"
/focus-group --mode scenario "You're choosing between two project management tools based on these feature lists" "Select the right tool for a 5-person remote team"
/focus-group --mode browser --personas methodical-user,adhd-profile https://example.com "Complete the checkout process"
```

## Mode Inference

If `--mode` is not specified:
- Target begins with `http://` or `https://` → `browser`
- Target is a file path ending in `.md`, `.pdf`, `.txt`, `.html` → `document`
- Target is a plain text description → `scenario`

## Task Validation

The facilitator will reject tasks that are too vague to evaluate. A valid task requires:
- A **specific goal** ("Find spring 2026 enrollment info" not "explore the site")
- A **defined success state** (what does completion look like?)
- A **specific target** (URL, file, or concrete scenario description)

If your task is rejected, you will be asked to clarify before the run proceeds.

## Delegation

This command delegates entirely to the `focus-group-facilitator` agent, which:
1. Validates the task and infers mode if not specified
2. Assigns randomized secondary attributes to each persona
3. Spawns persona teammates in parallel via Agent Teams
4. Collects all persona reports and synthesizes the final output
5. Writes the report to `docs/focus-groups/`

## Persona Roster

**Big Five Composites:**
- `methodical-user` — High Conscientiousness: reads instructions, follows expected flows
- `impatient-scanner` — Low Conscientiousness: skims, wants fastest path
- `anxious-navigator` — High Neuroticism: hesitant, needs reassurance
- `curious-explorer` — High Openness: tries unusual paths, tolerates complexity

**Neurodivergent Advocates:**
- `adhd-profile` — Attention/executive function: distracted by noise, needs visual hierarchy
- `autism-profile` — Pattern/literal processing: frustrated by ambiguity and inconsistency
- `dyslexia-profile` — Reading/decoding: relies on visual cues, struggles with dense text

## Output

Report written to: `docs/focus-groups/[target-slug]-[YYYY-MM-DD].md`

Requires `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` enabled in settings.json.
