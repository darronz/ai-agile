## /team-setup

Audit this repository's configuration against recommended guardrails for AI-assisted team development. Produce a checklist the tech lead can act on.

### Instructions

Read `.team/config.yaml` to understand the team's configured methodology and policies.

Then check each of the following and report the status:

**Repository structure**
- [ ] `CLAUDE.md` exists at repo root with coding standards and architecture context
- [ ] `.team/config.yaml` exists and is valid
- [ ] `.team/CONVENTIONS.md` exists
- [ ] `.team/SPRINT.md` exists
- [ ] `.github/PULL_REQUEST_TEMPLATE.md` exists

**Branch protection (check via `gh` CLI if available, otherwise advise manually)**
- [ ] Main branch has branch protection enabled
- [ ] PRs required before merge
- [ ] At least N approvals required (N from config.yaml `review_required`)
- [ ] Status checks required before merge (CI must pass)
- [ ] Force pushes disabled on main
- [ ] Branch deletion protection on main

**CODEOWNERS**
- [ ] `.github/CODEOWNERS` file exists
- [ ] Protected paths from config.yaml have designated owners

**CI pipeline**
- [ ] Lint check runs on PR
- [ ] Type check runs on PR (if applicable per config)
- [ ] Test suite runs on PR
- [ ] Coverage threshold is enforced (threshold from config.yaml)

**Commit hygiene**
- [ ] `.gitignore` excludes agent working directories (`.planning/`, local state files)
- [ ] Commit signing configured (advisory, not required)

**PR template**
- [ ] Template includes ticket reference field
- [ ] Template includes AI-assistance disclosure
- [ ] Template includes scope confirmation
- [ ] Template includes testing instructions section

### Output format

```
# Team SDLC Setup Audit — <date>

## ✅ Configured correctly
- <item>

## ⚠️ Recommended
- <item> — <what to do>

## ❌ Missing
- <item> — <what to do and why it matters>

## Manual steps required
- <items that need to be done in GitHub/GitLab settings UI>
```

If the `gh` CLI is available, offer to apply fixable settings automatically (branch protection rules). Always confirm with the developer before making changes.
