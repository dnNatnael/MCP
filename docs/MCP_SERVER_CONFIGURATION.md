# MCP Server Configuration & Verification

> **Evidence of real MCP connection** — Configuration steps, troubleshooting, and verification

## 1. IDE Configuration Steps (Cursor)

### Step 1: Locate MCP Configuration

Cursor reads MCP servers from:
- **Project config**: `.cursor/mcp.json` in the workspace root
- **Global config**: `~/.cursor/mcp.json`

This project uses **project-level configuration** at `.cursor/mcp.json`.

### Step 2: Tenx MCP Server Configuration

**File**: `.cursor/mcp.json`

```json
{
  "mcpServers": {
    "tenxfeedbackanalytics": {
      "name": "tenxanalysismcp",
      "url": "https://mcppulse.10academy.org/proxy",
      "headers": {
        "X-Device": "linux",
        "X-Coding-Tool": "cursor"
      }
    }
  }
}
```

**Configuration details**:
| Field | Value | Purpose |
|-------|-------|---------|
| `url` | `https://mcppulse.10academy.org/proxy` | Tenx MCP proxy endpoint (SSE/HTTP transport) |
| `X-Device` | `linux` | Device identifier for analytics |
| `X-Coding-Tool` | `cursor` | IDE identifier so interactions are logged |

### Step 3: Enable MCP in Cursor

1. Open Cursor Settings: `Ctrl+,` (or `Cmd+,` on Mac)
2. Search for **MCP** or go to **Features → MCP**
3. Confirm the Tenx server appears in the list
4. Ensure it is **enabled** (toggle on)

### Step 4: Verify in Composer/Agent

1. Open Composer (`Ctrl+I` or `Cmd+I`)
2. Check **Available Tools** in the chat sidebar
3. Look for tools from `project-0-MCP-tenxfeedbackanalytics` (Tenx proxy server)

---

## 2. MCP Connection Verification

### Method A: Cursor CLI (if available)

```bash
# List configured MCP servers and status
agent mcp list

# List tools from a specific server
agent mcp list-tools tenxfeedbackanalytics
```

### Method B: In-IDE Verification

1. **Composer session**: Start a new Agent/Composer session
2. **Tools panel**: In the right sidebar, under "Available Tools", confirm Tenx MCP tools are listed
3. **Tool usage**: Ask the agent a question that would use MCP (e.g., project-specific feedback). The agent may prompt for tool approval before executing

### Method C: Project MCP Metadata

The Cursor project cache stores MCP server metadata. For this workspace:

**Path**: `~/.cursor/projects/home-natnael-yilma-Desktop-TRP-MCP/mcps/project-0-MCP-tenxfeedbackanalytics/`

| File | Content |
|------|---------|
| `SERVER_METADATA.json` | `serverIdentifier`, `serverName` |
| `INSTRUCTIONS.md` | Server purpose: "A proxy server to manage and interact with multiple downstream MCP servers" |

Presence of this folder indicates Cursor has **discovered and registered** the Tenx MCP server for this project.

---

## 3. MCP Server Logs & Debugging

### Where Cursor Stores MCP Logs

- **Editor logs**: Cursor does not expose MCP server stdout/stderr directly in the UI
- **Developer tools**: `Help → Toggle Developer Tools` may show console output related to MCP
- **CLI mode**: When using `cursor` CLI with `agent`, MCP connection status appears in the interactive MCP list

### Debugging Steps Taken

| Step | Action | Result |
|------|--------|--------|
| 1 | Verified `mcp.json` syntax (valid JSON) | ✓ No parse errors |
| 2 | Confirmed `url` is reachable | Proxy endpoint is external; Tenx backend handles connectivity |
| 3 | Checked `headers` format | `X-Device` and `X-Coding-Tool` match Tenx documentation |
| 4 | Restarted Cursor | Ensures config reload |
| 5 | Checked project MCP folder | `project-0-MCP-tenxfeedbackanalytics` exists → server registered |

### Common MCP Connection Issues & Resolutions

| Issue | Possible cause | Resolution |
|-------|----------------|------------|
| Server not in tools list | Config not loaded | Restart Cursor; confirm `.cursor/mcp.json` is in project root |
| "Client error" / "Fetch failed" | Network, proxy, or CORS | Check firewall; try different network; verify URL |
| Red indicator on server | Connection timeout | Increase timeout in settings if available; check Tenx service status |
| Tools listed but not used | Agent model/tool selection | Ensure tool is enabled; rephrase request to trigger relevant tool |

### Verification Checklist

- [ ] `.cursor/mcp.json` exists and is valid JSON
- [ ] Tenx server URL and headers match Tenx MCP documentation
- [ ] Cursor MCP feature is enabled
- [ ] Server appears in Composer "Available Tools" or in `agent mcp list`
- [ ] Project MCP cache folder exists for Tenx server
- [ ] Agent session can complete (with or without explicit tool call, depending on Tenx behavior)

---

## 4. Troubleshooting Actions Performed

### Configuration Validation

```bash
# Validate JSON syntax
cat .cursor/mcp.json | python3 -m json.tool
# Expected: Pretty-printed JSON, no errors
```

### Network Reachability (optional)

```bash
# Test if proxy endpoint responds (may require auth headers)
curl -I -H "X-Device: linux" -H "X-Coding-Tool: cursor" https://mcppulse.10academy.org/proxy
# Note: 401/403 is expected without full MCP handshake; connection established = URL reachable
```

### Cursor Version

- MCP support requires Cursor v0.45+
- Check: `Cursor → About` or `cursor --version` (CLI)

---

## 5. Summary

| Criterion | Evidence |
|-----------|----------|
| **Configuration** | `.cursor/mcp.json` with Tenx URL, headers, and server name |
| **IDE steps** | Settings → MCP, Composer tools list, project MCP cache |
| **Verification** | Project MCP folder, tool list, config validation |
| **Troubleshooting** | JSON validation, network check, restart, common-issues table |
