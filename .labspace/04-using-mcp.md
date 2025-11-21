# Step 4: MCP (Model Context Protocol)

[MCP](https://modelcontextprotocol.io/) is an open-source standard for
connecting AI applications to external systems.

MCP is a standardized protocol that gives agents access to different MCP servers containing tools (and much more - we encourage you to read the specification).


## Pre-configured MCP Gateway

In this Labspace, we've pre-configured an MCP Gateway with the following servers:

- **fetch** - Fetch and read web pages
- **duckduckgo** - Search the web
- **context7** - Access library documentation

Let's verify the MCP Gateway is running:

```bash
curl http://mcp-gateway:8080/health
```

## Using the Fetch MCP Server

Create an agent that can fetch and summarize web pages:

```bash
cat > fetch_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: Summarize URLs for users. Use the fetch tool to retrieve web pages.
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

Ask it: `fetch docker.com and summarize what Docker does`

You will see the agent calling the `fetch` tool from the MCP server!

## Using the DuckDuckGo MCP Server

The same gateway provides web search capabilities:

```bash
cat > search_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: |
      You are a research assistant. 
      Use the duckduckgo search tool to find information.
      Always cite your sources.
    toolsets:
      - type: mcp
        remote:
          url: http://mcp-gateway:8080
          transport_type: sse
EOF
```

Run the agent:

```bash
cagent run search_agent.yaml
```

Ask it: `Search for the latest news about Docker containers`

## Using Context7 for Documentation

Context7 provides up-to-date documentation for libraries and frameworks:

```bash
cat > docs_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: |
      You are a documentation expert.
      Use context7 to find accurate, up-to-date documentation for any library.
    toolsets:
      - type: mcp
        remote:
          url: http://mcp-gateway:8080
          transport_type: sse
EOF
```

Run it:

```bash
cagent run docs_agent.yaml
```

Ask it: `How do I set up a basic Express.js server? Use context7 to get the latest documentation.`


## Enhancing Our Developer Agent

Let's combine everything we've learned to create a powerful developer agent:

```bash
cat > developer.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: |
      You are an amazing developer.
      Use available tools to:
      - Search for information (duckduckgo)
      - Fetch documentation (fetch, context7)
      - Read and write files (filesystem)
      - Run commands (shell)
      - Track your tasks (todo)
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
Create a directory "server" and write a Node.js Express server with TypeScript. 
The server should be a key-value store with GET, PUT, and DELETE endpoints.
Use context7 to get the latest Express.js documentation.
```

The agent will:
1. Use context7 to fetch Express.js documentation
2. Create the directory structure
3. Write the TypeScript server code
4. Set up the necessary configuration files

## Using DMR with MCP (No API Key Required!)

You can combine Docker Model Runner with MCP for a completely local setup:

```bash
cat > local_developer.yaml << 'EOF'
version: "2"

agents:
  root:
    model: dmr/ai/smollm2
    instruction: You are a helpful developer assistant
    add_environment_info: true
    model_config:
      base_url: http://model-runner:12434/engines/llama.cpp/v1
    toolsets:
      - type: filesystem
      - type: shell
      - type: mcp
        remote:
          url: http://mcp-gateway:8080
          transport_type: sse
EOF
```

Run it:

```bash
cagent run local_developer.yaml
```

Now you have a fully local AI agent with web access and documentation lookup!

## Troubleshooting

If MCP servers aren't working:

1. **Check MCP Gateway status**:
   ```bash
   curl http://mcp-gateway:8080/health
   ```

2. **Check available tools**:
   ```bash
   curl http://mcp-gateway:8080/tools
   ```

3. **Test fetch directly**:
   ```bash
   curl -X POST http://mcp-gateway:8080/fetch \
     -H "Content-Type: application/json" \
     -d '{"url": "https://example.com"}'
   ```

## Next Steps

In Step 5, we'll learn how to share your agents with others using Docker Registry.
