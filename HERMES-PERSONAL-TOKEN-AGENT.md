# Hermes Personal Token-Saving Agent Blueprint

Updated: 2026-04-25

## Reader And Goal

Reader: the owner of this workspace, building a personalized AI agent on top of Hermes.

Post-read action: decide the first Hermes-based architecture and implementation path for a personal agent that remembers the user while keeping token usage low.

## Executive Decision

Use Hermes as the base agent. Build token saving as an explicit operating layer around Hermes, not as a single prompt trick.

The first version should focus on four things:

1. Keep always-loaded memory small and curated.
2. Search before reading large context.
3. Compress noisy tool output.
4. Measure token usage before adding more tools.

## Why Hermes Fits This Project

Hermes is a strong fit because it already has the pieces needed for a personal agent:

- Persistent memory through `MEMORY.md` and `USER.md`.
- Session search for recalling past conversations without always injecting them.
- Skills for reusable procedures.
- Native MCP support for connecting external tools.
- Security controls for command approval, MCP credential filtering, context scanning, and isolation.

The most important design point is that Hermes memory is bounded. Its docs describe `MEMORY.md` as about 2,200 characters and `USER.md` as about 1,375 characters. That is good for token control because user preferences and environment facts stay compact instead of becoming a giant profile file.

Primary sources:

- https://hermes-agent.nousresearch.com/docs/user-guide/features/memory/
- https://github.com/NousResearch/hermes-agent
- https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp
- https://hermes-agent.nousresearch.com/docs/guides/use-mcp-with-hermes/
- https://hermes-agent.nousresearch.com/docs/user-guide/security/

## Agent Architecture

The agent should have five layers.

## 1. Personal Profile Layer

Purpose: remember the user without bloating every prompt.

Store only stable, high-value facts in Hermes `USER.md`:

- Preferred language and tone.
- Preferred response length.
- Tools the user actually uses.
- Recurring project types.
- Hard dislikes, such as "do not over-explain simple steps."

Do not store:

- Temporary debugging details.
- Large notes.
- Raw logs.
- Full project documentation.
- Secrets or API keys.

Rule: if a fact will not help in at least five future sessions, do not put it in always-loaded memory.

## 2. Working Memory Layer

Purpose: keep the current task coherent while avoiding long sessions.

Recommended behavior:

- Start a fresh session for a new major goal.
- Summarize current task state when switching topics.
- Use session search to recall old work instead of keeping everything active.
- Save only durable lessons into memory.

This matches Hermes's split between always-loaded memory and session search.

## 3. Retrieval Layer

Purpose: find the right files or notes before reading them.

Candidate tools:

- `ory/lumen` for local semantic code search.
- `zilliztech/claude-context` for MCP-based code search with hybrid search.
- Hermes optional `qmd` skill or MCP integration for local knowledge base retrieval.

Recommended first choice:

- Start with `Lumen` for local-first codebase search if privacy and simplicity matter.
- Consider `claude-context` later if the project needs vector database scale or MCP-first workflows.

Sources:

- https://github.com/ory/lumen
- https://github.com/zilliztech/claude-context
- https://github.com/NousResearch/hermes-agent/blob/main/optional-skills/research/qmd/SKILL.md

## 4. Output Compression Layer

Purpose: stop terminal output from eating the context window.

Candidate tools:

- `rtk-ai/rtk`: CLI proxy for reducing common development command output.
- `edouard-claude/snip`: YAML-driven shell output filtering.
- `claudioemmanuel/squeez`: hook-based compression with session memory and MCP read-only tools.

Recommended first choice:

- Use `rtk` or `snip` first because they are conceptually simple: command output goes in, shorter output comes out.
- Test `squeez` later if we want deeper hook integration, cross-call deduplication, and session-level token accounting.

Sources:

- https://github.com/rtk-ai/rtk
- https://github.com/edouard-claude/snip
- https://github.com/claudioemmanuel/squeez

## 5. Measurement Layer

Purpose: know where tokens go before optimizing blindly.

Candidate tool:

- `junhoyeo/tokscale` tracks usage across many AI coding tools, including Hermes Agent, Codex, Claude Code, Gemini CLI, Cursor, OpenClaw, and others.

Recommended first choice:

- Install measurement before adding many optimizers. If we cannot see token burn, we cannot prove the agent is getting better.

Source:

- https://github.com/junhoyeo/tokscale

## Prompt And Cache Strategy

