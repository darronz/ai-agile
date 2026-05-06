# Current Sprint

> Edit this file at sprint planning. It tells every developer's agent what the team is working on
> and helps flag potential conflicts between parallel work.

## Sprint goal

<!-- One sentence. What does this sprint deliver? -->

## Sprint backlog

| Ticket | Title | Assigned to | Branch | Status | Notes |
|--------|-------|-------------|--------|--------|-------|
| PROJ-123 | Add webhook support | alice | feat/PROJ-123-webhooks | In progress | Touches API layer |
| PROJ-124 | Fix queue race condition | bob | fix/PROJ-124-queue-race | Research | High risk — no test coverage |
| PROJ-125 | Dashboard loading perf | carol | perf/PROJ-125-dashboard | Not started | Frontend only |

## Known dependencies between tickets

<!-- Flag tickets that touch overlapping files or modules. This helps agents
     and reviewers anticipate merge conflicts. -->

- PROJ-123 and PROJ-124 both touch `src/services/queue.ts` — coordinate merge order.

## Carry-over from last sprint

<!-- Tickets that didn't complete. Note why and whether scope changed. -->
