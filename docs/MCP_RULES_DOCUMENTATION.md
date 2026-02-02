# MCP Setup Challenge — Rules Documentation

> **Task 2 & 3 Deliverable** — Documentation of rules file changes and insights

**Related artifacts**: [RULE_ITERATION_HISTORY.md](RULE_ITERATION_HISTORY.md) | [EXPERIMENTATION_LOG.md](EXPERIMENTATION_LOG.md) | [RESEARCH_EXPLORATION.md](RESEARCH_EXPLORATION.md) | [docs/README.md](README.md)

## What I Did

### Changes to `.cursor/rules/agent.mdc`

**Before**: The file contained only minimal frontmatter with `alwaysApply: true` and no guidance.

**After**: I added structured rules covering:

1. **Project Context** — Describes the workspace layout (MCP config + trp1-ai-artist) and points to project-specific rules when working inside ai-content
2. **Workflow Guidance** — Explore-first approach, verify commands before suggesting, document troubleshooting
3. **Key Paths** — Table of important directories (providers, presets, CLI, examples, docs) for quick reference
4. **Code Standards** — Async-first, result objects, registry pattern, UTC datetimes (aligned with ai-content’s RULES.md)
5. **Verification Before Suggesting** — Check installation options (uv vs pip), API key requirements, and provider capabilities before giving advice

### Design Principles Used

- **Simple and focused** — One main rule file, clear sections
- **Project-specific** — Tailored to TRP workspace and ai-content package
- **Actionable** — Concrete paths, commands, and checks instead of vague advice
- **Context-aware** — Defers to ai-content’s own rules when editing that package
- **Verification-first** — Reduces incorrect commands by checking compatibility and environment

## What Worked

1. **Project context section** — Helps the agent distinguish MCP config work from ai-content work
2. **Key paths table** — Speeds up navigation and reduces wrong-file suggestions
3. **Verification checklist** — Surfaces common pitfalls (e.g., Lyria vs MiniMax capabilities, uv availability)
4. **Pointer to CLAUDE.md and RULES.md** — Keeps ai-content–specific conventions in one place

## What Didn’t Work / Challenges

1. **Installation diversity** — User environment may lack `uv`, `python3-venv`, or `pipx`. Rules now mention fallbacks, but the agent still needs to handle environment-specific failures.
2. **CLI vs docs mismatch** — The challenge suggests `uv run ai-content music --style jazz` without `--prompt`, but the CLI currently requires `--prompt`. Documented this in CODEBASE_EXPLORATION.md and added a verification note in the rules.

## Insights Gained

### How Rules Change Agent Behavior

- **Context loading** — Explicit project structure and paths reduce guesses and off-target suggestions
- **Constraint articulation** — Rules like “verify commands match the actual CLI” and “confirm provider supports the feature” push the agent to validate before proposing solutions
- **Delegation** — Pointing to CLAUDE.md and RULES.md for ai-content keeps the main agent rules short while preserving detailed project standards
- **Failure awareness** — Mentioning common issues (e.g., Lyria instrumental-only, uv not installed) helps the agent proactively address them

### Best Practices Applied

1. **Living documentation** — Rules evolved from observed behavior (e.g., CLI requirements, installation failures)
2. **Iterative refinement** — Adding verification steps came from seeing incorrect command suggestions
3. **Separation of concerns** — General workspace rules in agent.mdc; ai-content specifics in its own RULES.md
4. **Concrete over abstract** — Paths and examples instead of generic “follow best practices”

### Comparison to Other AI Tools

- Similar to **Claude Code’s CLAUDE.md** — Living docs and project-specific guardrails
- Similar to **Boris Cherny’s approach** — Plan/verify before execution, deterministic checks
- **Cursor-specific** — `.mdc` format, `alwaysApply` for global context, `globs` for file-specific rules (not used here to keep scope broad)
