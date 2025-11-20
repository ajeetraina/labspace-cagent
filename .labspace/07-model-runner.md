## Step 7: Using Docker Model Runner (DMR)

Docker Model Runner (DMR) makes it easy to run AI models locally. In this labspace, we've pre-configured a Model Runner container.

### Create/Pull a Model

First, create (pull) a model:
```bash
curl -X POST http://model-runner:12434/models/create \
  -H "Content-Type: application/json" \
  -d '{"model": "ai/smollm2"}'
```

### List Available Models
```bash
curl http://model-runner:12434/models
```

### Test the Model Directly
```bash
curl http://model-runner:12434/engines/llama.cpp/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "ai/smollm2",
    "messages": [
      {"role": "user", "content": "Hello, who are you?"}
    ]
  }'
```

### Create an Agent Using DMR
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



## Next Steps

Congratulations! You've now learned how to use local models with Docker Model Runner. This gives you complete flexibility to work offline, keep data private, and experiment without API costs.

In the final step, we'll wrap up what you've learned and explore next steps.
