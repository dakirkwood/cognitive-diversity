---
name: perspectives
description: >
  Generate, list, and validate custom perspective agents for use with
  /discuss and /canvas. Perspectives are domain-specific viewpoints
  that participate in structured multi-perspective discussions.
argument-hint: "[list|generate <domain>|validate [path]] [--project]"
---

# Perspectives

Manage custom perspective agents for `/discuss` and `/canvas`.

## Subcommands

Parse `$ARGUMENTS` to determine the subcommand:

### `list`

Scan all three perspective locations and report what's available:

1. **Project-local**: `.claude/perspectives/` in the current working directory
2. **User-global**: `~/.claude/perspectives/`
3. **Plugin built-in**: `agents/perspectives/` in this plugin

For each perspective found, report:
- **Name** (from frontmatter `name` field)
- **Description** (from frontmatter `description` field)
- **Source** (built-in / user / project)
- **Override** (if a higher-precedence location shadows a lower one)

Deduplicate by name — show only the highest-precedence version, but
note when an override is in effect.

Output as a formatted table.

### `generate <domain>`

Generate 3–5 domain-specific perspectives for the given domain.

**Process:**
1. Read the built-in perspectives in `agents/perspectives/` to
   understand the contract (frontmatter format, section structure).
2. Analyze the domain to identify 3–5 distinct viewpoints that would
   create productive tension when discussing topics in that domain.
3. For each perspective, generate a file following the exact contract:

```markdown
---
name: [kebab-case-name]
description: >
  [One-line description of what this perspective evaluates and optimizes for.]
model: inherit
---

# [Display Name] — [Domain Role]

## Perspective

[What this perspective evaluates through. What lens they apply.
What they care about that others in the room might not.]

## Reasoning Style

[How they think and argue. What kind of evidence they find
compelling. How they approach problems.]

## Priorities

- [Priority 1]
- [Priority 2]
- [Priority 3]
- [Priority 4]
- [Priority 5]

## Voice

[How they sound in discussion. What phrases they use. How they
engage with disagreement.]

## Prefix

**[Display Name/Domain]:**
```

4. **Output location:**
   - Default: write to `~/.claude/perspectives/`
   - With `--project` flag: write to `.claude/perspectives/` in the
     current working directory

5. After writing, run the equivalent of `validate` on each generated
   file and report any issues.

6. Suggest example `/discuss` invocations using the new perspectives
   with `--roster`.

**Domain examples and suggested perspectives:**

- **healthcare**: clinician, patient-advocate, regulator, health-economist, clinical-informaticist
- **education**: instructor, student-advocate, curriculum-designer, accessibility-specialist, administrator
- **fintech**: risk-analyst, compliance-officer, product-owner, security-engineer, customer-advocate
- **gaming**: game-designer, player-advocate, monetization-strategist, community-manager, technical-artist

These are suggestions — the actual perspectives should be tailored to
the specific domain description provided.

### `validate [path]`

Validate a perspective file (or directory of files) against the contract.

**If path is a file:** validate that single file.
**If path is a directory:** validate all `.md` files in the directory.
**If no path:** validate all files in `~/.claude/perspectives/` and
`.claude/perspectives/`.

**Validation checks:**

1. **Frontmatter present** with `name`, `description`, and `model` fields
2. **Name is kebab-case** and unique across all scan locations
3. **Required sections present**: Perspective, Reasoning Style, Priorities, Voice
4. **Priorities section** contains at least 3 items
5. **Prefix section** present with labeled format
6. **No path references** — perspective files should be self-contained

Report each issue with file path and specific problem. Exit with a
summary: N files checked, M valid, K issues found.

## No Subcommand

If `$ARGUMENTS` is empty or doesn't match a subcommand, display:

```
Usage:
  /perspectives list                    — show all available perspectives
  /perspectives generate <domain>       — create domain-specific perspectives
  /perspectives validate [path]         — check perspective files for issues

Perspectives are loaded by /discuss and /canvas from three locations:
  1. .claude/perspectives/    (project-local, highest precedence)
  2. ~/.claude/perspectives/  (user-global)
  3. Built-in plugin defaults (5 perspectives)
```
