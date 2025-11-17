# Step 7: Using Docker Model Runner (DMR)

Docker Model Runner (DMR) makes it easy to manage, run, and deploy AI models using Docker. Designed for developers,
Docker Model Runner streamlines the process of pulling, running, and serving large language models (LLMs) and other AI
models directly from Docker Hub or any OCI-compliant registry.

Discover curated models on [Docker Hub](https://hub.docker.com/u/ai).

Naturally, `cagent` has first-class support for DMR.

## Step 1: Pull a Model

Before running an agent with a local model, make sure you have the model available locally:

```bash
docker model pull ai/smollm2
```

This will download the model to your local machine.

## Step 2: Create an Agent Using DMR

Now create an agent that uses this local model:

```bash
cat > dmr_pirate_agent.yaml << 'EOF'
agents:
  root:
    model: dmr/ai/smollm2
    instruction: Talk like a pirate
EOF
```

## Step 3: Run the Agent

Run the agent:

```bash
cagent run dmr_pirate_agent.yaml
```

You should see the agent using your local model. This is great for:

- **Privacy**: Keep your data local
- **Offline work**: No internet connection needed
- **Cost savings**: No API fees
- **Experimentation**: Try different models without API limits

## Other Available Models

You can explore more models on [Docker Hub](https://hub.docker.com/u/ai). Some popular options include:

- `ai/llama3.2:3b` - Small but capable Llama model
- `ai/phi3` - Microsoft's efficient model
- `ai/gemma2` - Google's Gemma 2 model
- `ai/qwen2` - Alibaba's multilingual model

To use any of these, just pull the model and update your YAML:

```bash
# Pull the model
docker model pull ai/llama3.2:3b

# Create an agent
cat > dmr_llama_agent.yaml << 'EOF'
agents:
  root:
    model: dmr/ai/llama3.2:3b
    instruction: You are a helpful assistant
EOF

# Run it
cagent run dmr_llama_agent.yaml
```

## Combining DMR with Tools

You can also combine local models with all the tools we've learned about:

```bash
cat > dmr_developer.yaml << 'EOF'
version: "2"

agents:
  root:
    model: dmr/ai/smollm2
    instruction: You are a developer assistant
    add_environment_info: true
    toolsets:
      - type: filesystem
      - type: shell
      - type: todo
EOF
```

Run it:

```bash
cagent run dmr_developer.yaml
```

## Next Steps

Congratulations! You've now learned how to use local models with Docker Model Runner. This gives you complete flexibility to work offline, keep data private, and experiment without API costs.

In the final step, we'll wrap up what you've learned and explore next steps.
