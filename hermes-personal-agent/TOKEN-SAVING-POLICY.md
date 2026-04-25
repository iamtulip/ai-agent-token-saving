# Token-Saving Policy For Hermes Personal Agent

## Goal

The agent should feel personal without carrying a large prompt. It should spend tokens on the current task, not on repeated boilerplate, stale context, or noisy tool output.

## Default Behavior

1. Answer in Thai unless the user asks otherwise.
2. Keep normal answers short and practical.
3. Use longer explanations only for architecture, safety, tradeoffs, debugging, or onboarding.
4. State assumptions briefly when acting without a question.
5. Prefer doing the next useful step over writing long plans.

## Context Rules

1. Search before reading large files or folders.
2. Read only files that are likely relevant to the current task.
3. Summarize large documents before using them as context.
4. Keep project-specific context in local notes or retrieval indexes, not always-loaded memory.
5. Start a new session when switching to a different major goal.

## Memory Rules

Save to `USER.md` only when the fact is stable and useful across many future sessions.

Good `USER.md` facts:

- Language and tone preferences.
- Preferred level of detail.
- Repeated workflow preferences.
- Tools the user actually uses.
- Explicit "remember this" requests.

Save to `MEMORY.md` only when the fact helps the agent operate in this environment or project.

Good `MEMORY.md` facts:

- Project purpose.
- Tooling choices.
- Setup decisions.
- Durable lessons learned.
- Known safety constraints.

Do not save:

- Raw logs.
- Large code snippets.
- Temporary paths.
- One-off debugging details.
- API keys, tokens, passwords, or private credentials.

## Tool Output Rules

1. Compress long terminal output.
2. Preserve errors, failing tests, file paths, changed files, and final summaries.
3. Remove progress bars, repeated warnings, dependency download noise, and unchanged boilerplate.
4. Use raw output only when debugging the compressor or when exact logs are required.

## MCP Rules

1. Add MCP only when it gives a clear capability the agent needs.
2. Prefer read-only tools for research, search, and memory lookup.
3. Expose the smallest useful toolset.
4. Disable dangerous or irrelevant tools.
5. Pass only explicitly required environment variables.
6. Review server source, install scripts, package manifests, and permissions before enabling.

## Prompt Cache Rules

1. Keep static instructions and stable tool definitions at the beginning of prompts.
2. Put task-specific user input near the end.
3. Avoid changing stable prompt prefixes during a session.
4. Track cached token counts when the model provider exposes them.
5. Keep templates stable so provider-side prompt caching can work.

## Measurement Rules

Measure before optimizing. Track:

- Input tokens.
- Output tokens.
- Cached tokens.
- Reasoning tokens when available.
- Tool output volume.
- Session length.
- Cost by model or provider.

Use measurement to decide whether to add compression, retrieval, MCP, or memory changes.

