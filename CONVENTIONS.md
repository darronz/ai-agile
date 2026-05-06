# Team Conventions

This document defines how we work together. Both humans and AI agents read this.

## Definition of Done

A ticket is done when:

- [ ] All acceptance criteria from grooming are met
- [ ] Tests exist and pass (unit, integration as appropriate)
- [ ] Coverage threshold met for new/changed code
- [ ] Linter and type checker pass
- [ ] Self-review (`/team-review`) completed with no critical findings
- [ ] PR raised with completed template
- [ ] At least one human reviewer has approved
- [ ] CI pipeline passes
- [ ] Documentation updated if required by `.team/config.yaml`
- [ ] No unresolved scope creep flags

## Commit message format

When using conventional commits:

```
<type>(<scope>): <short description>

<body - what and why, not how>

[ai] # if agent-authored, added in body not subject
```

Types: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `perf`, `ci`

Scope is optional but encouraged — use the module or area affected.

## Branch naming

```
<type>/<ticket-id>-<short-description>

# Examples:
feat/PROJ-123-add-webhook-support
fix/PROJ-456-race-condition-queue
chore/PROJ-789-upgrade-vitest
```

## PR expectations

Every PR must:
- Reference the ticket it implements
- Stay within the scope of that ticket
- Include a description of what changed and why
- Note whether the implementation was AI-assisted
- Flag any new dependencies with justification
- Flag any changes to protected paths
- Include testing instructions for the reviewer

## Code review checklist (for human reviewers)

- Does the change match the ticket's acceptance criteria?
- Are there tests for the meaningful behaviour changes?
- Does the approach fit the existing architecture?
- Are there any scope additions that weren't in the ticket?
- If AI-assisted: do the abstractions make sense, or has the agent over-engineered?
- Are error cases handled?
- Are there any security implications?

## Scope discipline

The most common way AI-assisted development goes wrong in a team is scope creep. The agent "improves" things outside the ticket, refactors unrelated code, or adds features nobody asked for. This creates review burden, merge conflicts, and confusion.

Rules:
- Agent stays within the files and modules identified in the plan phase.
- If the agent identifies something that needs fixing outside scope, it logs it as a separate finding — it does not fix it.
- Refactoring that isn't in the ticket gets its own ticket, its own branch, its own PR.
- "While I'm here" changes are not permitted.
