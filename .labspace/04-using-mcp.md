# Step 4: MCP (Model Context Protocol)

For this section you will need [Docker
Desktop](https://www.docker.com/products/docker-desktop/) installed.

[MCP](https://modelcontextprotocol.io/) is an open-source standard for
connecting AI applications to external systems.

MCP is a standardised protocol clients can use to give an agent access to
different MCP servers that contain tools (and much more, we encourage you to go
and read the specification).

The Docker MCP Toolkit is a gateway that lets you set up, manage, and run
containerized MCP servers and connect them to AI agents.

![MCP Toolkit](mcp.png)

Naturally, `cagent` makes it easy to use an MCP server from the MCP Toolkit.

Here is an example agent that uses the `fetch` MCP server from the toolkit. Create the file:

```bash
cat > fetch_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: Summarize URLs for users
    toolsets:
      - type: mcp
        remote:
          url: http://mcp-gateway:8080
          transport_type: sse
EOF
```

Run this agent:

```bash
cagent run fetch_agent.yaml
```

Ask it: `fetch rumpl.dev`

You will see the agent calling the `fetch` tool from the `fetch` MCP server.

This is the simplest way to use an MCP server with cagent, but you can also run
_any_ MCP server, local or remote.

## Custom Local MCP Server

To run the following example you will need the
[uvx](https://docs.astral.sh/uv/getting-started/installation/) command line,
which is often needed when running local MCP servers outside of docker
containers.

`yfmcp` is the Yahoo Finance MCP server. Create the agent:

```bash
cat > yahoo_finance_agent.yaml << 'EOF'
version: "2"
agents:
  root:
    model: openai/gpt-4o
    add_date: true
    instruction: |
      Analyse the given stock
      Use tables to display the data
    toolsets:
      - type: mcp
        command: uvx
        args: ["yfmcp"]
EOF
```

Run the agent:

```bash
cagent run yahoo_finance_agent.yaml
```

## Remote MCP Server

You can also connect to remote MCP servers. Create this agent:

```bash
cat > moby_expert.yaml << 'EOF'
version: "2"

agents:
  root:
    model: anthropic/claude-3-5-sonnet-latest
    description: Moby Project Expert
    instruction: You are an AI assistant with expertise in the moby/moby project's documentation.
    toolsets:
      - type: mcp
        remote:
          url: https://gitmcp.io/moby/moby
          transport_type: streamable
EOF
```

Run the agent:

```bash
cagent run moby_expert.yaml
```

## Your Own MCP Server

In this exercise we will take an existing (very simple) MCP server, build it,
and then use it in our agent.

First, let's clone the repository and build the Docker image:

```bash
git clone https://github.com/rumpl/mcp-strawberry
cd mcp-strawberry
docker build -t mcp-strawberry .
cd ..
```

> **Note:** If you don't want to clone and build, you can use the already pushed MCP server image from Docker Hub: `djordjelukic1639080/mcp-strawberry`

Now create an agent that will use this MCP server:

```bash
cat > strawberry_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: Count the number of 'r' in a word
    toolsets:
      - type: mcp
        command: docker
        args: ["run", "-i", "--rm", "mcp-strawberry"]
EOF
```

Run the agent:

```bash
cagent run strawberry_agent.yaml
```

Ask it: `How many 'r's are in the word strawberry?`

## Enhancing Our Developer Agent

Let's use the power of MCP servers to make our developer agent even more powerful.

A good developer always reads the documentation (right?). So let's give
our developer the capability to fetch up-to-date documentation for pretty much
any library out there. For this we will use the
[context7](https://context7.com/) MCP server. Luckily for us, this server is
already present in Docker's MCP Toolkit.

Update your `developer.yaml`:

```bash
cat > developer.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: You are an amazing developer
    add_environment_info: true
    add_date: true
    toolsets:
      - type: todo
      - type: shell
      - type: filesystem
      - type: mcp
        ref: docker:context7
EOF
```

Run this agent:

```bash
cagent run developer.yaml
```

Ask this:

```
Create a directory "server" and in there write a server in node and typescript. The server should be a key-value store. Use context7 to get the documentation you need for the server library you will use.
```

Running this, the agent should use the context7 tools to find the appropriate
documentation for the library it chose to use to write this server.

Look around the MCP Toolkit, play with some more MCP servers. What other servers
do you think could be useful for a developer agent?

## Next Steps

In Step 5, we'll learn how to share your agents with others using Docker Registry.