For model providers that support prompt caching, structure prompts so stable content appears first and variable content appears last.

OpenAI's prompt caching docs say cache hits depend on exact prefix matches and recommend putting static instructions and examples before user-specific content. OpenAI also exposes cached token counts in usage data.

Anthropic's prompt caching docs use a similar principle: cache stable prompt prefixes and reuse them across repeated requests.

For our Hermes agent, this means:

- Keep system/persona/tool definitions stable across a session.
- Avoid constantly rewriting the top of the prompt.
- Put task-specific user input near the end.
- Keep frequently reused templates stable.
- Measure cached tokens when the provider exposes them.

Sources:

- https://platform.openai.com/docs/guides/prompt-caching
- https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching

## MCP Strategy

MCP should be used selectively. Hermes supports native MCP, toolsets, and config-level exposure control, but every MCP server can add context, tool descriptions, and security surface.

Default rule:

- Add MCP only when it gives a capability Hermes does not already have.
- Expose the smallest useful toolset.
- Disable dangerous or irrelevant tools.
- Avoid broad filesystem, shell, browser, or database access unless needed.
- Prefer read-only MCP tools for research and memory lookup.

Security rule:

- Treat every MCP server like executable code.
- Review install scripts and package manifests.
- Pass only explicitly needed environment variables.
- Do not expose secrets to MCP subprocesses by default.

Sources:

- https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp
- https://hermes-agent.nousresearch.com/docs/guides/use-mcp-with-hermes/
- https://hermes-agent.nousresearch.com/docs/user-guide/security/
- https://modelcontextprotocol.io/specification/draft/server/tools
- https://modelcontextprotocol.io/docs/concepts/resources

## First Implementation Plan

## Phase 1: Research Pack

Create a curated research pack from GitHub and official docs:

- Hermes memory, skills, MCP, security.
- Token compression tools.
- Semantic search tools.
- Token measurement tools.
- Provider prompt caching docs.

Output:

- A source list with notes.
- A short recommendation for each category.
- A risk note for each tool.

## Phase 2: Personal Agent Spec

Define the user's agent profile:

- Communication style.
- Languages.
- Tools used.
- Typical projects.
- What the agent should remember.
- What the agent must never store.
- Token-saving behavior rules.

Output:

- `USER.md` draft.
- `MEMORY.md` draft.
- Agent behavior policy.

## Phase 3: Hermes Configuration

Design minimal Hermes setup:

- Core Hermes config.
- Memory enabled.
- Only essential skills.
- Minimal MCP servers.
- Optional retrieval tool.
- Optional output compression wrapper.

Output:

- Config checklist.
- Install order.
- Rollback plan.

## Phase 4: Token Budget Experiments

Run small experiments:

- Baseline Hermes session.
- Hermes with short memory.
- Hermes plus output compressor.
- Hermes plus retrieval.
- Hermes plus token measurement.

Output:

- Before/after token numbers.
- Latency notes.
- Quality notes.
- Final recommended stack.

## Proposed V1 Stack

Start small:

1. Hermes Agent.
2. Hermes built-in memory.
3. `tokscale` for measurement.
4. `rtk` or `snip` for terminal output compression.
5. `Lumen` when codebase search becomes painful.
6. MCP only after we know exactly which tools are worth exposing.

Avoid in V1:

- Installing many MCP servers at once.
- Large memory providers before we know what needs to be remembered.
- Aggressive output style compressors before measuring quality loss.
- Any install command that pipes remote code directly into a shell without review.

## Success Metrics

The agent is working when:

- It remembers the user's stable preferences without needing repeated instructions.
- It can find relevant project files before reading large directories.
- Terminal output is compressed while preserving errors and failing tests.
- Token usage can be measured by tool, session, or model.
- The user can understand why each tool is installed.
- Removing one tool does not break the whole system.

## Open Decisions

1. Which machine will host Hermes: local Windows machine, WSL, VPS, or cloud VM?
2. Which model provider will be primary: OpenAI, OpenRouter, Anthropic, Nous Portal, or local endpoint?
3. Should memory stay built-in only at first, or should we add an external memory provider later?
4. Should the first retrieval layer target codebases, personal notes, or both?
5. Should this agent be used through terminal only, or also through chat platforms?

## Current Recommendation

Build the first version as a local Hermes agent with small curated memory, token usage measurement, and one output-compression tool. Add semantic search after the first real coding project exposes the need.

This keeps the first version useful, understandable, and easy to debug.
