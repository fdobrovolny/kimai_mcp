# Kimai MCP Server

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![MCP Compatible](https://img.shields.io/badge/MCP-Compatible-green.svg)](https://modelcontextprotocol.io/)

A comprehensive Model Context Protocol (MCP) server for integrating with the Kimai time-tracking API. This server allows AI assistants like Claude to efficiently interact with Kimai instances to manage time tracking, projects, activities, customers, users, teams, absences, and more.

## 🚀 Quick Start

```bash
# Install the package
pip install -e .

# Run with your Kimai credentials
python -m kimai_mcp.server --kimai-url=https://your-kimai.com --kimai-token=your-token
```

## 🛠️ Available Tools

### Core Management Tools
1. **Entity Tool** - Universal CRUD operations for projects, activities, customers, users, teams, tags, invoices, holidays
2. **Timesheet Tool** - Complete timesheet management (list, create, update, delete, export)
3. **Timer Tool** - Active timer operations (start, stop, restart, view active/recent)
4. **Rate Tool** - Rate management across all entity types
5. **Team Access Tool** - Team member and permission management
6. **Absence Tool** - Complete absence workflow (create, approve, reject, list)
7. **Calendar Tool** - Unified calendar data access
8. **Meta Tool** - Custom field management across entities
9. **User Current Tool** - Current user information
10. **Project Analysis Tool** - Advanced project analytics

### Complete Kimai Integration
- **Timesheet Management** - Create, update, delete, start/stop timers, view active timers
- **Project & Activity Management** - Browse and view projects and activities
- **Customer Management** - Browse and view customer information
- **User Management** - List, view, create, and update user accounts
- **Team Management** - Create teams, manage members, control access permissions
- **Absence Management** - Create, approve, reject, and track absences
- **Tag Management** - Create and manage tags for better organization
- **Invoice Queries** - View invoice information and status

### Advanced Features
- **Real-time Timer Control** - Start, stop, and monitor active time tracking
- **Comprehensive Filtering** - Advanced filters for all data types
- **Permission Management** - Respect Kimai's role-based permissions
- **Error Handling** - Proper error handling with meaningful messages
- **Flexible Configuration** - Multiple configuration methods (CLI args, .env files, environment variables)

## Installation

### Prerequisites
- Python 3.8+
- A Kimai instance with API access enabled
- API token from your Kimai user profile

### Install the Package

```bash
# Clone the repository
git clone https://github.com/glazperle/kimai_mcp.git
cd kimai_mcp

# Install the package
pip install -e .
```

### Alternative: Install Dependencies Only
```bash
pip install mcp httpx pydantic python-dotenv
```

## Configuration

### Getting Your Kimai API Token

1. Log into your Kimai instance
2. Go to your user profile (click your username)
3. Navigate to the "API" or "API Access" section
4. Create a new API token or copy an existing one
5. Note your Kimai instance URL (e.g., `https://kimai.example.com`)

## Claude Desktop Integration

### Step 1: Configure Claude Desktop

Add the Kimai MCP server to your Claude Desktop configuration file:

**On macOS:**
`~/Library/Application Support/Claude/claude_desktop_config.json`

**On Windows:**
`%APPDATA%\Claude\claude_desktop_config.json`

### Step 2: Add Configuration

Add the following to your Claude Desktop configuration:

```json
{
  "mcpServers": {
    "kimai": {
      "command": "python",
      "args": [
        "-m", "kimai_mcp.server",
        "--kimai-url=https://your-kimai-instance.com",
        "--kimai-token=your-api-token-here"
      ]
    }
  }
}
```

**Important Notes:**
- Replace `https://your-kimai-instance.com` with your actual Kimai URL
- Replace `your-api-token-here` with your API token from Kimai
- Optionally add `--kimai-user=USER_ID` for a default user ID

### Step 3: Restart Claude Desktop

After saving the configuration file, restart Claude Desktop for the changes to take effect.

### Alternative Configuration Methods

#### Method 1: Using a .env File (Recommended for Development)
If you prefer using a .env file for configuration, create a `.env` file in your project directory:

```bash
# .env file in the kimai_mcp directory
KIMAI_URL=https://your-kimai-instance.com
KIMAI_API_TOKEN=your-api-token-here
KIMAI_DEFAULT_USER=1
```

Then use this Claude Desktop configuration:
```json
{
  "mcpServers": {
    "kimai": {
      "command": "python",
      "args": ["-m", "kimai_mcp.server"],
      "cwd": "/path/to/your/kimai_mcp/directory"
    }
  }
}
```

**Important Notes for .env Configuration:**
- Replace `/path/to/your/kimai_mcp/directory` with the actual path to your kimai_mcp directory
- The `cwd` parameter ensures the .env file is found in the correct directory
- Keep your .env file secure and never commit it to version control
- On Windows, use forward slashes in the path or escape backslashes

**Example Windows Path:**
```json
{
  "mcpServers": {
    "kimai": {
      "command": "python",
      "args": ["-m", "kimai_mcp.server"],
      "cwd": "C:/Users/YourName/Projects/kimai_mcp"
    }
  }
}
```

#### Method 2: Using Environment Variables (System-wide)
If you prefer system environment variables, you can set:
```bash
export KIMAI_URL="https://your-kimai-instance.com"
export KIMAI_API_TOKEN="your-api-token-here"
export KIMAI_DEFAULT_USER="1"  # Optional
```

Then use this Claude Desktop configuration:
```json
{
  "mcpServers": {
    "kimai": {
      "command": "python",
      "args": ["-m", "kimai_mcp.server"]
    }
  }
}
```

## Usage Examples

### Timesheet Management

#### List Timesheets
```json
{
  "tool": "timesheet",
  "parameters": {
    "action": "list",
    "filters": {
      "project": 17,
      "user_scope": "self"
    }
  }
}
```

#### Create a Timesheet Entry
```json
{
  "tool": "timesheet",
  "parameters": {
    "action": "create",
    "data": {
      "project": 1,
      "activity": 5,
      "description": "Working on API integration",
      "begin": "2024-08-03T09:00:00",
      "end": "2024-08-03T10:30:00"
    }
  }
}
```

#### Start a Timer
```json
{
  "tool": "timer",
  "parameters": {
    "action": "start",
    "data": {
      "project": 1,
      "activity": 5,
      "description": "Working on API integration"
    }
  }
}
```

#### Stop a Timer
```json
{
  "tool": "timer",
  "parameters": {
    "action": "stop",
    "id": 12345
  }
}
```

### Project & Activity Management

#### List Projects
```json
{
  "tool": "entity",
  "parameters": {
    "type": "project",
    "action": "list",
    "filters": {"customer": 1}
  }
}
```

#### Get Project Details
```json
{
  "tool": "entity",
  "parameters": {
    "type": "project",
    "action": "get",
    "id": 17
  }
}
```

#### List Activities
```json
{
  "tool": "entity",
  "parameters": {
    "type": "activity",
    "action": "list",
    "filters": {"project": 17}
  }
}
```

### User & Team Management

#### List Users
```json
{
  "tool": "entity",
  "parameters": {
    "type": "user",
    "action": "list"
  }
}
```

#### Create a Team
```json
{
  "tool": "entity",
  "parameters": {
    "type": "team",
    "action": "create",
    "data": {
      "name": "Development Team",
      "color": "#3498db"
    }
  }
}
```

#### Add Team Member
```json
{
  "tool": "team_access",
  "parameters": {
    "action": "add_member",
    "team_id": 1,
    "user_id": 5
  }
}
```

### Absence Management

#### Create an Absence
```json
{
  "tool": "absence",
  "parameters": {
    "action": "create",
    "data": {
      "comment": "Vacation in the mountains",
      "date": "2024-02-15",
      "end": "2024-02-20",
      "type": "holiday"
    }
  }
}
```

#### List Absences
```json
{
  "tool": "absence",
  "parameters": {
    "action": "list",
    "filters": {
      "user": "5",
      "status": "all"
    }
  }
}
```

### Rate Management

#### List Customer Rates
```json
{
  "tool": "rate",
  "parameters": {
    "entity": "customer",
    "action": "list",
    "entity_id": 1
  }
}
```

### Current User Information

#### Get Current User
```json
{
  "tool": "user_current"
}
```

## Troubleshooting

### Common Issues

#### Connection Problems
1. **Verify Kimai URL**: Ensure your Kimai URL is correct and accessible
2. **Check API Token**: Verify your API token is valid and not expired
3. **API Access**: Ensure your Kimai instance has API access enabled
4. **Network**: Check if there are any firewall or network restrictions

#### Permission Errors
- Creating timesheets for other users requires admin permissions
- Managing users and teams requires appropriate role permissions
- Some absence operations require manager permissions

#### Configuration Issues
1. **Claude Desktop Config**: Verify the JSON syntax is correct
2. **Path Issues**: Ensure Python can find the `kimai_mcp` module
3. **Arguments**: Check that command-line arguments are properly formatted

### Debug Mode
For debugging, you can run the server directly:

```bash
# Using command line arguments
python -m kimai_mcp.server --kimai-url=https://your-kimai.com --kimai-token=your-token

# Using .env file (make sure you're in the directory with the .env file)
python -m kimai_mcp.server

# Test the package module execution
python -m kimai_mcp.server --help
```

### Logging
The server includes comprehensive logging. Check the logs for detailed error information.

## Security Considerations

- **API Token Security**: Keep your API token secure and never commit it to version control
- **Network Security**: Use HTTPS for your Kimai instance
- **Permission Management**: Use appropriate Kimai roles and permissions
- **Regular Updates**: Keep the MCP server and dependencies updated

## Development

### Project Structure
```
kimai_mcp/
├── src/
│   ├── kimai_mcp/
│   │   ├── __init__.py
│   │   ├── server.py         # MCP server implementation
│   │   ├── client.py         # Kimai API client
│   │   ├── models.py         # Data models
│   │   └── tools/            # MCP tool implementations
│   │       ├── entity_manager.py
│   │       ├── timesheet_consolidated.py
│   │       ├── rate_manager.py
│   │       ├── team_access_manager.py
│   │       ├── absence_manager.py
│   │       ├── calendar_meta.py
│   │       └── project_analysis.py
├── tests/
├── README.md
├── pyproject.toml
└── .gitignore
```

### Running Tests
```bash
pytest tests/ -v
```

### Contributing
1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new functionality
5. Submit a pull request

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### Licensing Information

- **Kimai MCP Server**: MIT License (this project)
- **Kimai Core**: AGPL-3.0 License (separate project)
- **Model Context Protocol**: Open standard by Anthropic

This MCP server is an independent integration tool that communicates with Kimai via its public API. It is not a derivative work of Kimai itself and can be freely used under the MIT license terms.

## 🤝 Contributing

We welcome contributions! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

### Development Setup

1. Clone the repository
2. Install development dependencies: `pip install -e ".[dev]"`
3. Run tests: `pytest tests/ -v`
4. Follow the existing code style and add tests for new features

## 📞 Support

- **Issues**: Please use the [GitHub issue tracker](https://github.com/glazperle/kimai_mcp/issues)
- **Documentation**: Check the examples in the `examples/` directory
- **Kimai Documentation**: Visit [kimai.org](https://www.kimai.org/) for Kimai-specific questions

## 🙏 Acknowledgments

- **Anthropic** for creating the Model Context Protocol
- **Kimai Team** for the excellent time-tracking software and API
- **MCP Community** for examples and best practices