# Leave Management MCP Server

An MCP (Model Context Protocol) server for employee leave management, built with FastMCP and Python.

## Features

- **Check leave balance** — query remaining leave days for an employee
- **Apply for leave** — submit leave requests for specific dates
- **View leave history** — retrieve past leave records
- **Greeting resource** — personalized greeting endpoint

---

## Prerequisites

- [Python 3.14+](https://www.python.org/downloads/)
- [uv](https://docs.astral.sh/uv/getting-started/installation/) — fast Python package manager
- [Claude Desktop](https://claude.ai/download) (to use the server with Claude)

---

## Environment Setup

### 1. Install `uv`

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Verify the installation:

```bash
uv --version
```

### 2. Clone the repository

```bash
git clone <your-repo-url>
cd mcp1_leave_management
```

### 3. Install Python 3.14

`uv` will read the `.python-version` file and auto-install the correct Python version:

```bash
uv python install
```

### 4. Create a virtual environment

```bash
uv venv
```

### 5. Activate the virtual environment

```bash
# macOS / Linux
source .venv/bin/activate

# Windows
.venv\Scripts\activate
```

### 6. Install the `mcp` dependency

```bash
uv add mcp
```

### 7. Verify the installation

```bash
python -c "from mcp.server.fastmcp import FastMCP; print('MCP installed successfully')"
```

---

## Running the Server

### Standalone (for testing)

```bash
python main.py
```

### With MCP Inspector (interactive testing)

```bash
npx @modelcontextprotocol/inspector python main.py
```

Then open `http://localhost:5173` in your browser to interact with the tools.

---

## Connecting to Claude Desktop

### 1. Find your Claude Desktop config file

| OS      | Path                                                                 |
|---------|----------------------------------------------------------------------|
| macOS   | `~/Library/Application Support/Claude/claude_desktop_config.json`   |
| Windows | `%APPDATA%\Claude\claude_desktop_config.json`                        |

### 2. Add the server configuration

Open the config file and add an entry under `mcpServers`:

```json
{
  "mcpServers": {
    "leave-management": {
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "/absolute/path/to/mcp1_leave_management",
        "python",
        "main.py"
      ]
    }
  }
}
```

Replace `/absolute/path/to/mcp1_leave_management` with the actual path on your machine.

### 3. Restart Claude Desktop

Quit and relaunch Claude Desktop. The leave management tools will appear in the tool panel.

---

## Available Tools

| Tool | Description | Parameters |
|------|-------------|------------|
| `get_leave_balance` | Returns remaining leave days | `employee_id: str` |
| `apply_leave` | Applies leave for given dates | `employee_id: str`, `leave_dates: List[str]` |
| `get_leave_history` | Returns past leave dates | `employee_id: str` |

### Example usage (in Claude)

```
Check leave balance for employee E001
Apply leave for E002 on 2025-06-10 and 2025-06-11
Show leave history for E001
```

---

## Mock Data

The server starts with two pre-seeded employees:

| Employee ID | Initial Balance | Pre-existing Leaves |
|-------------|-----------------|---------------------|
| E001        | 18 days         | 2024-12-25, 2025-01-01 |
| E002        | 20 days         | None |

Data is held in memory and resets each time the server restarts.

---

## Project Structure

```
mcp1_leave_management/
├── main.py           # MCP server and tool definitions
├── pyproject.toml    # Project metadata and dependencies
├── uv.lock           # Locked dependency versions
├── .python-version   # Pinned Python version (3.14)
└── README.md
```

## Terminal setup steps from zero
uv venv --python 3.14 .venv
source .venv/bin/activate
which python
python --version
uv pip install mcp
uv
uv init
uv pip show mcp
uv run mcp install main.py