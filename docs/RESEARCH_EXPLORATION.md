# Research & Exploration Log

> **Concrete experiments, community findings, and resource summaries**

## 1. Boris Cherny — Claude Code Workflow

**Source**: [X thread by @bcherny](https://x.com/bcherny/status/2007179832300581177) (referenced in TRP challenge)

### Summary of Findings

| Practice | Description | Applicability to Cursor |
|----------|-------------|-------------------------|
| **Plan Mode first** | Start non-trivial tasks in Plan mode, iterate on scope before execution | Cursor has Plan mode; use before auto-accept |
| **Parallel sessions** | Run multiple agent instances (tabs/sessions) for different tasks | Cursor supports multiple Composer tabs |
| **Shared rules file** | Team maintains CLAUDE.md in git; update during code review | Use `.cursor/rules/` committed to repo |
| **Slash commands** | Custom commands for repeated workflows | Cursor has `/` commands; can add custom |
| **Model choice** | Prefer stronger model (Opus) for less steering | Cursor model selector; adjust per task |
| **Verification loop** | Give agent deterministic ways to verify work | Add rules for "run tests," "check CLI" |

### Takeaways Applied

- **Rules as living docs**: Treat rules file as something that evolves from observed mistakes
- **Plan before execute**: Encourage planning in rules to reduce rework
- **Verification**: Rules now include "verify commands match actual CLI" and "confirm provider supports feature"

---

## 2. Cursor Rules Best Practices

**Sources**:
- [cursor.fan — Using Cursor Rules Effectively](https://cursor.fan/tutorial/HowTo/using-cursor-rules-effectively/)
- [cursor.fan — MDC Rules Best Practices](https://cursor.fan/tutorial/HowTo/mdc-rules-best-practices/)
- [Medium — Mastering .mdc Files in Cursor](https://medium.com/@ror.venkat/mastering-mdc-files-in-cursor-best-practices-f535e670f651)

### Community Consensus

| Recommendation | Implementation |
|----------------|----------------|
| **Simple and focused** | One concern per rule; avoid overcomplicated logic |
| **Separate files** | Different rules for style, testing, docs (we use one agent.mdc for workspace scope) |
| **Iterative development** | Add rules when noticing repeated mistakes |
| **Project-specific** | Align with project conventions (async, protocols, etc.) |
| **Version control** | Commit rules; document changes in commits |
| **Avoid** | Complex conditionals; ignoring effectiveness; outdated rules |

### Experiments Performed

1. **Before rules**: Agent suggested `ai-content music --style jazz` without checking if `--prompt` was required → incorrect command
2. **After verification rule**: Agent checks CLI before suggesting → correct commands
3. **Before project context**: Agent sometimes gave generic advice → After adding paths table, more targeted suggestions
4. **Before provider-awareness**: Agent suggested lyrics for Lyria → After adding Lyria = instrumental, avoided wrong advice

---

## 3. MCP Ecosystem Research

**Sources**:
- [Cursor MCP Documentation](https://cursor.com/docs/context/mcp)
- [Cursor MCP CLI](https://cursor.com/docs/cli/mcp)
- [Developertoolkit — MCP Connection Issues](https://developertoolkit.ai/en/shared-workflows/mcp-ecosystem/mcp-connection-issues/)
- [Dredyson — Remote MCP Connection Failures](https://dredyson.com/i-tested-every-fix-for-remote-mcp-connection-failures-in-cursor-ide/)

### Findings

| Topic | Finding |
|-------|---------|
| **Transport types** | Cursor supports stdio (local), SSE, Streamable HTTP (remote) |
| **Config precedence** | Project → global → nested |
| **Verification** | `agent mcp list`, `agent mcp list-tools <id>` show status and tools |
| **Common failures** | "No Tools Found" (config/process), "spawn ENOENT" (path), "Fetch failed" (network/timeout) |
| **Logging** | Cursor isolates MCP processes; stdout/stderr not always visible; use Developer Tools or CLI |

### GitHub / Workflow Patterns

- **cursor-community/rules**: Shared rule examples for different stacks
- **MCP install links**: One-click install from directory; custom servers need `mcp.json`
- **OAuth for remote**: Static client ID/secret; redirect `cursor://anysphere.cursor-mcp/oauth/callback`

---

## 4. IDE Comparison Notes

| Aspect | Cursor | Claude Code | VS Code (Copilot) |
|--------|--------|-------------|-------------------|
| **Rules location** | `.cursor/rules/*.mdc` | `CLAUDE.md` | `.github/copilot-instructions.md` |
| **Rules format** | YAML frontmatter + markdown | Markdown | Markdown |
| **MCP support** | Native, Agent/Composer | CLI `agent mcp` | Via extensions |
| **File-specific rules** | `globs` in frontmatter | N/A | Repo-level |
| **Plan mode** | Yes | Yes (Shift+Tab) | Varies |

### Why Cursor for This Project

- MCP configuration in project `.cursor/mcp.json`
- Rules in `.cursor/rules/` apply to workspace
- Agent uses MCP tools when relevant
- Tenx MCP documentation references Cursor as supported IDE

---

## 5. Curated Resource List

See [CURATED_RESOURCES.md](CURATED_RESOURCES.md) for the full list used during research.

---

## 6. Experiments Summary

| # | Experiment | Result | Lesson |
|---|------------|--------|--------|
| 1 | Rule: "Verify commands" | Fewer wrong CLI suggestions | Explicit verification reduces hallucination |
| 2 | Rule: "Key paths" | Faster file references | Concrete paths > generic "explore codebase" |
| 3 | Rule: "Provider capabilities" | No lyrics suggested for Lyria | Domain constraints improve accuracy |
| 4 | Empty rules vs structured | Structured rules improved relevance | Context and constraints matter |
| 5 | Single rule file vs multiple | Single file sufficient for workspace scope | Start simple; split when rules grow |
