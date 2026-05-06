## /team-retro

Pull metrics from git history for the current or most recent sprint. Format them for a human retrospective conversation. This provides data, not conclusions.

### Instructions

### Preset resolution

Read `.team/config.yaml`. If a `preset` field is set, load `.team/presets/<preset>.yaml` and merge its values as defaults. The retro command primarily uses git history and sprint data, but the resolved config is needed for identifying AI-assisted metrics (the `ai_commit_prefix` field).

Read `.team/SPRINT.md` for the sprint scope and assignments.

Determine the sprint date range from the SPRINT.md or ask the developer for the start and end dates.

**Gather metrics from git history:**

**Throughput**
- Tickets completed (branches merged to main during the sprint)
- PRs merged: count, average size (lines changed), largest PR
- Commits per developer

**Cycle time**
- For each completed ticket: time from first commit on feature branch to merge
- Average, median, fastest, slowest
- If data is available: time from ticket creation to merge (requires tracker integration)

**Churn**
- Files that were modified most frequently across all branches
- Files that appeared in the most PRs (potential hotspots or conflict zones)
- Reverted commits or force pushes (if any)

**Rework**
- PRs that required multiple rounds of review (commits after first review)
- Tests that were added after initial implementation (late test coverage)
- Fixup commits or "fix review feedback" commits

**Scope**
- Tickets that were carried over from previous sprint
- Tickets that were added mid-sprint
- FINDINGS.md entries generated during execution (out-of-scope issues found)
- Any branches that were abandoned or significantly re-scoped

**AI-assisted metrics (if ai_commit_prefix is enabled)**
- Ratio of AI-assisted vs manual commits
- Which areas of the codebase had most AI involvement
- Any patterns in which types of work were AI-assisted vs manual

### Output

```
# Sprint Retrospective Data — <sprint dates>

## Throughput
- Tickets completed: <N> / <planned>
- PRs merged: <N>
- Average PR size: <lines> (largest: <lines> in <PR>)

## Cycle time
- Average: <time>
- Median: <time>
- Fastest: <ticket> — <time>
- Slowest: <ticket> — <time>

## Hotspots
<Files that appeared in 3+ PRs — potential architecture or ownership issues>

## Rework
- PRs with post-review changes: <N>
- Late test additions: <N>

## Scope changes
- Carried over: <list>
- Added mid-sprint: <list>
- Abandoned: <list>

## Findings backlog
<Out-of-scope issues discovered during the sprint, aggregated from FINDINGS.md files>

## AI-assisted work
<Summary if data available>

---
*This data is for discussion, not judgment. The team decides what it means.*
```

### Important

This command gathers data. It does NOT:
- Draw conclusions about team performance
- Make recommendations about process changes
- Evaluate individual developers
- Suggest that any metric is "good" or "bad"

The retrospective conversation is human. This command just makes sure the conversation is informed by actual data rather than selective memory.
