# Hermes Personal Agent Setup Checklist

## Phase 1: Confirm Baseline

- Confirm where Hermes will run: Windows, WSL, local Linux, VPS, or cloud VM.
- Confirm primary model provider: OpenAI, OpenRouter, Anthropic, Nous Portal, or local endpoint.
- Confirm whether the first interface is terminal only or also chat platforms.
- Keep memory built-in only for V1.

## Phase 2: Prepare Memory

- Review `USER.md` and remove anything too personal, too vague, or not useful across future sessions.
- Review `MEMORY.md` and keep only stable project/environment facts.
- Keep both files short enough for Hermes memory limits.
- Do not include secrets.

## Phase 3: Add Measurement

- Add `tokscale` first so changes can be measured.
- Capture a baseline session before adding compression or retrieval.
- Track token use by tool, model, and session if available.

## Phase 4: Add Output Compression

- Choose one: `rtk` or `snip`.
- Test with common commands: `git status`, `git diff`, test runner output, package manager output, and directory listings.
- Verify that failing tests and error messages survive compression.
- Keep an escape hatch for raw output.

## Phase 5: Add Retrieval

- Add `Lumen` when the agent starts reading too much code.
- Index one real project first.
- Compare baseline vs retrieval-assisted sessions.
- Add `claude-context` only if MCP/vector-database workflows become necessary.

## Phase 6: Add MCP Selectively

- Add one MCP server at a time.
- Whitelist only tools the agent actually needs.
- Prefer read-only access first.
- Review environment variables passed to the server.
- Remove unused MCP servers.

## Phase 7: Review

- Check whether answers are still useful, not just shorter.
- Check whether token savings are visible in measurements.
- Check whether the setup is still easy to debug.
- Promote only proven rules into long-term memory.

