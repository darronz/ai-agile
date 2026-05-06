## /team-review

Self-review the current branch before raising a PR. This catches problems before a human reviewer has to find them.

### Instructions

Read the project context:
- `CLAUDE.md` for coding standards and architecture
- `.team/config.yaml` for policies and quality thresholds
- `.team/CONVENTIONS.md` for PR expectations and definition of done
- The groomed ticket (acceptance criteria)
- The implementation plan from `/team-plan`

**Review the diff against the base branch.**

Use `git diff <base-branch>...HEAD` to see everything this branch changes.

Check each of the following:

**1. Acceptance criteria coverage**
For each acceptance criterion in the groomed ticket:
- Is it implemented? Point to the specific code.
- Is it tested? Point to the specific test.
- If any criterion is NOT met: flag it as CRITICAL.

**2. Scope compliance**
- Does the diff modify only files identified in the plan?
- Are there any "while I'm here" changes?
- Are there any refactors outside the ticket scope?
- If scope drift is detected: flag it as CRITICAL.

**3. Test quality**
- Do tests verify behaviour, not implementation details?
- Are error/edge cases covered?
- Is the coverage threshold from config.yaml met for new code?
- Are there any tests that are trivially passing (testing the mock, not the code)?

**4. Code quality**
- Does the code follow patterns established in the codebase?
- Are there any obvious performance issues?
- Are error cases handled?
- Is there appropriate logging/observability?
- Are there hardcoded values that should be configurable?

**5. Security check**
- No secrets, tokens, or credentials in the diff
- No new SQL without parameterisation
- No new user input without validation/sanitisation
- No new endpoints without authentication/authorisation checks
- No changes to auth logic without explicit ticket scope

**6. Protected paths**
- Were any protected paths (from config.yaml) modified?
- If yes: was explicit approval given during execution?

**7. Dependencies**
- Were any new dependencies added?
- If yes: are they justified and noted for the PR description?
- Any known vulnerabilities in new dependencies?

**8. Documentation**
- Check docs requirements from config.yaml
- If `api_docs: required`: are new/changed endpoints documented?
- If `changelog: per_pr`: is the changelog entry drafted?
- If `adr: when_architectural`: does this change warrant an ADR?

**9. Commit hygiene**
- Do commits follow the convention?
- Are commits atomic (each one leaves the code working)?
- Is the `[ai]` prefix present if required by config?

### Output

```
# Self-Review: <ticket-id> — <branch-name>
Date: <date>

## Summary
<2-3 sentences: what this branch delivers>

## Acceptance criteria
| Criterion | Implemented | Tested | Status |
|-----------|-------------|--------|--------|
| <criterion> | ✅ file:line | ✅ test file:line | PASS |
| <criterion> | ✅ file:line | ❌ missing | FAIL |

## Checks
| Check | Status | Notes |
|-------|--------|-------|
| Scope compliance | ✅/⚠️/❌ | <detail if not clean> |
| Test coverage | ✅/❌ | <% for new code> |
| Code quality | ✅/⚠️ | <concerns if any> |
| Security | ✅/⚠️/❌ | <issues if any> |
| Protected paths | ✅/⚠️ | <modifications if any> |
| Dependencies | ✅/⚠️ | <new deps if any> |
| Documentation | ✅/❌ | <missing items if any> |
| Commit hygiene | ✅/⚠️ | <issues if any> |

## Critical issues
<List anything that MUST be fixed before PR — or "None">

## Warnings
<List anything the reviewer should pay attention to — or "None">

## PR description draft
<Pre-filled PR description following the template in .team/CONVENTIONS.md>

## Findings for backlog
<Out-of-scope issues from FINDINGS.md, formatted as potential tickets>
```

### Gate check

If any CRITICAL issues are found:
- Do NOT advise raising a PR
- List what needs to be fixed
- Suggest running `/team-execute` to address gaps (within scope only)

If only warnings:
- Advise raising the PR with warnings noted in the description
- The human reviewer will make the call

If clean:
- Advise raising the PR
- Provide the draft PR description ready to paste
