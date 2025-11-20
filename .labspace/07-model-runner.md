## Step 7: Using Docker Model Runner (DMR)

Docker Model Runner (DMR) makes it easy to run AI models locally. In this labspace, we've pre-configured a Model Runner container accessible at `http://model-runner:12434`.

### Pull a Model

First, pull a model (this may take a few minutes):
```bash
curl -X POST http://model-runner:12434/models/pull \
  -H "Content-Type: application/json" \
  -d '{"model": "ai/smollm2"}'
```

### Create an Agent Using DMR

Create an agent that uses the local model:
```bash
cat > dmr_pirate_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: dmr/ai/smollm2
    instruction: Talk like a pirate
    model_config:
      base_url: http://model-runner:12434/engines/llama.cpp/v1
EOF
```

### Run the Agent
```bash
cagent run dmr_pirate_agent.yaml
```

This is great for:
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
