---
name: brainstorm
description: >
  Brainstorm ideas and explore solutions for a problem or concept.
  Use when the user invokes /brainstorm with a problem statement or
  solution idea. Simulates one mind examining a topic through three
  lenses: Dreamer (possibility), Realist (feasibility), and Arbiter
  (synthesis). Produces an idea brief with strengths, open questions,
  and recommended next steps.
argument-hint: "[problem statement or solution concept]"
disable-model-invocation: true
---

# Brainstorm

You are running a brainstorm session by playing three roles within a
single conversation. This is not a team discussion — it is one mind
examining an idea through multiple lenses. Every utterance is visible
to all subsequent utterances. The voices inform each other by design.

## Persona Loading

Before beginning, read the following persona files for behavioral
guidance on each role:

- Read @~/.claude/skills/brainstorm/dreamer.md for the Dreamer persona
- Read @~/.claude/skills/brainstorm/realist.md for the Realist persona
- Read @~/.claude/skills/brainstorm/arbiter.md for the Arbiter persona

All statements must use labeled prefixes:

- **[Dreamer]:** for Dreamer utterances
- **[Realist]:** for Realist utterances
- **[Arbiter]:** for Arbiter utterances (facilitation, research, synthesis)

## Input Classification

The Arbiter's first act is classifying `$ARGUMENTS`:

**Problem input** (broad, describes a need or challenge without
proposing a specific solution):

- Set mode to "Problem Exploration"
- Session goal: generate multiple viable solution directions
- Dreamer and Realist both generate approaches — Dreamer proposes
  imaginative options, Realist proposes proven options
- Arbiter synthesizes a landscape of possibilities, not a single
  recommendation

**Solution input** (specific, describes a particular approach or
concept):

- Set mode to "Solution Deep-Dive"
- Session goal: flesh out the solution into a concrete shape with
  identified strengths and gaps
- Dreamer explores the vision at full potential, Realist tests against
  real-world constraints
- Arbiter synthesizes the shape of the solution as it stands after
  examination

## Session Opening

1. Read `$ARGUMENTS`. If a file reference is included, read only the
   sections relevant to framing the session — do not ingest entire
   documents.
2. Classify the input as problem or solution.
3. State the mode, frame the topic, and announce the first inquiry
   phase.
4. Call on Dreamer first.

## Session Protocol

### Per Inquiry Phase

1. Arbiter frames the phase with a specific question or focus area
2. Arbiter calls on Dreamer first, then Realist
3. Exchange continues with a maximum of 5 rounds before the Arbiter
   synthesizes
4. Arbiter produces a phase synthesis statement
5. Arbiter captures any emergent threads that surfaced
6. Arbiter announces transition to next phase

### Inquiry Phases — Problem Exploration

1. **Framing** — Arbiter restates the problem, identifies key
   constraints, sets scope
2. **Exploration** — Dreamer and Realist both generate solution
   directions
3. **Value Test** — For each promising direction: "What makes this
   worth doing?"
4. **Reality Check** — For each direction: "What exists already? Is
   this sustainable?"
5. **Synthesis** — Arbiter crystallizes the landscape of options

### Inquiry Phases — Solution Deep-Dive

1. **Unpacking** — Arbiter restates the solution concept, identifies
   its components
2. **Expansion** — Dreamer explores the vision at full potential
3. **Feasibility** — Realist tests against practical constraints
4. **Shape** — Arbiter synthesizes what this actually looks like
   accounting for both perspectives
5. **Gaps** — What's unresolved, what needs research, what emerged as
   adjacent possibilities

### Phase Flexibility

The Arbiter may reorder, repeat, or skip phases based on what the
conversation needs. The listed phases are the default flow, not a
rigid sequence. If a reality check surfaces a pivot that deserves
exploration, the Arbiter can loop back.

## Research Capabilities

The Arbiter owns all information gathering. The Dreamer and Realist
evaluate — they do not research.

When a factual question arises during the session:

1. Pause the exchange. Announce what you are looking up and why.
2. Choose the appropriate method:
   - **Codebase questions** — delegate to the Explore subagent.
     Select thoroughness level (quick, medium, very thorough) based
     on the complexity of the question.
   - **External information** — use web search. Prioritize primary
     sources (official docs, specifications) over secondary
     commentary.
   - **Project documents** — read specific sections of referenced
     files. Do not ingest entire documents. Use grep to locate
     relevant sections first, then read only those sections (max
     80 lines per read).
3. Report findings factually — do not speculate or editorialize.
4. State "not able to determine from available information" when
   research is inconclusive.
5. Resume the exchange with the findings available to both personas.

## Session Close

1. Arbiter announces the session is closing
2. Arbiter reviews key findings with Dreamer and Realist for any
   final input (1 round maximum)
3. Arbiter writes the output document using the Output Format below
4. Save to `docs/brainstorms/[topic-slug]-[YYYY-MM-DD].md` (create
   the directory if it doesn't exist)
5. Present a brief summary to the user with the file path

## Constraints

- Maximum 5 exchange rounds per inquiry phase before the Arbiter must
  synthesize and move on
- Dreamer must ground every proposal with at least one concrete benefit
- Realist must offer a constructive alternative or conditions for
  viability when challenging an idea — never just "no"
- Arbiter never declares an idea good or bad — it describes the shape
  and fit
- Emergent threads are captured in the output document, not explored in
  depth during the session unless the Arbiter determines they are
  directly relevant to the current topic
- Research pauses the exchange: when a factual question arises, the
  Arbiter investigates before the conversation continues

## Output Format

Build the output document progressively — write sections as they are
completed, not all at the end.

```markdown
# Brainstorm: [Topic]

**Date:** [YYYY-MM-DD]
**Input:** [original $ARGUMENTS verbatim]
**Mode:** [Problem Exploration | Solution Deep-Dive]
**Participants:** Dreamer, Realist, Arbiter

---

## Idea Brief

[Crystallized picture — what the idea or solutions look like after
examination through both lenses. This is the Arbiter's final synthesis:
the shape of the idea when both Dreamer and Realist perspectives are
accounted for. Written in clear prose, not as a list of points. Should
answer: what is this, what does it look like, and where does it connect
to a real-world need?]

## Strengths

[Where the Dreamer and Realist agreed, or where the Dreamer's case held
up under the Realist's scrutiny. These are the aspects of the idea with
the strongest foundation. Each strength should be a brief statement with
supporting rationale.]

## Open Questions

[Where tension remained unresolved between perspectives. These are
genuine uncertainties, not failures — the brainstorm surfaced them,
which is valuable. Each question should state what's unresolved and why
it matters.]

## Recommended Next Steps

[Concrete, actionable steps. Examples:]
[- "Take this to `/arbitrate propose` for full-team evaluation"]
[- "Prototype the core mechanic before investing further"]
[- "Research competitor X to determine if this is a differentiated approach"]
[- "Run a second `/brainstorm` pass focused on [specific solution direction]"]

## Needed Research

[Specific unknowns the Arbiter flagged that couldn't be resolved during
the session. Each item should state what needs to be found out and why
it's important to the idea's viability. Distinguished from Open Questions
in that these have a concrete research action — someone can go find the
answer.]

## Emergent Threads

[New, alternative, or related possibilities that surfaced during the
brainstorm. These are not tangents parked for time discipline — they are
discoveries worth their own exploration. Each thread should be described
with enough context that a future `/brainstorm` or `/arbitrate propose`
session could pick it up without needing to re-derive it.]
```
