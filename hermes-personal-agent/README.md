# Hermes Personal Agent Kit

This folder contains the first working draft for a personalized Hermes-based agent focused on token efficiency.

## Files

- `USER.md`: compact user profile draft for Hermes user memory.
- `MEMORY.md`: compact agent/project memory draft for Hermes memory.
- `TOKEN-SAVING-POLICY.md`: operating rules for keeping context and output small.
- `SETUP-CHECKLIST.md`: phased setup plan before installing or enabling tools.
- `RESEARCH-SOURCES.md`: source list for continuing the research pack.

## V1 Direction

Start with a small stack:

1. Hermes Agent.
2. Built-in Hermes memory.
3. `tokscale` for token measurement.
4. `rtk` or `snip` for terminal output compression.
5. `Lumen` later when codebase search becomes a real bottleneck.

Avoid enabling many MCP servers in the first version. Each server can add tool descriptions, context, credentials risk, and debugging complexity.

## Memory Principle

Hermes memory should remember stable facts, not everything.

Keep always-loaded memory short. Use session search, local notes, or retrieval tools for details that do not need to appear in every prompt.

