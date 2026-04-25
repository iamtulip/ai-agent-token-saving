# MEMORY

Project `d:\ai-agent-token-saving-notes` stores research and design notes for token-saving AI agents.

Existing note `token-saving-tools-for-codex-claude.md` catalogs tools such as RTK, Snip, Squeez, Lumen, claude-context, MCP optimizers, Tokscale, and output-style compressors.

Blueprint `HERMES-PERSONAL-TOKEN-AGENT.md` chooses Hermes as the base for a personal token-saving agent.

Token strategy: keep Hermes built-in memory small; use session search for old context; retrieve before reading large files; compress terminal output; measure with `tokscale` before adding more tools.

Recommended V1 stack: Hermes plus built-in memory, `tokscale`, and either `rtk` or `snip`. Add `Lumen` when codebase search becomes painful. Add MCP servers selectively with minimal tool exposure.

Security rule: treat MCP servers and install scripts as executable code. Review READMEs, install scripts, package manifests, and requested environment variables. Never store API keys or secrets in AI-readable memory.

