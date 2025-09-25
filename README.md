# Instructions for setting up Docker Toolkit MCP gateway server and containerizing .NET applications for use as docker MCP servers

### Clone sample repo  
```
git clone https://github.com/timsonner/dotnet-mcp-skeleton-project-vscode.git
cd dotnet-mcp-skeleton-project-vscode
```

# Containerize .NET app

### Initialize Docker
```
docker init
```

For demo, accept defaults for port and project solution, use 9.0 for .NET version

### Build container  
```
docker compose up --build
```

# Configure Docker MCP Gateway server

### Connect VS Code as client
```
docker mcp client connect vscode
```

### Create mcp.json MCP configuration file
```
mkdir ./vscode
touch ./vscode/mcp.json
```

### Verify docker image name (used in mcp.json)
```
docker images | grep dotnet-mcp-skeleton
```

### Modify mcp.json  
mcp.json
```
{
  "servers": {
    "MCP_DOCKER": {
      "command": "docker",
      "args": ["mcp", "gateway", "run"],
      "type": "stdio"
    },
    "dotnet-mcp-skeleton": {
      "command": "docker",
      "args": ["run", "--rm", "-i", "dotnet-mcp-skeleton-project-vscode-server"],
      "type": "stdio"
    }
  }
}
```

### Next steps
- Hit play button next to `MCP_DOCKER` MCP gateway server and `dotnet-mcp-skeleton` MCP server
- Ask Copilot to generate character

### References  
```
https://docs.docker.com/guides/dotnet/containerize/
https://docs.docker.com/ai/mcp-gateway/
```
