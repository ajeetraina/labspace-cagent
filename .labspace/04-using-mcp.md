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

Ask it: `fetch docker.com`

You will see the agent calling the `fetch` tool from the `fetch` MCP server.

This is the simplest way to use an MCP server with cagent, but you can also run
_any_ MCP server, local or remote.


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
        remote:
          url: http://mcp-gateway:8080
          transport_type: sse
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
