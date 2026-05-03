# Semaphore MCP Server

![Semaphore MCP Server](semaphore-mcp-server.svg?raw=true)

A Model Context Protocol (MCP) server for [Semaphore UI](https://semaphoreui.com/) - the modern open-source alternative to Ansible Tower/AWX.

This server enables AI assistants and MCP-compatible tools to interact with Semaphore UI for managing Ansible, Terraform, and other automation workflows.

## Features

- **Project Management** — List, create, and delete projects
- **Task Management** — Launch, monitor, stop, and retrieve output of tasks
- **Template Management** — CRUD operations on job templates
- **Inventory Management** — Manage Ansible inventories
- **Repository Management** — Manage Git repositories
- **Environment Management** — Manage environment variable sets
- **Key Store** — List and manage SSH keys and credentials
- **Schedule Management** — Create and manage cron-based schedules
- **User & Events** — Get user info and project event logs
- **Server Info** — Check server health and version

## Installation

```bash
pip install -e .
```

Or with the semaphore client from a local path:

```bash
pip install -e /path/to/python3-semaphore-client
pip install -e .
```

## Configuration

Copy `.env.example` to `.env` and configure:

```bash
cp .env.example .env
```

Required environment variables:

| Variable | Description |
|----------|-------------|
| `SEMAPHORE_URL` | Base URL of your Semaphore UI instance (e.g., `http://localhost:3000`) |
| `SEMAPHORE_TOKEN` | API Bearer token (create via Semaphore UI or API) |

Optional environment variables:

| Variable | Default | Description |
|----------|---------|-------------|
| `READ_ONLY` | `true` | Set to `false` to enable write operations |
| `DEBUG` | _(unset)_ | Set to `true` for verbose logging |

### Creating an API Token

1. **Via Web UI** (Semaphore 2.14+): Go to your user settings and create a token
2. **Via API**:
   ```bash
   # Login
   curl -c /tmp/semaphore-cookie -XPOST \
     -H 'Content-Type: application/json' \
     -d '{"auth": "YOUR_LOGIN", "password": "YOUR_PASSWORD"}' \
     http://localhost:3000/api/auth/login

   # Generate token
   curl -b /tmp/semaphore-cookie -XPOST \
     http://localhost:3000/api/user/tokens
   ```

## Usage

### Running the MCP Server

```bash
semaphore-mcp
```

### MCP Configuration

Add to your MCP client configuration (e.g., VS Code `mcp.json`):

```json
{
  "servers": {
    "semaphore": {
      "command": "semaphore-mcp",
      "env": {
        "SEMAPHORE_URL": "http://localhost:3000",
        "SEMAPHORE_TOKEN": "your-token-here",
        "READ_ONLY": "false"
      }
    }
  }
}
```

Or for Claude Desktop (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "semaphore": {
      "command": "semaphore-mcp",
      "env": {
        "SEMAPHORE_URL": "http://localhost:3000",
        "SEMAPHORE_TOKEN": "your-token-here"
      }
    }
  }
}
```

## Available Tools

| Tool | Description |
|------|-------------|
| `project_list` | List all projects |
| `project_get` | Get project details |
| `project_create` | Create a new project |
| `project_delete` | Delete a project |
| `task_list` | List tasks in a project |
| `task_get` | Get task details |
| `task_launch` | Launch a task from a template |
| `task_stop` | Stop a running task |
| `task_output` | Get task output/log |
| `task_delete` | Delete a task |
| `template_list` | List templates in a project |
| `template_get` | Get template details |
| `template_create` | Create a new template |
| `template_delete` | Delete a template |
| `inventory_list` | List inventories in a project |
| `inventory_get` | Get inventory details |
| `inventory_create` | Create a new inventory |
| `inventory_delete` | Delete an inventory |
| `repository_list` | List repositories in a project |
| `repository_get` | Get repository details |
| `repository_create` | Create a new repository |
| `repository_delete` | Delete a repository |
| `environment_list` | List environments in a project |
| `environment_get` | Get environment details |
| `environment_create` | Create a new environment |
| `environment_delete` | Delete an environment |
| `key_list` | List keys/credentials in a project |
| `key_get` | Get key details |
| `key_delete` | Delete a key |
| `schedule_list` | List schedules in a project |
| `schedule_create` | Create a new schedule |
| `schedule_delete` | Delete a schedule |
| `user_get_current` | Get current user info |
| `user_tokens` | List user API tokens |
| `event_list` | List project events |
| `server_info` | Get server version/info |
| `server_ping` | Check server connectivity |

## Dependencies

This project uses [python3-semaphore-client](https://github.com/VitexSoftware/python3-semaphore-client) — an OpenAPI-generated Python client library for the Semaphore UI API (v2.16.14).

## Development

```bash
# Install in development mode
pip install -e ".[dev]"

# Run directly
python -m src.semaphore_mcp_server
```

## License

MIT
