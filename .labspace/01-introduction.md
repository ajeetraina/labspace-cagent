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

## Prerequisites

Before starting this workshop, you should have:

1. **cagent installed** - Follow the installation instructions below
2. **Docker Desktop running** - Required for Step 3 onwards (MCP and agent sharing)
3. **Docker Hub account** - For pushing and pulling shared agents
4. **AI API access** - OpenAI, Anthropic, or other supported model providers

## Step 1: Install cagent on Linux

Since you're running this lab on Ubuntu Linux, let's install cagent directly. Run the following commands:

```bash
# Download the latest cagent Linux binary
curl -L -o /tmp/cagent https://github.com/docker/cagent/releases/latest/download/cagent-linux-amd64

# Make it executable
chmod +x /tmp/cagent

# Move it to a location in your PATH
sudo mv /tmp/cagent /usr/local/bin/cagent

# Verify installation
cagent --version
```

You should see output showing the cagent version. If you see "command not found", make sure `/usr/local/bin` is in your PATH:

```bash
echo $PATH
```

If needed, add it to your PATH:

```bash
export PATH="/usr/local/bin:$PATH"
```


## Step 2: Set Your API Keys

You'll configure your API keys in the next step of this workshop. Based on the models you configure your agents to use, you will need to set the corresponding provider API key. You will need at least one of these:

- **OpenAI** - For GPT-4, GPT-4o, and other OpenAI models
- **Anthropic** - For Claude models (Claude 3.5 Sonnet, etc.)
- **Google** - For Gemini models
- **Docker Model Runner (DMR)** - For local models (no API key needed)

## Workshop Overview

In this workshop we will learn the basics of creating agents and teams of
agents. How to run them and how to share them.

While we learn how to use cagent we will focus on making a coding agent.

By the end of this workshop you should have everything you need to make your own
agents that do things for you.

## What You'll Learn

- **Step 2**: Getting started with your first agent and setting up API keys
- **Step 3**: Using built-in tools for file operations and web browsing
- **Step 4**: Integrating external services with Model Context Protocol (MCP)
- **Step 5**: Sharing agents with your team via Docker Hub
- **Step 6**: Building multi-agent teams with sub-agents
- **Step 7**: Using Docker Model Runner for local AI models
- **Step 8**: Next steps and advanced topics

Let's get started!
