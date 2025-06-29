# MCP_Resources

## MCP Server Configuration

Add the following configuration to your Claude Desktop app:

```json
{
  "mcpServers": {
    "brave-search": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "BRAVE_API_KEY",
        "mcp/brave-search"
      ],
      "env": {
        "BRAVE_API_KEY": "your_token_here"
      }
    },
    "github": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "-e", "GITHUB_PERSONAL_ACCESS_TOKEN", "ghcr.io/github/github-mcp-server"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your_token_here"
      }
    },
    "context7": {
      "autoApprove": [],
      "disabled": false,
      "timeout": 60,
      "command": "docker",
      "args": ["run", "-i", "--rm", "context7-mcp"],
      "transportType": "stdio"
    },
    "serena": {
      "command": "uvx",
      "args": ["--from", "git+https://github.com/oraios/serena", "serena-mcp-server.exe"]
    }
  }
}
```

Add to Claude Desktop to test out.

## Brave Search API Key

To get a Brave Search API key:

1. Visit [Brave Search API](https://brave.com/search/api/)
2. Sign up for an account
3. Navigate to your dashboard
4. Generate a new API key
5. Copy the API key and replace `your_token_here` in the configuration above

## GitHub Personal Access Token

To get a GitHub Personal Access Token:

1. Go to GitHub > Settings > Developer Settings > Personal Access Tokens
2. Click "Generate new token"
3. Add required scopes (repos and projects)
4. Generate the token
5. Copy the token and replace `your_token_here` in the configuration above

## Context7 Setup

To run the Context7 MCP server in a Docker container:

1. Create a Dockerfile in your project root:

```dockerfile
FROM node:18-alpine

WORKDIR /app

# Install the latest version globally
RUN npm install -g @upstash/context7-mcp

# Expose default port if needed (optional, depends on MCP client interaction)
# EXPOSE 3000

# Default command to run the server
CMD ["context7-mcp"]
```

2. Build the Docker image (make sure Docker Desktop or Docker daemon is running):

```bash
docker build -t context7-mcp .
```

## Serena Setup

**Note:** The Serena MCP server uses `serena-mcp-server.exe` on Windows and `serena-mcp-server` (without .exe) on macOS/Linux. Update the configuration accordingly based on your operating system.

## Claude Code Integration

To add these servers to Claude Code, run:

```bash
claude mcp add-from-claude-desktop
```