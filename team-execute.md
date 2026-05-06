## /team-execute

Execute the implementation plan for the current ticket. This is where code gets written, tests get created, and commits are made — all within the current feature branch.

### Prerequisites

- A plan exists from `/team-plan`
- You are on the correct feature branch (not main, not develop)
- The plan has passed its gate check (reviewed if required by config)

### Instructions

Read the project context:
- `CLAUDE.md` for coding standards and architecture
- `.team/config.yaml` for methodology and policies
- `.team/CONVENTIONS.md` for commit format and scope rules
- The implementation plan from `/team-plan`

**Execute each step in the plan sequentially.**

For each step:

1. **If methodology is `tdd`:**
   - Write the failing test FIRST
   - Run the test suite — confirm it fails for the right reason
   - Write the minimum implementation to make it pass
   - Run the test suite — confirm it passes
   - Refactor if needed (tests must still pass)
   - Commit

2. **If methodology is `bdd`:**
   - Write the scenario test FIRST
   - Run it — confirm it fails
   - Implement the behaviour
   - Run it — confirm it passes
   - Commit

3. **If methodology is `standard`:**
   - Implement the change
   - Write tests for the change
   - Run the test suite
   - Commit

### Hard guardrails — these are non-negotiable

**Branch discipline**
- NEVER commit to the main branch or develop branch
- NEVER merge branches
- NEVER create new branches without asking the developer
- NEVER force push
- ALL commits go to the current feature branch only

**Scope discipline**
- ONLY modify files identified in the plan
- If you discover something that needs fixing outside scope: log it in a `FINDINGS.md` file locally, do NOT fix it
- If a step requires changes outside scope to work: STOP and ask the developer
- Do NOT refactor unrelated code
- Do NOT "improve" things that aren't in the ticket
- Do NOT add features that aren't in the acceptance criteria
- "While I'm here" changes are NOT permitted

**Dependency discipline**
- Check `.team/config.yaml` `dependency_policy`
- If `flag`: note any new dependency in a local findings file for the PR description
- If `lock`: do NOT add new dependencies without explicit developer approval during this session
- If `open`: proceed, but still note new dependencies for the PR

**Protected paths**
- Check `.team/config.yaml` `protected_paths`
- If a step requires modifying a protected path: STOP and ask the developer for explicit approval
- Log the approval and the reason

**Commit hygiene**
- Each commit corresponds to one step in the plan
- Commit messages follow the convention in `.team/CONVENTIONS.md`
- If `ai_commit_prefix: true` in config: include `[ai]` in the commit body
- Commits are atomic — each one should leave the codebase in a working state
- Run the lint command and type check command from config before each commit

### During execution

After completing each step:
- Report what was done
- Report test results
- Report any findings (things outside scope that need attention)
- If the next step depends on a decision: ask before proceeding

If a test fails and the fix is straightforward: fix it and re-run.
If a test fails and the fix requires changes outside scope: STOP and discuss with the developer.

### On completion

When all steps are executed:

1. Run the full test suite
2. Run the lint command
3. Run the type check command
4. Report:
   - Steps completed: X/Y
   - Tests: passing/failing
   - Lint: clean/issues
   - Types: clean/issues
   - New dependencies: list (if any)
   - Protected path changes: list (if any)
   - Findings (out-of-scope issues): list (if any)
   - Coverage for new code: %

5. If all checks pass: advise the developer to run `/team-review` before raising a PR
6. If any checks fail: report what failed and suggest fixes within scope

### Findings file

If any out-of-scope issues were discovered during execution, save them locally:

```
# Findings: <ticket-id>
Date: <date>

## Out-of-scope issues discovered during implementation

1. <description> — Suggested ticket: <type>/<short-description>
   Location: <file:line>
   Severity: low/medium/high

2. <description> — Suggested ticket: ...
```

These findings should be reviewed by the developer and potentially added to the backlog during the next grooming session.
