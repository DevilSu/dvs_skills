# <span style="color: #88C0D0;">**Vivado MCP Server Connection, Dual-Modality & Health-Check Guide**</span>

### <span style="color: #81A1C1;">**Filename:** `connect_to_vivado.md`</span>

| <span style="color: #EBCB8B;">**Field**</span> | <span style="color: #EBCB8B;">**Details**</span> |
| :--- | :--- |
| <span style="color: #EBCB8B;">**Author**</span> | devilsu |
| <span style="color: #EBCB8B;">**Created**</span> | 2026-10-03 20:30 PDT |
| <span style="color: #EBCB8B;">**Modified**</span> | 2026-10-03 20:30 PDT |
| <span style="color: #EBCB8B;">**Version**</span> | v1.0.0 |
| <span style="color: #EBCB8B;">**Status**</span> | <span style="color: #A3BE8C;">**Active Reference & Health-Check Guide**</span> |
| <span style="color: #EBCB8B;">**Supported Tool Versions**</span> | AMD Xilinx Vivado ML 2024.x / 2025.1 / 2025.2+ |
| <span style="color: #EBCB8B;">**MCP Servers Available**</span> | `vivado-gui` (Socket Port 7654) & `vivado-headless` (CLI Native) |

---

## <span style="color: #88C0D0;">**Table of Contents**</span>
- [<span style="color: #88C0D0;">**1. Purpose & The Mandatory Selection Prompt Rule**</span>](#1-purpose--the-mandatory-selection-prompt-rule)
  - [<span style="color: #81A1C1;">1.1 Dual Operational Purpose</span>](#11-dual-operational-purpose)
  - [<span style="color: #81A1C1;">1.2 Mandatory User Prompt Rule (Agent Directive)</span>](#12-mandatory-user-prompt-rule-agent-directive)
- [<span style="color: #88C0D0;">**2. Dual-Modality Architecture & Comparison (ASCII)**</span>](#2-dual-modality-architecture--comparison-ascii)
  - [<span style="color: #81A1C1;">2.1 Architecture Diagram</span>](#21-architecture-diagram)
  - [<span style="color: #81A1C1;">2.2 Detailed Comparison Matrix</span>](#22-detailed-comparison-matrix)
- [<span style="color: #88C0D0;">**3. Fresh Host Setup (One-Time Configuration on New PC)**</span>](#3-fresh-host-setup-one-time-configuration-on-new-pc)
  - [<span style="color: #81A1C1;">3.1 Install Prerequisites & Packages</span>](#31-install-prerequisites--packages)
  - [<span style="color: #81A1C1;">3.2 Configure Antigravity IDE `mcp_config.json`</span>](#32-configure-antigravity-ide-mcp_configjson)
  - [<span style="color: #81A1C1;">3.3 Configure Vivado GUI Auto-Load Script (`Vivado_init.tcl`)</span>](#33-configure-vivado-gui-auto-load-script-vivado_inittcl)
- [<span style="color: #88C0D0;">**4. Mode A: Vivado GUI MCP Server (`vivado-gui`)**</span>](#4-mode-a-vivado-gui-mcp-server-vivado-gui)
  - [<span style="color: #81A1C1;">4.1 How It Works</span>](#41-how-it-works)
  - [<span style="color: #81A1C1;">4.2 Starting the GUI Server Inside Vivado</span>](#42-starting-the-gui-server-inside-vivado)
  - [<span style="color: #81A1C1;">4.3 Verification & Health-Check Tool Calls</span>](#43-verification--health-check-tool-calls)
- [<span style="color: #88C0D0;">**5. Mode B: Vivado Headless MCP Server (`vivado-headless`)**</span>](#5-mode-b-vivado-headless-mcp-server-vivado-headless)
  - [<span style="color: #81A1C1;">5.1 How It Works</span>](#51-how-it-works)
  - [<span style="color: #81A1C1;">5.2 Managing Headless Background Sessions</span>](#52-managing-headless-background-sessions)
  - [<span style="color: #81A1C1;">5.3 Verification & Health-Check Tool Calls</span>](#53-verification--health-check-tool-calls)
- [<span style="color: #88C0D0;">**6. Session Health-Check Procedure (Run in Fresh Sessions)**</span>](#6-session-health-check-procedure-run-in-fresh-sessions)
- [<span style="color: #88C0D0;">**7. Troubleshooting Matrix**</span>](#7-troubleshooting-matrix)

---

## <span style="color: #88C0D0;">**1. Purpose & The Mandatory Selection Prompt Rule**</span>

### <span style="color: #81A1C1;">**1.1 Dual Operational Purpose**</span>

1. <span style="color: #EBCB8B;">**Fresh PC Setup Blueprint**</span>: Provides complete instructions to install and configure both Vivado MCP servers (`vivado-gui` and `vivado-headless`) on a newly formatted workstation or VM.
2. <span style="color: #EBCB8B;">**Fresh Session Verification Checklist**</span>: Acts as a standard operating procedure when the user asks <span style="color: #EBCB8B;">**"Check if Vivado MCP is ready using connect_to_vivado.md"**</span> at the beginning of a conversation.

### <span style="color: #81A1C1;">**1.2 Mandatory User Prompt Rule (Agent Directive)**</span>

> [!IMPORTANT]
> **MANDATORY AGENT BEHAVIOR WHEN USER POINTS TO THIS FILE:**
>
> Whenever the user references, opens, or asks to use [`connect_to_vivado.md`](file:///home/devilsu/Desktop/share_512GB/FPGA/notes/dvs_skills/skills/connect_to_vivado.md), the AI assistant **MUST immediately prompt the user** using `ask_question` (or direct text prompt) to specify which MCP server mode they wish to work with:
>
> - <span style="color: #88C0D0;">**Option 1 (Recommended for Visual Design): `vivado-gui` (Interactive GUI Mode)**</span>:
>   - *Use when*: You have the Vivado application open on your desktop, and want the assistant to inspect block designs graphically, highlight nets, open schematics, or program hardware over JTAG.
> - <span style="color: #81A1C1;">**Option 2 (Recommended for Automation): `vivado-headless` (Headless CLI Mode)**</span>:
>   - *Use when*: You want lightweight, fast, background execution without launching the Vivado GUI window (e.g. running batch synthesis, timing reports, or simulation regressions).

---

## <span style="color: #88C0D0;">**2. Dual-Modality Architecture & Comparison (ASCII)**</span>

### <span style="color: #81A1C1;">**2.1 Architecture Diagram**</span>

```text
                                   +------------------------------------------+
                                   |         ANTIGRAVITY AI ASSISTANT         |
                                   |         (MCP Client in IDE/CLI)          |
                                   +------------------------------------------+
                                           |                          |
               Tool Calls: `vivado-gui`    |                          | Tool Calls: `vivado-headless`
                                           v                          v
+===================================================+   +===================================================+
|        MODE A: vivado-gui (Socket Server)         |   |      MODE B: vivado-headless (Native Batch)       |
|                                                   |   |                                                   |
|   +-------------------------------------------+   |   |   +-------------------------------------------+   |
|   |  vivado-mcp-server Bridge (Python/pipx)   |   |   |   |  vivado-mcp-native Bridge (Python/pipx)   |   |
|   +-------------------------------------------+   |   |   +-------------------------------------------+   |
|                         | TCP Socket Port 7654    |   |                         | Background Process Pipe |
|                         v                         |   |                         v (VIVADO_PATH)           |
|   +-------------------------------------------+   |   |   +-------------------------------------------+   |
|   |  Running Vivado GUI Desktop Window        |   |   |   |  Headless Vivado Batch Process            |   |
|   |  - Sources vivado_server.tcl socket       |   |   |   |  - Spawned: vivado -mode tcl (no GUI)     |   |
|   |  - Visual Block Design (.bd) canvas       |   |   |   |  - Fast CI/CD, reports & batch runs       |   |
|   |  - Live Hardware Manager & ILA probes     |   |   |   |  - Controlled via stdout/stdin pipes      |   |
|   +-------------------------------------------+   |   |   +-------------------------------------------+   |
+===================================================+   +===================================================+
```

### <span style="color: #81A1C1;">**2.2 Detailed Comparison Matrix**</span>

| Feature / Capability | `vivado-gui` (GUI Mode) | `vivado-headless` (Headless Mode) |
| :--- | :--- | :--- |
| <span style="color: #EBCB8B;">**Server Name**</span> | `vivado-gui` | `vivado-headless` |
| <span style="color: #EBCB8B;">**Host Binary**</span> | `~/.local/bin/vivado-mcp-server --port 7654` | `~/.local/bin/vivado-mcp-native` |
| <span style="color: #EBCB8B;">**Requires Open Vivado Window?**</span> | **Yes** (Vivado GUI must be launched on desktop) | **No** (Starts/stops background processes on demand) |
| <span style="color: #EBCB8B;">**Visual Canvas Inspection**</span> | **Yes** (Block design canvas updates in real time) | **No** (Headless text-only output) |
| <span style="color: #EBCB8B;">**Hardware Manager & ILA Probes**</span> | **Yes** (`connect_hw`, `arm_ila`, `read_ila_data`) | Limited / Simulation-focused |
| <span style="color: #EBCB8B;">**Behavioral Simulation Engine**</span> | Available via TCL console | Built-in testbench runner (`launch_simulation`) |
| <span style="color: #EBCB8B;">**RAM & CPU Overhead**</span> | Higher (Full GUI desktop footprint) | Lower (Minimal background CLI instance) |
| <span style="color: #EBCB8B;">**Ideal Use Case**</span> | **Interactive FPGA design, IP integration & debug** | **Batch regressions, CI/CD, report extraction** |

---

## <span style="color: #88C0D0;">**3. Fresh Host Setup (One-Time Configuration on New PC)**</span>

### <span style="color: #81A1C1;">**3.1 Install Prerequisites & Packages**</span>

On a fresh Linux machine, install both MCP servers using `pipx`:

```bash
# Ensure pipx is installed
sudo apt-get update && sudo apt-get install -y pipx
pipx ensurepath

# Install both Vivado MCP servers
pipx install vivado-mcp-server
pipx install vivado-mcp-native
```

### <span style="color: #81A1C1;">**3.2 Configure Antigravity IDE `mcp_config.json`**</span>

Create or edit `/home/devilsu/.gemini/config/mcp_config.json`:

```json
{
  "mcpServers": {
    "vivado-gui": {
      "command": "/home/devilsu/.local/bin/vivado-mcp-server",
      "args": [
        "--port",
        "7654"
      ]
    },
    "vivado-headless": {
      "command": "/home/devilsu/.local/bin/vivado-mcp-native",
      "args": [],
      "env": {
        "VIVADO_PATH": "/tools/Xilinx/2025.1/Vivado/bin/vivado"
      }
    }
  }
}
```

*(Adjust paths if Vivado is installed in `/tools/Xilinx/...` or `/opt/Xilinx/...`).*

### <span style="color: #81A1C1;">**3.3 Configure Vivado GUI Auto-Load Script (`Vivado_init.tcl`)**</span>

To ensure the Vivado GUI automatically opens the socket server on port `7654` on startup:

Create or append to `~/.Xilinx/Vivado/Vivado_init.tcl`:

```tcl
# Auto-load vivado-mcp-server socket plugin for AI / MCP integration
if {[file exists "$::env(HOME)/.vivado-mcp-server/tcl/vivado_server.tcl"]} {
    source "$::env(HOME)/.vivado-mcp-server/tcl/vivado_server.tcl"
}
```

---

## <span style="color: #88C0D0;">**4. Mode A: Vivado GUI MCP Server (`vivado-gui`)**</span>

### <span style="color: #81A1C1;">**4.1 How It Works**</span>

- The Vivado GUI starts on your workstation.
- Its internal TCL interpreter sources `vivado_server.tcl` and listens for incoming JSON-RPC / socket commands on TCP port `7654`.
- The assistant sends commands through the `vivado-gui` MCP tools, which execute live inside your active project window.

### <span style="color: #81A1C1;">**4.2 Starting the GUI Server Inside Vivado**</span>

If Vivado is already running and the server was not loaded on startup, paste this into the **Vivado TCL Console**:

```tcl
source ~/.vivado-mcp-server/tcl/vivado_server.tcl
```
*(Vivado will output: `Listening on port 7654`)*.

### <span style="color: #81A1C1;">**4.3 Verification & Health-Check Tool Calls**</span>

To verify that the GUI server is alive and communicating:

```json
Tool: call_mcp_tool
Arguments: {
  "ServerName": "vivado-gui",
  "ToolName": "check_connection",
  "Arguments": {}
}
```

<span style="color: #A3BE8C;">**Expected Response:**</span>
```text
Vivado plugin is healthy.
  version: 2025.1
  timestamp: 1791084659
```

---

## <span style="color: #88C0D0;">**5. Mode B: Vivado Headless MCP Server (`vivado-headless`)**</span>

### <span style="color: #81A1C1;">**5.1 How It Works**</span>

- Operates independently without requiring the desktop GUI to be open.
- When an action is invoked, `vivado-mcp-native` launches a lightweight background `vivado -mode tcl` process.
- The assistant can start, monitor, and stop background Vivado sessions using session lifecycle tools.

### <span style="color: #81A1C1;">**5.2 Managing Headless Background Sessions**</span>

1. <span style="color: #EBCB8B;">**Start Session**</span>:
   `call_mcp_tool ServerName="vivado-headless", ToolName="start_session", Arguments: {}`
2. <span style="color: #EBCB8B;">**Check Status**</span>:
   `call_mcp_tool ServerName="vivado-headless", ToolName="session_status", Arguments: {}`
3. <span style="color: #EBCB8B;">**Stop Session**</span>:
   `call_mcp_tool ServerName="vivado-headless", ToolName="stop_session", Arguments: {}`

### <span style="color: #81A1C1;">**5.3 Verification & Health-Check Tool Calls**</span>

To verify the headless server status without launching a long job:

```json
Tool: call_mcp_tool
Arguments: {
  "ServerName": "vivado-headless",
  "ToolName": "session_status",
  "Arguments": {}
}
```

<span style="color: #A3BE8C;">**Expected Response:**</span>
```json
{
  "platform": "posix",
  "is_running": false,
  "output_encoding": "utf-8"
}
```

---

## <span style="color: #88C0D0;">**6. Session Health-Check Procedure (Run in Fresh Sessions)**</span>

Whenever a user starts a session and says <span style="color: #EBCB8B;">**"Check Vivado using connect_to_vivado.md"**</span>, follow this exact sequence:

1. <span style="color: #EBCB8B;">**Step 1: Ask User Preference**</span>:
   Prompt the user:
   > *"Would you like to connect using **`vivado-gui`** (Interactive desktop GUI on port 7654) or **`vivado-headless`** (background CLI)?*

2. <span style="color: #EBCB8B;">**Step 2: If User Selects `vivado-gui`**</span>:
   - Call `check_connection` on `vivado-gui`.
   - If successful, call `get_project_info` to report the currently open project, top module, and target part.
   - If failed (`Connection refused`), instruct the user to ensure Vivado is open and run `source ~/.vivado-mcp-server/tcl/vivado_server.tcl` in Vivado's TCL console.

3. <span style="color: #EBCB8B;">**Step 3: If User Selects `vivado-headless`**</span>:
   - Call `session_status` on `vivado-headless`.
   - Verify `VIVADO_PATH` resolves to the installed Vivado executable.
   - If not running, offer to call `start_session`.

---

## <span style="color: #88C0D0;">**7. Troubleshooting Matrix**</span>

| Symptom | Probable Cause | Corrective Action |
| :--- | :--- | :--- |
| `vivado-gui: Connection refused on port 7654` | Vivado GUI is not running or `vivado_server.tcl` was not sourced | Launch Vivado GUI on desktop. In TCL console, enter: `source ~/.vivado-mcp-server/tcl/vivado_server.tcl` |
| `vivado-gui: Server error: port already in use` | A previous instance of the TCL socket server was not closed | In Vivado TCL console, run `close [socket -server ...]` or restart Vivado |
| `vivado-headless: VIVADO_PATH not found` | The path in `mcp_config.json` points to wrong directory | Check output of `which vivado`. Update `VIVADO_PATH` in `~/.gemini/config/mcp_config.json` |
| `vivado-gui: Tool call hangs / timeout` | Vivado is executing a heavy blocking command (synthesis/impl) | Check Vivado GUI progress bar. Wait for current run to finish or call `get_run_status` |
| `Tool not found in MCP registry` | `mcp_config.json` was modified but IDE server not restarted | Restart Antigravity IDE or reload MCP servers |
