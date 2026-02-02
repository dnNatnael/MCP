# Experimentation Log

> **Concrete experiments** with rules, MCP, and agent behavior

## Experiment 1: Empty vs. Contextful Rules

**Date**: During initial setup  
**Setup**: Compare agent behavior with empty `agent.mdc` vs. version with project context

**Prompt**: "Where should I add a new video preset?"

| Rules state | Response quality |
|-------------|------------------|
| Empty | Generic "check project structure"; sometimes suggested wrong paths |
| With key paths | Correctly pointed to `trp1-ai-artist/src/ai_content/presets/video.py` |

**Conclusion**: Explicit paths significantly improve accuracy.

---

## Experiment 2: CLI Command Verification

**Date**: After first rules iteration  
**Setup**: Ask for music generation command with different rule versions

**Prompt**: "Give me the command to generate 30 seconds of jazz with Lyria"

| Rules state | Response |
|-------------|----------|
| No verification rule | `uv run ai-content music --style jazz --provider lyria --duration 30` (correct after CLI fix) or sometimes included redundant `--prompt` |
| With verification rule | Agent confirms style/prompt logic; suggests exact command; notes Lyria = instrumental |

**Conclusion**: Verification rule reduces command errors.

---

## Experiment 3: Provider Capability Awareness

**Date**: During rules refinement  
**Setup**: Ask for vocal music generation

**Prompt**: "I want to generate a song with lyrics"

| Rules state | Response |
|-------------|----------|
| No provider rule | Sometimes suggested Lyria + lyrics (incorrect) |
| With capability rule | Correctly suggests MiniMax + `--lyrics`; warns Lyria doesn't support vocals |

**Conclusion**: Explicit provider constraints prevent wrong suggestions.

---

## Experiment 4: MCP Tool Visibility

**Date**: Post MCP config  
**Setup**: Check if Tenx MCP tools appear in Composer

**Method**:
1. Open Composer
2. Inspect "Available Tools" in sidebar
3. Look for `project-0-MCP-tenxfeedbackanalytics` or Tenx-related tools

**Result**: Tools listed (or server present) when MCP config is valid and Cursor has loaded it. Connection status may vary by session.

---

## Experiment 5: Rule Length vs. Relevance

**Date**: During refinement  
**Setup**: Compare short vs. detailed rules

**Observation**: Very long rules (e.g., full RULES.md content in agent.mdc) can dilute focus. Concise, actionable rules with pointers to detailed docs performed better.

**Conclusion**: Keep agent.mdc focused; delegate to project-specific files.

---

## Experiment 6: Cursor vs. Other Context

**Date**: Research phase  
**Setup**: Compare Cursor rules to Claude Code CLAUDE.md structure

**Finding**: Claude Code uses single CLAUDE.md at repo root. Cursor uses `.cursor/rules/` with multiple .mdc files. For this workspace, single agent.mdc with `alwaysApply: true` matched our needs.

---

## Summary Table

| # | Experiment | Key variable | Outcome |
|---|------------|--------------|---------|
| 1 | Path awareness | Key paths in rules | +Accuracy |
| 2 | CLI commands | Verification rule | +Correctness |
| 3 | Provider advice | Capability constraints | +Correctness |
| 4 | MCP visibility | Config + restart | Tools available |
| 5 | Rule length | Conciseness | +Relevance |
| 6 | IDE comparison | Cursor vs Claude | Structure differs; both work |
