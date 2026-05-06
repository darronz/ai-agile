## /team-validate

Generate and run validation checks against the acceptance criteria for a completed ticket. This bridges the gap between "tests pass" and "the thing actually works as specified."

### Instructions

Read the project context:
- `CLAUDE.md` for architecture context
- `.team/config.yaml` for test frameworks and methodology
- The groomed ticket (acceptance criteria)

**1. Map acceptance criteria to test coverage**

For each acceptance criterion:
- Does a test exist that specifically validates this criterion?
- Is the test meaningful (tests the behaviour, not just that code runs)?
- Rate coverage: ✅ covered / ⚠️ partially covered / ❌ not covered

**2. Generate missing test scenarios**

For any criterion that is not fully covered, generate test cases:

If `methodology: bdd`:
- Generate Gherkin scenarios that aren't yet implemented
- Include edge cases the developer may not have considered

If `methodology: tdd`:
- Generate test descriptions with expected inputs and outputs
- Focus on boundary conditions, error paths, null/empty cases

If `methodology: standard`:
- Generate test cases as a checklist with steps and expected results

**3. Edge case analysis**

Go beyond the acceptance criteria and consider:
- What happens with empty/null input?
- What happens with very large input?
- What happens when external services are unavailable?
- What happens with concurrent requests?
- What happens with malformed data?
- What happens with unauthorised access?

Generate test scenarios for any edge cases not already covered.

**4. Integration check**

If the change touches multiple modules or services:
- Are there integration tests that verify the interaction?
- Could this change break existing functionality elsewhere?
- Run the full test suite and report any failures outside the changed files

**5. Manual test script (for UAT)**

Produce a manual testing script the PO or QA person can follow:

```
# Manual Test Script: <ticket-id>

## Prerequisites
<Environment setup, test data needed, user roles required>

## Test cases

### TC1: <scenario name>
1. <step>
2. <step>
3. <step>
Expected: <what should happen>

### TC2: <scenario name>
1. <step>
...
```

### Output

```
# Validation Report: <ticket-id>
Date: <date>

## Acceptance criteria coverage
| Criterion | Test exists | Quality | Gaps |
|-----------|------------|---------|------|
| <criterion> | ✅ | Good | None |
| <criterion> | ⚠️ | Partial | <missing edge case> |
| <criterion> | ❌ | Missing | <needs test> |

## Generated test scenarios
<New test cases to add — grouped by type>

## Edge cases identified
<Scenarios not in acceptance criteria that should be tested>

## Integration concerns
<Cross-module or cross-service risks>

## Manual test script
<For UAT — paste this into the ticket or hand to QA>

## Recommendation
<Is this ready for UAT? What should be addressed first?>
```

### Important

This command can generate test code but should ask the developer before adding it to the codebase. Generated tests go through the same branch/commit/review cycle as any other code.

The manual test script is the artefact that makes UAT possible without the PO needing to understand the implementation. It translates acceptance criteria into "click here, type this, see that."
