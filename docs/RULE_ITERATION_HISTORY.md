# Rule Iteration History

> **Evidence of iterative refinement** — Changes to agent rules across versions

## Version 0 — Initial State

**File**: `.cursor/rules/agent.mdc`  
**Date**: Before challenge

```yaml
---
alwaysApply: true
---
```

**Content**: Empty. No guidance for the agent.

**Observed behavior**: Generic suggestions; no project awareness; commands sometimes incorrect (e.g., Lyria with lyrics).

---

## Version 1 — Project Context & Paths

**Focus**: Give agent awareness of workspace structure

**Added**:
- Project context (MCP config + trp1-ai-artist)
- Key paths table (providers, presets, CLI, examples)
- Pointer to trp1-ai-artist CLAUDE.md and RULES.md

**Rationale**: Agent frequently suggested edits without knowing where files lived.

**Test**: Asked "Where are the music providers?" — Agent correctly pointed to `trp1-ai-artist/src/ai_content/providers/`.

---

## Version 2 — Workflow & Verification

**Focus**: Reduce incorrect commands and improve reliability

**Added**:
- Workflow guidance: explore before implementing, verify commands, document troubleshooting
- Verification checklist: installation (uv vs pip), API keys, provider capabilities

**Rationale**: Agent suggested `ai-content music --style jazz` but CLI initially required `--prompt`. Also suggested lyrics for Lyria (instrumental only).

**Test**: Asked "How do I generate jazz music?" — Agent now checks CLI signature and suggests correct flags; notes Lyria = instrumental.

---

## Version 3 — Code Standards (Current)

**Focus**: Align with ai-content conventions when editing

**Added**:
- Code standards: async-first, result objects, registry pattern, UTC datetimes
- Refined verification: Lyria vs MiniMax capabilities explicitly called out

**Rationale**: When editing ai-content code, agent should follow project RULES.md (async, protocols, etc.).

**Test**: Asked "Add a new music preset" — Agent suggests correct pattern (MusicPreset dataclass, add to MUSIC_PRESETS dict).

---

## Changelog Summary

| Version | Sections added | Trigger |
|---------|----------------|---------|
| 0 → 1 | Project context, key paths, delegation | No project awareness |
| 1 → 2 | Workflow, verification | Wrong commands, wrong provider advice |
| 2 → 3 | Code standards, capability checks | Code style inconsistency |

---

## Rules Not Adopted (and Why)

| Idea | Reason |
|------|--------|
| File-specific rules (`globs`) | Workspace is small; single always-apply rule sufficient |
| Multiple rule files (style, testing, docs) | Overkill for this scope; one file easier to maintain |
| Slash commands | Not needed for current workflows |
| Auto-run MCP tools | Prefer explicit approval for Tenx tools |

---

## Metrics (Qualitative)

| Metric | V0 | V1 | V2 | V3 |
|--------|----|----|----|-----|
| Correct CLI commands | Low | Medium | High | High |
| Project path accuracy | Low | High | High | High |
| Provider capability accuracy | Low | Low | High | High |
| Code style alignment | N/A | N/A | Medium | High |
