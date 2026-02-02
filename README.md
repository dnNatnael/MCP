# TRP 1 – AI Content Generation & MCP Setup Challenge

**Candidate:** Ny  
**Role:** Forward Deployed Engineer (FDE)  
**Track:** AI Content Generation & MCP Configuration  
**Duration:** 2 Hours (TRP1) + 1 Hour (MCP)

---

## Overview

This repository documents my work for the **TRP 1 AI Content Generation Challenge** and the **MCP Setup Challenge**.

The goal of this challenge was not to build systems from scratch, but to:

- Explore an unfamiliar AI codebase  
- Understand how multi-provider AI systems work  
- Configure APIs and environments correctly  
- Generate real content (audio & video)  
- Setup a modern MCP-based AI coding environment  
- Document learnings, failures, and insights  

This reflects real-world Forward Deployed Engineer work: **operating complex systems under ambiguity**.

---

# Part 1 – Environment Setup

## Tools Used

- OS: Ubuntu Linux  
- Python: 3.11  
- Package manager: `uv`  
- IDE: Cursor  
- MCP Server: Tenx MCP  
- Providers used:
  - Google Gemini (Lyria, Veo)
  - AIMLAPI (MiniMax – optional)

---

## Installation Steps

```bash
git clone https://github.com/10xac/trp1-ai-artist.git
cd trp1-ai-artist

cp .env.example .env
uv sync
uv run ai-content --help
src/ai_content/
│
├── cli.py                # Main CLI entry
├── providers/            # AI model integrations
│   ├── lyria.py
│   ├── minimax.py
│   └── veo.py
│
├── pipelines/            # Orchestration layer
│   ├── music.py
│   └── video.py
│
├── presets/              # Style templates
│   ├── music.yaml
│   └── video.yaml
