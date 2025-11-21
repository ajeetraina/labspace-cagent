# Introduction

# Building Agents with cagent - Workshop

Welcome to this hands-on workshop on building intelligent agents using cagent!
This workshop will take you from basic agent concepts to building sophisticated
multi-agent teams that can handle complex real-world tasks.

## What is cagent?

cagent is a powerful framework for building and orchestrating AI agents. Unlike
simple chatbots, cagent agents can:

- **Take Actions**: Use tools to interact with files, run commands, browse the
  web, and more
- **Work in Teams**: Coordinate with specialized sub-agents for complex projects
- **Remember Context**: Maintain persistent memory across conversations
- **Think and Plan**: Use structured reasoning to solve complex problems
- **Integrate Anywhere**: Connect to external services via the Model Context
  Protocol (MCP)

## What's Pre-Configured in This Labspace

This Labspace comes with everything you need to get started:

| Service | URL | Description |
|---------|-----|-------------|
| **Docker Model Runner** | `http://model-runner:12434` | Run local AI models (no API key needed!) |
| **MCP Gateway** | `http://mcp-gateway:8080` | Pre-configured with fetch, duckduckgo, and context7 servers |

> **No API Key? No Problem!** You can complete this entire workshop using Docker Model Runner with local models. See Step 7 for details.

## Prerequisites

Before starting this workshop, you should have:

1. **cagent installed** - Follow the installation instructions below
2. **AI API access** - Choose one of:
   - OpenAI API key (for GPT-4, GPT-4o)
   - Anthropic API key (for Claude models)
   - Google API key (for Gemini models)
   - **OR** use Docker Model Runner (no API key needed!)

## Step 1: Install cagent

Since you're running this lab on Ubuntu Linux, let's install cagent directly. Run the following commands:

```bash
# Download the latest cagent Linux binary
curl -L -o /tmp/cagent https://github.com/docker/cagent/releases/latest/download/cagent-linux-amd64

# Make it executable
chmod +x /tmp/cagent

# Move it to a location in your PATH
sudo mv /tmp/cagent /usr/local/bin/cagent

# Verify installation
cagent version
```

You should see output showing the cagent version.

## Step 2: Verify Pre-Configured Services

Let's make sure the Labspace services are running:

```bash
# Check Docker Model Runner
curl http://model-runner:12434/models
```

```bash
# Check MCP Gateway
curl http://mcp-gateway:8080/health
```

If both commands return successfully, you're ready to go!

## Step 3: Pull a Local Model (Optional but Recommended)

If you want to use local models without API keys, pull a model now:

```bash
curl -X POST http://model-runner:12434/models/create \
  -H "Content-Type: application/json" \
  -d '{"model": "ai/smollm2"}'
```

This may take a few minutes to download.

## Workshop Overview

In this workshop we will learn the basics of creating agents and teams of
agents. How to run them and how to share them.

While we learn how to use cagent we will focus on making a coding agent.

By the end of this workshop you should have everything you need to make your own
agents that do things for you.

## What You'll Learn

| Step | Topic | Description |
|------|-------|-------------|
| 2 | Getting Started | Create your first agent and set up API keys |
| 3 | Built-in Tools | File operations, shell commands, memory, and todo tracking |
| 4 | MCP Integration | Connect to external services (fetch, search, documentation) |
| 5 | Sharing Agents | Push and pull agents via Docker Hub |
| 6 | Sub-agents | Build multi-agent teams with specialized roles |
| 7 | Model Runner | Run local AI models without API keys |
| 8 | Conclusion | Next steps and advanced topics |

Let's get started!
