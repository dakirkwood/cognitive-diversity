---
name: focus-group
description: >
  Synthesis patterns, report templates, severity framework, and mode
  specifications for the focus-group command. Reference material for
  the facilitator agent during report generation.
---

# Focus Group Skill

Synthesis patterns, report templates, severity framework, and mode specifications for the `/focus-group` command.

## Severity Framework

Rate each finding on a four-level scale:

| Level | Definition | Examples |
|-------|-----------|---------|
| **Task-Blocking** | Prevents task completion entirely | No visible path to goal, broken navigation, missing required information |
| **High Friction** | Significantly slows or frustrates task completion | Ambiguous labels, confusing information architecture, requires multiple wrong turns |
| **Moderate** | Causes hesitation or minor detours | Unclear button states, inconsistent terminology, dense text blocks |
| **Minor** | Small annoyance with easy recovery | Suboptimal label wording, slightly small tap targets, non-critical missing affordances |

**Consensus threshold**: An issue encountered by 3 or more personas is a consensus finding, regardless of severity level. Consensus issues always appear in the Consensus Findings section, not just in individual journeys.

---

## Mode Specifications

### Browser Mode

**What persona agents do:**
- Navigate to the target URL using Playwright
- Attempt to complete the task following their cognitive profile and behavioral instructions
- Record every navigation decision with rationale
- Note what they see, what they click, where they get confused
- Stop when task is complete, clearly failed, or after 20 steps (whichever comes first)

**Step counting:** Each distinct navigation action counts as one step: page load, click, scroll-to-action, form input, back navigation.

**Screenshot guidance:** Capture screenshots at friction points and at task completion/failure. Reference screenshots in the persona journey (e.g., "Screenshot: checkout-step-3-confusion.png").

**Viewport:** Set per assigned device context (see REFERENCE.md).

### Document Mode

**What persona agents do:**
- Read the provided document or file content
- Attempt to follow the task instructions or evaluate the content as directed
- Apply their cognitive profile to how they read and interpret the content
- Record where they get confused, skip content, or misinterpret instructions
- Rate their confidence that they completed the task correctly

**No navigation required.** Personas evaluate content in their context window.

### Scenario Mode

**What persona agents do:**
- Read the scenario description
- Reason through how they would approach the situation given their cognitive profile
- Identify what information they'd need, what they'd find confusing, what would help them decide
- Produce a reflective response describing their experience

**Most interpretive mode.** Facilitator should provide a rich scenario description (250+ words) for best results.

---

## Synthesis Patterns

### Consensus Issue Detection

```
For each unique friction point mentioned across all persona reports:
  count = number of personas who mention this friction point
  if count >= 3:
    classify as consensus issue
    assign severity (highest level reported by any persona)
    add to Consensus Findings section
  else:
    assign to Profile-Specific Findings (neurodivergent or trait-specific)
```

When grouping, use semantic similarity — personas may describe the same issue with different words. Consolidate related descriptions into a single finding.

### Path Analysis

Map distinct routes through the task:
1. Identify the **expected path** (what a methodical user following conventions would do)
2. Identify **deviation points** where personas diverged from the expected path
3. Note **convergence points** where multiple paths joined back together
4. Flag **dead ends** where personas got stuck with no clear recovery path

### Recommendation Generation

For each finding in Consensus Findings and high-severity Profile-Specific Findings, generate one recommendation:

```
Finding: [description]
Recommendation:
  - What to change: [specific, actionable change]
  - Addresses: Finding #[N]
  - Benefits: [persona names]
  - Impact: High / Medium / Low
  - Rationale: [why this change would reduce the friction]
```

**Impact assessment:**
- **High**: Removes a Task-Blocking or High Friction issue affecting 3+ personas or all neurodivergent profiles
- **Medium**: Reduces Moderate friction for 2+ personas, or resolves High Friction for 1 profile
- **Low**: Addresses Minor friction or improves experience for 1 profile

---

## Report Template

