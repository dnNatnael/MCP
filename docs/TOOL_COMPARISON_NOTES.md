# Tool & IDE Comparison Notes

> **Comparison between AI coding tools and MCP servers**

## IDE / AI Assistant Comparison

| Feature | Cursor | Claude Code | VS Code + Copilot |
|---------|--------|-------------|-------------------|
| **Rules file** | `.cursor/rules/*.mdc` | `CLAUDE.md` (root) | `.github/copilot-instructions.md` |
| **Rule format** | YAML frontmatter + markdown | Plain markdown | Plain markdown |
| **MCP support** | Built-in (Composer/Agent) | CLI `agent mcp` | Extension-dependent |
| **Plan mode** | Yes | Yes (Shift+Tab x2) | Varies |
| **Context** | Workspace, @ mentions | Workspace, codebase search | Repo-level |
| **Agent model** | Configurable (Claude, etc.) | Claude (Opus/Sonnet) | Copilot/Chat |

## MCP Server Comparison (This Project)

| Server | Purpose | Transport | Status |
|--------|---------|-----------|--------|
| **Tenx MCP (tenxfeedbackanalytics)** | Analytics/logging; 10 Academy assessment | SSE (URL) | Configured |
| **cursor-ide-browser** | Browser automation for web dev | stdio | Available |
| **cursor-browser-extension** | Page interaction | stdio | Available |

## Rules System Comparison

| Aspect | Cursor .mdc | Claude Code CLAUDE.md |
|--------|-------------|------------------------|
| **Scope** | Per-file (globs) or always-apply | Repo-wide |
| **Structure** | Frontmatter (description, globs, alwaysApply) | Freeform markdown |
| **Multi-file** | Yes (separate .mdc files) | Single file |
| **Examples** | Common | Common |
| **Best for** | Large repos, mixed concerns | Simpler setups, team convention |

## Boris Cherny Practices vs. Our Implementation

| Cherny practice | Our implementation |
|-----------------|--------------------|
| Plan mode first | Documented in workflow; not enforced in rules |
| Parallel sessions | User choice; not rule-driven |
| Shared rules in git | Yes — `.cursor/rules/` committed |
| Slash commands | Not implemented; could add for common tasks |
| Verification loop | Yes — "verify commands," "run tests" in rules |
| Strong model preference | User choice; not specified in rules |

## Takeaways

1. **Cursor rules** are flexible: one file or many; always-apply or file-specific.
2. **MCP** enables tools beyond the IDE; Tenx uses remote proxy for centralized logging.
3. **Verification** is portable across tools (run commands, check output).
4. **Project context** matters more than tool choice for rule effectiveness.
