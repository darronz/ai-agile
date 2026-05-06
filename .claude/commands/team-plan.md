## /team-plan <ticket>

Create an implementation plan for an assigned ticket. This happens in the developer's feature branch and produces a local plan file that guides execution.

### Prerequisites

- The ticket has been groomed (acceptance criteria exist)
- The developer has created or is on their feature branch
- Branch name follows the convention in `.team/CONVENTIONS.md`

### Preset resolution

Read `.team/config.yaml`. If a `preset` field is set, load `.team/presets/<preset>.yaml` and merge its values as defaults — any value explicitly set in `config.yaml` overrides the preset. Use the resolved config for all tooling references throughout this command.

### Prerequisite check

Check for `.team/work/<ticket-id>/grooming.md`:
- If it exists: read it for acceptance criteria, complexity assessment, and size recommendation.
- If it does NOT exist: **STOP.** Tell the developer: "No groomed ticket found at `.team/work/<ticket-id>/grooming.md`. Run `/team-groom <ticket>` first — planning without acceptance criteria produces unreliable plans." Do not proceed.

Also check for `.team/work/<ticket-id>/research.md`:
- If it exists: read it for codebase context, risks, and open questions.
- If it does not exist: note that research was not done. Proceed — research is informative, not required for planning.

### Work directory

Create `.team/work/<ticket-id>/` if it does not exist.

### Instructions

Read the project context:
- `CLAUDE.md` for architecture and standards
- `.team/config.yaml` for methodology and branching strategy
- `.team/CONVENTIONS.md` for commit conventions and scope rules
- `.team/SPRINT.md` for potential conflicts with other in-flight work
- The groomed ticket (acceptance criteria, complexity assessment)
- Any research output from `/team-research`

**Verify branch setup**
- Confirm you are on a feature branch, not main/develop
- Confirm the branch name matches conventions
- If not: stop and ask the developer to fix this before proceeding

**Create the implementation plan**

Break the ticket into ordered implementation steps. Each step should be:
- Small enough to be a single commit
- Testable in isolation where possible
- Clear about which files it modifies

The plan structure depends on methodology:

If `methodology: tdd`:
Each step follows the red-green-refactor cycle:
1. Write failing test
2. Write minimum implementation to pass
3. Refactor if needed
4. Commit

If `methodology: bdd`:
Each step implements one or more Given/When/Then scenarios:
1. Write scenario test (failing)
2. Implement behaviour
3. Verify scenario passes
4. Commit

If `methodology: standard`:
Each step produces working, tested code:
1. Implement change
2. Add/update tests
3. Verify
4. Commit

**Scope check**
For each step in the plan, verify:
- Does this step address something in the acceptance criteria?
- Does this step modify only files within scope?
- If a step seems to require changes outside scope: flag it and suggest a separate ticket

**Conflict check**
Cross-reference the plan's file list against `.team/SPRINT.md`:
- If another developer is working on overlapping files: flag the conflict
- Suggest merge ordering or coordination approach

### Output

Save the plan to `.team/work/<ticket-id>/plan.md`.

The file must begin with this frontmatter:

```
---
ticket: <ticket-id>
phase: plan
date: <YYYY-MM-DD>
author: <developer>
preset: <preset value from config, or "none">
---
```

Plan format:

```
# Implementation Plan: <ticket-id>
Branch: <branch-name>
Date: <date>
Developer: <name>

## Approach
<2-3 sentences: overall approach and key decisions>

## Steps

### Step 1: <description>
Files: <list of files to create/modify>
Test: <what test(s) to write>
Commit message: <conventional commit message>

### Step 2: <description>
Files: <list>
Test: <what test(s)>
Commit message: <message>
Depends on: Step 1

...

## Scope boundaries
- IN: <what this plan covers>
- OUT: <what this plan explicitly does not touch>
- FLAGGED: <things found that need attention but belong in separate tickets>

## Risk notes
<Anything the developer should watch for during execution>

## Conflict alert
<Any overlap with other sprint work — files, modules, APIs>
```

### Gate check

If `.team/config.yaml` has `gates.plan_to_execute: review`:
- Tell the developer this plan should be shared with the tech lead or another developer before proceeding to `/team-execute`
- Do not proceed to execution automatically

If `gates.plan_to_execute: auto`:
- The developer reviews the plan and proceeds when satisfied
