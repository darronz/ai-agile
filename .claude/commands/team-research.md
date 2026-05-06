## /team-research <ticket>

Research a ticket thoroughly before any planning or implementation begins. This is the "do we actually understand this problem?" phase.

### Instructions

### Preset resolution

Read `.team/config.yaml`. If a `preset` field is set, load `.team/presets/<preset>.yaml` and merge its values as defaults — any value explicitly set in `config.yaml` overrides the preset. Use the resolved config for all tooling references (test commands, lint commands, etc.) throughout this command.

### Work directory

Create `.team/work/<ticket-id>/` if it does not exist.

Read the project context:
- `CLAUDE.md` for architecture and coding standards
- `.team/config.yaml` for methodology
- `.team/SPRINT.md` for what else is in flight (potential conflicts)

Then research the ticket from multiple angles:

**Codebase analysis**
- What existing code relates to this ticket?
- What patterns, conventions, or abstractions already exist that this work should follow?
- What test coverage exists in the affected areas?
- What files and modules will likely need to change?

**Domain research**
- If the ticket involves external systems, APIs, or protocols — research them.
- If there are industry-standard approaches to this problem — identify them.
- If similar features exist in the codebase — understand how they were built and why.

**Risk identification**
- What could go wrong?
- What edge cases does the ticket description not mention?
- Are there security implications?
- Are there performance implications?
- Does this interact with work other team members are doing this sprint?

**Open questions**
- What is ambiguous in the ticket description?
- What decisions need to be made before implementation?
- What information is missing?

### Output

Save the research summary to `.team/work/<ticket-id>/research.md`.

The file must begin with this frontmatter:

```
---
ticket: <ticket-id>
phase: research
date: <YYYY-MM-DD>
author: <developer>
preset: <preset value from config, or "none">
---
```

```
# Research: <ticket-id> — <ticket-title>
Date: <date>
Researcher: <developer>

## Summary
<2-3 sentences: what this ticket involves and the key finding from research>

## Codebase context
<What exists, what patterns to follow, what files are involved>

## External context
<Relevant external docs, APIs, standards, prior art>

## Risks
<Numbered list with severity: low/medium/high>

## Open questions
<Things that need answers before planning — bring these to refinement>

## Recommendation
<Is this ticket well-scoped? Should it be split? Is the estimate reasonable?
Does it need more refinement before it's ready for a sprint?>
```

### Important

This phase produces information, not code. Do not write any implementation code. Do not create branches. Do not modify any project source files. Research is read-only except for writing the research summary to `.team/work/<ticket-id>/research.md`.

The output is designed to be shared with the team — in refinement, standup, or async in the team's communication channel. It's the basis for better grooming.
