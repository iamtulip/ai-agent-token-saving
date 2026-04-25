# AI Agent Token Saving Notes

This repo collects notes and draft configuration for building a personal AI agent whose main job is to reduce token waste in AI coding workflows.

This repo is a personal research and configuration note repository. It is not yet a packaged CLI tool.

## Goal

Build an agent that helps the user spend tokens on the current task instead of repeated boilerplate, noisy terminal output, oversized memory, or irrelevant files.

The agent should act as a token budget assistant:

- Search before reading large folders or files.
- Keep always-loaded memory short and curated.
- Compress terminal output while preserving errors and failing tests.
- Measure token usage before adding more tools.
- Add MCP servers and retrieval tools only when they solve a real bottleneck.

## Recommended V1

Start with a small stack:

1. Hermes Agent as the personal-agent base.
2. Hermes built-in `USER.md` and `MEMORY.md`.
3. `tokscale` for measuring usage.
4. One terminal output compressor: `rtk` or `snip`.
5. `Lumen` later, when codebase search becomes a real bottleneck.

Avoid installing many MCP servers at the start. Each server can add context, security surface, credentials risk, and debugging complexity.

## Files

- `RUNBOOK.md`: practical workflow for using and testing the token-saving agent.
- `HERMES-PERSONAL-TOKEN-AGENT.md`: full blueprint and architecture notes.
- `TOKEN-SAVING-AGENT-V1.md`: concise operating spec for the first agent version.
- `token-saving-tools-for-codex-claude.md`: tool catalog for Codex, Claude Code, Hermes, and related CLIs.
- `hermes-personal-agent/`: first Hermes memory and policy draft.
- `experiments/`: repeatable experiment notes for baseline measurement, output compression, and retrieval tests.

## Next Practical Step

Read `RUNBOOK.md`, use `TOKEN-SAVING-AGENT-V1.md` as the agent behavior contract, then run a baseline token measurement before installing compression or retrieval tools.
