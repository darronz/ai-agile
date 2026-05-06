## /team-groom <ticket>

Refine a ticket into a well-scoped, implementable unit of work with clear acceptance criteria. This is what agile refinement is supposed to produce but often doesn't.

### Instructions

Read the project context:
- `CLAUDE.md` for architecture and standards
- `.team/config.yaml` for methodology (this affects how acceptance criteria are written)
- `.team/CONVENTIONS.md` for definition of done
- Any research output from `/team-research` if it exists for this ticket

If `/team-research` has NOT been run for this ticket, do a lightweight version of the research phase first — read the relevant codebase areas and identify the scope. Flag that full research was not done.

**Refine the ticket description**
- Clarify what "done" looks like in concrete terms
- Remove ambiguity — if something could be interpreted two ways, call it out
- Identify what is explicitly out of scope

**Write acceptance criteria**

The format depends on the methodology in `.team/config.yaml`:

If `methodology: bdd`:
```gherkin
Given <precondition>
When <action>
Then <expected outcome>
```
Write scenarios for the happy path, key error cases, and edge cases.

If `methodology: tdd`:
Write test descriptions (what each test should verify) grouped by unit and integration level:
```
Unit tests:
- <function/module>: should <behaviour> when <condition>
- <function/module>: should <behaviour> when <condition>

Integration tests:
- <flow>: should <end-to-end behaviour>
```

If `methodology: standard`:
Write plain acceptance criteria as a checklist:
```
- [ ] <observable behaviour or outcome>
- [ ] <observable behaviour or outcome>
```

**Complexity assessment**
Analyse the codebase and produce:
- Files likely to change (list them)
- Services or modules affected
- Whether a database migration is needed
- Whether external API integration is involved
- Current test coverage in affected areas (%, and quality assessment)
- Dependencies on other tickets or work in flight
- Risk level: low / medium / high (with justification)

**Size recommendation**
Based on the complexity assessment:
- Is this a single PR or should it be split?
- If splitting: propose the slices and their ordering
- If trunk-based branching: can each slice be merged independently with a feature flag?

### Output

Produce the refined ticket as a markdown file. If the team uses a tracker (Jira, Linear, GitHub Issues), format it so it can be pasted in.

```
# <ticket-id>: <ticket-title>

## Description
<Clear, unambiguous description of what needs to be built>

## Acceptance criteria
<Format per methodology — see above>

## Out of scope
<Explicitly: what this ticket does NOT cover>

## Complexity assessment
- Files affected: <list>
- Modules/services: <list>
- Migration required: yes/no
- External APIs: yes/no
- Current test coverage: <% and quality note>
- Risk: low/medium/high — <reason>

## Size
- Estimated PRs: <number>
- Suggested slices (if splitting):
  1. <slice description>
  2. <slice description>

## Dependencies
- Blocked by: <tickets or external factors>
- Blocks: <tickets>
- Overlaps with: <tickets — potential merge conflict areas>

## Open questions (if any remain)
<Questions that should be resolved before sprint commitment>
```

### Important

Do not write implementation code. Do not create branches. The output of grooming is a refined ticket, not a plan or implementation.

This output is designed to be reviewed by the team in a refinement session. The developer brings it, the team discusses, the PO approves or adjusts.
