# Focus Group Reference

Persona design rationale, secondary attribute pools, and mode-specific behavioral translation guidance for the `/focus-group` command.

---

## Secondary Attribute Pools

The facilitator draws one value per attribute for each persona at spin-up time. No weighting — pure random selection.

| Attribute | Values |
|-----------|--------|
| **Tech comfort** | Low, Moderate, High |
| **Domain familiarity** | First visit, Some exposure, Experienced with similar |
| **Urgency** | Browsing casually, Moderate task pressure, Urgent deadline |
| **Device context** | Desktop large screen, Laptop, Tablet, Mobile |
| **Mood/patience** | Patient and willing to explore, Neutral, Frustrated before starting |

### Device Context → Playwright Viewport

| Device | Viewport (W×H) |
|--------|---------------|
| Desktop large screen | 1920×1080 |
| Laptop | 1440×900 |
| Tablet | 768×1024 (portrait) |
| Mobile | 390×844 |

---

## Behavioral Translation Guide

Cognitive profile tendencies are mode-agnostic. The facilitator translates each tendency into mode-specific behavioral instructions before dispatching personas.

### Translation Patterns by Tendency

| Cognitive Tendency | Browser Mode | Document Mode | Scenario Mode |
|-------------------|-------------|---------------|---------------|
| "Reads everything carefully" | Opens all links in the primary path, reads all page content before clicking | Reads document top to bottom before acting | Considers all information before responding |
| "Skips long text blocks" | Scrolls past paragraphs, clicks headings or buttons | Jumps to headers, bullets, and bold text | Focuses on summary statements, ignores detail paragraphs |
| "Hesitates before unfamiliar actions" | Hovers over buttons/links before clicking, re-reads labels, sometimes backs out | Re-reads instructions multiple times, looks for examples | Asks clarifying questions before committing to a path |
| "Tries unusual navigation paths" | Explores sidebar, footer, and secondary navigation; tries keyboard shortcuts | Looks for appendices, footnotes, or non-obvious callouts | Questions assumptions in the scenario, considers edge cases |
| "Needs visual hierarchy" | Relies on headings, icons, and visual groupings to navigate; ignores body text | Relies on headers and visual structure; ignores dense paragraphs | Needs the problem framed clearly before reasoning through it |
| "Interprets labels literally" | Clicks only links that exactly match the task description; confused by jargon or euphemisms | Follows instructions exactly as written; stumbles on implied steps | Takes scenario constraints at face value; doesn't infer unstated assumptions |
| "Relies on visual cues" | Uses images, icons, and button shape to navigate; avoids text-heavy areas | Uses callout boxes, images, and highlighted text; skips body paragraphs | Responds better to structured information (lists, comparisons) than prose |

---

## Persona Design Rationale

### Why Big Five Composites?

The Big Five (Openness, Conscientiousness, Extraversion, Agreeableness, Neuroticism) is the most empirically validated personality framework in psychology. It predicts behavior across contexts — including technology use and information-seeking.

The four composite personas represent distinct **interaction styles**, not extreme trait combinations:
- **High Conscientiousness** → rule-following, methodical, patient
- **Low Conscientiousness** → shortcut-seeking, impulsive, impatient
- **High Neuroticism** → anxious, risk-averse, reassurance-seeking
- **High Openness** → exploratory, tolerant of ambiguity, curious

We deliberately omit personas for High Extraversion/Agreeableness as these traits have weaker independent effects on solo task navigation behavior.

### Why Neurodivergent Advocates (Not Trait Extremes)?

ADHD, autism spectrum, and dyslexia represent **qualitatively different cognitive processing patterns** — not extremes of Big Five dimensions. They require dedicated representation rather than approximation via trait combinations because:

1. Their friction triggers are categorically different (e.g., "ambiguous labels" is a moderate frustration for trait-based personas but a significant barrier for the Autism profile)
2. Their findings map directly to **accessibility guidelines** (WCAG, ARIA) and regulatory obligations
3. Grouping them with trait-based personas would obscure their distinct contributions to the report

**Important caveat:** LLM-simulated neurodivergent experiences are approximations based on research literature, not lived experience. These agents are useful for catching obvious friction (walls of text, inconsistent navigation, ambiguous labels) but should never substitute for usability testing with real people from these communities.

### Persona Roster Scope

The default roster of 7 personas (4 trait-based, 3 neurodivergent) is designed to:
- Cover the most distinct interaction styles likely to surface different issues
- Complete in reasonable time for parallel Agent Team execution
- Remain legible in the synthesis report without becoming overwhelming

For focused runs, use `--personas` to select a subset. For example:
- `--personas adhd-profile,autism-profile,dyslexia-profile` for an accessibility-focused run
- `--personas impatient-scanner,anxious-navigator` for a quick two-persona pass

---

## Persona Voice Guidelines

Each persona's "Verbatim Reaction" in the report should sound distinct. Reference these voice signatures when generating in-character quotes:

| Persona | Voice Signature |
|---------|----------------|
| Methodical User | Measured, analytical, references specific elements ("The second step in the instructions said...") |
| Impatient Scanner | Clipped, mildly frustrated, focuses on time ("Why is this taking so long?") |
| Anxious Navigator | Uncertain, hedged, seeks confirmation ("I think I did this right, but I'm not sure if...") |
| Curious Explorer | Observational, surprised, notes unexpected things ("Interesting — there's a whole section here that most people probably miss") |
| ADHD Profile | Fragmented, honest about distraction ("I got sidetracked by the banner — what was I doing again?") |
| Autism Spectrum Profile | Precise, direct, notes inconsistencies ("The button says 'Submit' on page 1 but 'Send' on page 2 — which is correct?") |
| Dyslexia Profile | Effort-oriented, visual-first ("The picture helped a lot — I wasn't sure what to do until I saw that") |

---

## Step Count Guidelines

| Mode | Shallow | Deep |
|------|---------|------|
| Browser | Max 10 steps per persona | Max 20 steps per persona |
| Document | Max 3 passes through content | Full document, all sections |
| Scenario | Initial response only | Full exploration of edge cases |

A "step" in browser mode is any distinct navigation action: page load, click, scroll-to-action, form input, or back navigation.

If a persona hasn't made progress in 5 consecutive steps, flag as "stuck" and record the last known state before ending their session.

---

## Output Directory

Reports are written to `docs/focus-groups/` relative to the project root.

**Filename format:** `[target-slug]-[YYYY-MM-DD].md`

**Target slug rules:**
- Lowercase
- Hyphens for spaces and special characters
- Strip protocol (`https://`) from URLs
- Strip leading `www.` from URLs
- Max 40 characters (truncate if needed)
- Examples:
  - `https://example.edu/enrollment` → `example-edu-enrollment`
  - `onboarding-email.md` → `onboarding-email`
  - "Choosing between PM tools" → `choosing-between-pm-tools`