```markdown
# Focus Group Report: [Task Description]

> **Note:** Findings are potential issues identified through simulated cognitive profiles.
> This is a screening tool, not a replacement for usability testing with real people.

---

## Configuration

| Field | Value |
|-------|-------|
| Mode | [browser / document / scenario] |
| Target | [URL, filename, or "Scenario"] |
| Task | [exact task description] |
| Date | [YYYY-MM-DD] |
| Personas | [count] — [comma-separated names] |
| Depth | [shallow / deep] |
| Output | [summary / full] |

---

## Executive Summary

[2–3 paragraphs: overall assessment of how the target performed across persona profiles, the single most important finding, and the top recommendation. Written in plain language for a non-technical audience.]

---

## Task Completion Matrix

| Persona | Completed | Steps | Primary Blocker |
|---------|-----------|-------|-----------------|
| Methodical User | ✅ Yes | 6 | — |
| Impatient Scanner | ⚠️ Partial | 8 | [blocker description] |
| Anxious Navigator | ❌ No | 12 | [blocker description] |
| Curious Explorer | ✅ Yes | 14 | — |
| ADHD Profile | ❌ No | 9 | [blocker description] |
| Autism Spectrum Profile | ⚠️ Partial | 11 | [blocker description] |
| Dyslexia Profile | ✅ Yes | 7 | — |

---

## Consensus Findings

*Issues encountered by 3 or more personas, sorted by severity.*

### 🔴 Task-Blocking

[If any. Each finding as a numbered item with description and list of affected personas.]

### 🟠 High Friction

[If any.]

### 🟡 Moderate

[If any.]

### 🟢 Minor

[If any.]

---

## Profile-Specific Findings

### Neurodivergent-Specific

*Separated for visibility — these findings have accessibility and potentially regulatory implications.*

**ADHD Profile**
- [findings]

**Autism Spectrum Profile**
- [findings]

**Dyslexia Profile**
- [findings]

### Trait-Specific

**Methodical User**
- [findings, if any beyond consensus]

**Impatient Scanner**
- [findings]

**Anxious Navigator**
- [findings]

**Curious Explorer**
- [findings]

---

## Persona Journeys

### Methodical User — Systematic Executor

**Secondary Attributes (this run):**
- Tech comfort: [assigned value]
- Domain familiarity: [assigned value]
- Urgency: [assigned value]
- Device: [assigned value]
- Mood: [assigned value]

**Outcome:** [Completed / Failed at step N / Partial — reached X but couldn't complete Y]

**Path Taken:**
1. [Step 1 description]
2. [Step 2 description]
3. [...]

**Friction Points:**
- [What slowed or stopped them]

**Positive Notes:**
- [What worked well for this persona]

**Verbatim Reaction:**
> "[Brief in-character quote capturing their experience]"

---

[Repeat for each active persona]

---

## Recommendations

*Each recommendation tied to specific findings. Sorted by estimated impact.*

### High Impact

1. **[Recommendation title]**
   - What to change: [specific, actionable change]
   - Addresses: [Finding reference]
   - Benefits: [persona names]
   - Impact: High
   - Rationale: [why this reduces friction]

### Medium Impact

[...]

### Low Impact

[...]

---

## Rerun Recommendations

*Specific combinations worth retesting, with rationale.*

1. **[Persona] with [attribute combination]**
   - Rationale: [Why this combination might surface different issues]
   - Focus: [What to pay attention to in the rerun]

---

*Report generated: [YYYY-MM-DD HH:MM]*
*Plugin: focus-group v1.0.0*
```

---

## Compare Mode Additions

When `--compare` is used, add these sections after the Task Completion Matrix:

```markdown
## Comparative Task Completion

| Persona | Target A Result | Target B Result | Preferred |
|---------|----------------|----------------|-----------|
| ...     | ...            | ...            | ...       |

## Comparative Analysis

[Side-by-side analysis of how the two targets differed across personas. Focus on cases where Target A succeeded and Target B failed, and vice versa.]
```

Output filename for compare mode: `[target-a-slug]-vs-[target-b-slug]-[YYYY-MM-DD].md`
