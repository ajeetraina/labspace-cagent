# Step 7: Using Docker Model Runner (DMR)

Docker Model Runner (DMR) makes it easy to manage, run, and deploy AI models using Docker. Designed for developers, Docker Model Runner streamlines the process of pulling, running, and serving large language models (LLMs) and other AI models directly from Docker Hub or any OCI-compliant registry.

> ⚠️ **Note:** Docker Model Runner requires Docker Desktop and works best on your local machine with GPU acceleration. This section is for reference only - try these commands on your local machine after the workshop.

## On Your Local Machine

First, ensure Docker Model Runner is enabled in Docker Desktop (Settings → Features in development → Enable Docker Model Runner).

### Step 1: Pull a Model

```bash
docker model pull ai/smollm2
```

### Step 2: Run the Model

```bash
docker model run ai/smollm2 "Hello, how are you?"
```

### Step 3: Create an Agent Using DMR

```bash
cat > dmr_pirate_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: dmr/ai/smollm2
    instruction: Talk like a pirate
EOF
```

### Step 4: Run the Agent

```bash
cagent run dmr_pirate_agent.yaml
```

## Benefits of Local Models

Running models locally with Docker Model Runner provides several advantages:

- **Privacy**: Keep your data local - nothing is sent to external APIs
- **Offline work**: No internet connection needed once the model is downloaded
- **Cost savings**: No API fees or token limits
- **Experimentation**: Try different models without worrying about costs

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

## Other Available Models

You can explore more models on [Docker Hub](https://hub.docker.com/search?q=ai%2F). Some popular options include:

- `ai/smollm2` - Small and fast, great for testing
- `ai/llama3.2:3B` - Small but capable Llama model
- `ai/phi4` - Microsoft's efficient model
- `ai/gemma3` - Google's Gemma 3 model
- `ai/qwen2.5` - Alibaba's multilingual model

To use a different model:

```bash
# Pull the model
docker model pull ai/llama3.2:3B

# Create an agent
cat > dmr_llama_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: dmr/ai/llama3.2:3B
    instruction: You are a helpful assistant
EOF

# Run it
cagent run dmr_llama_agent.yaml
```

## Learn More

For more information about Docker Model Runner, visit the [official documentation](https://docs.docker.com/ai/model-runner/).

## Next Steps

Congratulations! You've now learned about Docker Model Runner for running local models. This gives you complete flexibility to work offline, keep data private, and experiment without API costs.

In the final step, we'll wrap up what you've learned and explore next steps.
