# Team SDLC Skill

AI-augmented agile development for real teams. Not a framework. Not an orchestrator. A set of conventions and commands that make your existing process work better with AI assistance.

## Philosophy

This skill does not replace your team's way of working. It augments each phase of a standard agile SDLC by giving every developer an AI assistant that understands the project, respects boundaries, and enforces the standards the team has agreed on.

The principles:

- **Humans decide, AI prepares.** The AI researches, drafts, implements, flags. Humans prioritise, commit, approve, accept.
- **Shared context, scoped execution.** Project-level docs are shared and version-controlled. Developer working state is local to their branch.
- **Git is the integration bus.** No AI orchestrator decides what merges where. PRs, code review, and CI are the coordination layer.
- **Tool-agnostic.** The team standardises on context documents and git workflow, not on which AI tool each developer uses. Claude Code, Cursor, Windsurf, Copilot — the skill commands work the same way because they read the same project context.
- **Process without theatre.** Every step exists because it prevents a real problem, not because a methodology says it should.

## What's in the box

```
your-repo/
├── CLAUDE.md                          # Agent behaviour, coding standards, architecture
├── .team/
│   ├── config.yaml                    # Methodology, branching, quality gates
│   ├── CONVENTIONS.md                 # PR templates, commit format, review checklist
│   ├── BACKLOG.md                     # Or MCP connector to your tracker
│   └── SPRINT.md                      # Current sprint scope and assignments
├── .claude/
│   └── commands/
│       ├── team-research.md           # /team-research <ticket>
│       ├── team-groom.md              # /team-groom <ticket>
│       ├── team-plan.md               # /team-plan <ticket>
│       ├── team-execute.md            # /team-execute
│       ├── team-review.md             # /team-review
│       ├── team-validate.md           # /team-validate
│       ├── team-retro.md              # /team-retro
│       └── team-setup.md              # /team-setup (repo audit)
└── .github/
    ├── branch-protection.md           # Recommended config (reference doc)
    └── PULL_REQUEST_TEMPLATE.md       # Enforced PR structure
```

## Quick start

1. Copy the `.team/`, `.claude/commands/`, and `.github/` directories into your repo root.
2. Write your `CLAUDE.md` — coding standards, architecture decisions, domain concepts.
3. Edit `.team/config.yaml` to match your team's methodology.
4. Run `/team-setup` to audit your repo configuration against recommended guardrails.
5. Each developer works in their branch. Each developer uses whichever AI tool they prefer. The shared context keeps output consistent.

## Commands

| Command | Phase | Who runs it | Output |
|---|---|---|---|
| `/team-setup` | Setup | Tech lead, once | Repo config audit and checklist |
| `/team-research <ticket>` | Discovery | Any developer | Research summary with findings, risks, open questions |
| `/team-groom <ticket>` | Refinement | Any developer | Refined ticket with acceptance criteria and complexity assessment |
| `/team-plan <ticket>` | Planning | Assigned developer | Implementation plan scoped to the ticket, in current branch |
| `/team-execute` | Implementation | Assigned developer | Code, tests, commits in feature branch |
| `/team-review` | Pre-PR review | Assigned developer | Self-review report, gap analysis against acceptance criteria |
| `/team-validate` | Testing | Developer or QA | Test scenarios from acceptance criteria, coverage check |
| `/team-retro` | Retrospective | Tech lead | Sprint metrics pulled from git history |

## What this is not

- Not a replacement for your project tracker. Use Jira, Linear, GitHub Issues, a whiteboard — whatever works. The skill reads tickets, it doesn't manage them.
- Not a multi-agent orchestration system. Each developer runs their own AI session in their own branch. Coordination happens through git and human conversation.
- Not prescriptive about AI tooling. The commands are written for Claude Code but the principles apply to any agent that can read markdown context files.
- Not a substitute for engineering judgment. The AI flags problems and suggests approaches. The developer decides.
