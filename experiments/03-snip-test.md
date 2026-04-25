# Experiment 03: Snip Output Compression Test

## Goal

Test whether `snip` reduces terminal output token waste while keeping errors, failing tests, paths, and changed files visible.

## Setup

- Date:
- Working folder:
- Agent/tool:
- Model/provider:
- Snip version:
- Snip config file:
- Commands tested:

## Baseline Reference

Compare against:

- `experiments/01-baseline-token-measurement.md`

## Test Commands

Use commands that usually create noisy output:

```bash
git status
git diff
npm test
npm run build
pytest
```

Adjust the command list to match the project.

## Measurements

- Input tokens before:
- Output tokens before:
- Total tokens before:
- Input tokens after:
- Output tokens after:
- Total tokens after:
- Runtime change:

## Quality Checks

- Were failing tests preserved?
- Were error messages preserved?
- Were file paths preserved?
- Were changed files preserved?
- Was useful context removed accidentally?
- Was the YAML filtering easy to understand?

## Decision

Use Snip in regular workflow:

- Yes / No / Not yet

Reason:

```text

```

