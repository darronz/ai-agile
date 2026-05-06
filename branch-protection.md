# Repository Guardrails — Platform Configuration Reference

This document describes the recommended platform-level protections for AI-assisted team development. These settings enforce the same rules the skill commands follow, providing defence in depth — the AI is told the rules, and the platform prevents violations even if the AI ignores them.

Run `/team-setup` to audit your repo against these recommendations.

## GitHub

### Branch protection rules (Settings → Branches → Branch protection rules)

For your main branch (and `develop` if using Gitflow):

| Setting | Recommended | Why |
|---------|-------------|-----|
| Require a pull request before merging | ✅ Enabled | Agents must not push directly to main |
| Required approving reviews | ≥ 1 (match `review_required` in config.yaml) | Human approval is non-negotiable |
| Dismiss stale PR approvals when new commits are pushed | ✅ Enabled | Agent fixups after approval require re-review |
| Require review from code owners | ✅ Enabled (if CODEOWNERS exists) | Critical paths get the right reviewer |
| Require status checks to pass | ✅ Enabled | CI must pass before merge |
| Required status checks | lint, typecheck, test (match your CI job names) | Matches config.yaml quality gates |
| Require branches to be up to date | ✅ Enabled | Prevents stale branch merges |
| Require signed commits | Optional | Audit trail for AI vs human authorship |
| Include administrators | ✅ Enabled | Rules apply to everyone, including repo owners |
| Restrict force pushes | ✅ Enabled | Agents must never force push |
| Restrict deletions | ✅ Enabled | Protect branch history |

### CODEOWNERS (.github/CODEOWNERS)

Map protected paths from `.team/config.yaml` to responsible team members:

```
# Infrastructure and deployment
/.github/         @tech-lead
/infrastructure/  @tech-lead @devops-lead
Dockerfile*       @tech-lead
docker-compose*   @tech-lead

# Project configuration
CLAUDE.md         @tech-lead
.team/config.yaml @tech-lead

# Security-sensitive areas
/src/auth/        @security-reviewer @tech-lead
/src/middleware/   @tech-lead

# API contracts
/src/api/         @api-owner
/openapi/         @api-owner
```

### Required status checks

Your CI pipeline should include these checks, and they should be required to pass before merge:

```yaml
# Example GitHub Actions workflow
name: PR Checks
on: [pull_request]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run lint

  typecheck:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run typecheck

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm test -- --coverage
      - name: Check coverage threshold
        run: |
          # Enforce coverage threshold from .team/config.yaml
          # Adapt this to your coverage tool

  # Optional: AI-assisted PR review
  # Runs the equivalent of /team-review as a CI check
  # This is advisory, not blocking
```

### Repository settings

| Setting | Recommended | Why |
|---------|-------------|-----|
| Allow merge commits | Team preference | — |
| Allow squash merging | ✅ Recommended for clean history | Each PR becomes one commit on main |
| Allow rebase merging | Team preference | — |
| Automatically delete head branches | ✅ Enabled | Clean up after merge |
| Allow auto-merge | ⚠️ Disabled initially | Enable once the team trusts the CI pipeline fully |

## GitLab

The equivalent settings exist under Settings → Repository → Protected Branches and Settings → Merge Requests. Key differences:

- "Allowed to push" should be set to "No one" for main
- "Allowed to merge" should require at least Maintainer role
- Merge request approvals configured under Settings → Merge Requests → Approvals

## Bitbucket

- Branch permissions under Repository Settings → Branch Restrictions
- Merge checks under Repository Settings → Merge Checks

## General principles

1. **The platform is the last line of defence.** The skill tells the agent the rules. The platform enforces them even when the agent doesn't listen.

2. **Treat CI as a required gate, not an advisory signal.** If tests fail on the PR, the PR doesn't merge. No exceptions.

3. **CODEOWNERS is your escalation path.** When the agent modifies something sensitive, the right human gets notified automatically.

4. **Audit trail matters more with AI.** Signed commits, AI-prefix in commit messages, and the AI-assistance checkbox in the PR template create a record of what was human-authored and what was AI-assisted.

5. **Start strict, loosen with trust.** Begin with all protections enabled. As the team builds confidence in the AI-assisted workflow, selectively relax constraints that create friction without adding safety.
