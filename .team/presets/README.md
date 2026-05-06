# Stack Presets

Presets provide sensible tooling defaults for common tech stacks. Set the
`preset` field in `.team/config.yaml` to load one:

```yaml
preset: node-typescript
```

## How presets work

- The preset file is loaded and its values are used as defaults.
- Any value you set explicitly in `config.yaml` overrides the preset.
- The merge is shallow: scalar values are replaced, list values (like
  `protected_paths`) are replaced entirely — not appended.
- To extend a preset's list, copy it into `config.yaml` and add your entries.
- Remove the `preset` line to configure everything manually.

## Available presets

| Preset | Stack | Test runner | Linter |
|---|---|---|---|
| `node-typescript` | Node.js + TypeScript | vitest | eslint via npm |
| `python` | Python | pytest | ruff |
| `go` | Go | go test | golangci-lint |

## Creating a custom preset

1. Copy an existing preset file and rename it (e.g., `ruby-rails.yaml`).
2. Set the tooling values for your stack.
3. Reference it in `config.yaml`: `preset: ruby-rails`

Preset files contain only tooling and tech-stack defaults:

- `test_framework` — the test runner name
- `test_command` — the shell command to run tests
- `e2e_framework` — E2E test tool, or `none` to disable
- `coverage_threshold` — minimum % coverage for new code
- `lint_command` — the linter shell command
- `type_check_command` — the type checker command, or `""` if not applicable
- `protected_paths` — files the agent should not modify without approval

Workflow and policy settings (methodology, branching, gates, etc.) belong in
`config.yaml`, not in presets.
