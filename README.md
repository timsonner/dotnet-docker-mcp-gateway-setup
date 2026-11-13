# Docker MCP gateway setup for a .NET MCP server

This repository is a setup guide. It shows how to containerize the sample .NET Model Context Protocol (MCP) server and register that container, along with the Docker MCP gateway, as stdio servers in VS Code.

The application is not in this repository. The steps clone [dotnet-mcp-skeleton-project-vscode](https://github.com/timsonner/dotnet-mcp-skeleton-project-vscode). That sample is a .NET 9 console app (`TargetFramework` is `net9.0`). `Program.cs` starts an MCP server on stdio and registers the `CharacterCreator` tool. `GetRandomCharacter` returns a random class and six ability scores. When `PLAYER_CLASS` is unset, the tool uses the class list written in `Tools/CharacterCreator.cs`.

Follow this guide when VS Code should start that server with `docker run` and should start the Docker MCP gateway with `docker mcp gateway run`.

## Prerequisites

- Git
- Docker, including Compose, `docker init`, and the `docker mcp` command
- VS Code

Nothing in this repository is installed or built. It has no package manifest, Dockerfile, Compose file, or test project. When `docker init` asks for a .NET version, choose 9.0, matching the sample project. The steps below do not use a .NET SDK on the host.

## Install, build, run, and test

Clone the sample and change to it:

```bash
git clone https://github.com/timsonner/dotnet-mcp-skeleton-project-vscode.git
cd dotnet-mcp-skeleton-project-vscode
```

Create the container files. This command prompts for answers. Accept the defaults for the port and the project or solution, and choose .NET 9.0:

```bash
docker init
```

Build the image and start the Compose stack:

```bash
docker compose up --build
```

Check the image name. The MCP config below uses `dotnet-mcp-skeleton-project-vscode-server`. If the command prints a different name, use that name in the `docker run` arguments:

```bash
docker images | grep dotnet-mcp-skeleton
```

This repository has no test script, and the sample repository has no test project, so there is no test command.

## Usage

Connect VS Code as a Docker MCP client:

```bash
docker mcp client connect vscode
```

From the sample directory, write the workspace MCP config. VS Code reads `.vscode/mcp.json`:

```bash
mkdir -p .vscode
cat > .vscode/mcp.json <<'EOF'
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
EOF
```

Open the sample folder in VS Code. Start `MCP_DOCKER` and `dotnet-mcp-skeleton` with the play control next to each server. Ask Copilot to generate a character. The server runs `GetRandomCharacter`.

## Layout

This repository contains only:

```text
README.md
```

The cloned sample contains:

```text
Program.cs
dotnet-mcp-skeleton-project-vscode.csproj
Tools/CharacterCreator.cs
README.md
.gitignore
```

`docker init` adds the container files it generates in that sample directory. Those files are not stored here.

## License

This repository has no license file.

## References

- https://docs.docker.com/guides/dotnet/containerize/
- https://docs.docker.com/ai/mcp-gateway/
