# Step 7: Using Docker Model Runner (DMR)

Docker Model Runner (DMR) makes it easy to run AI models locally without needing external API keys. In this Labspace, we've pre-configured a Model Runner container that's ready to use!

## Why Use Docker Model Runner?

- **No API Keys Required**: Run models locally without OpenAI/Anthropic/Google keys
- **Privacy**: Keep your data on your machine
- **Offline Work**: No internet connection needed after pulling models
- **Cost Savings**: No API fees
- **Experimentation**: Try different models without limits

## Step 1: Pull a Model

First, let's pull the `smollm2` model (a small but capable model):

```bash
curl -X POST http://model-runner:12434/models/create \
  -H "Content-Type: application/json" \
  -d '{"model": "ai/smollm2"}'
```

This will download the model. Wait for it to complete.

## Step 2: Verify the Model is Available

List available models:

```bash
curl http://model-runner:12434/models
```

You should see `ai/smollm2` in the list.

## Step 3: Test the Model Directly

Let's test that the model is working:

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

You should see a JSON response with the model's reply.

## Step 4: Create an Agent Using DMR

Now let's create a cagent that uses this local model:

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

## Step 5: Run the Agent

```bash
cagent run dmr_pirate_agent.yaml
```

You should see the TUI and be able to chat with your pirate agent running entirely locally!

## Using DMR with Tools

You can combine local models with all the tools we've learned about:

```bash
cat > dmr_developer.yaml << 'EOF'
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
      - type: todo
EOF
```

Run it:

```bash
cagent run dmr_developer.yaml
```

## Other Available Models

You can explore more models on [Docker Hub AI](https://hub.docker.com/u/ai). To pull and use a different model:

```bash
# Pull a different model
curl -X POST http://model-runner:12434/models/create \
  -H "Content-Type: application/json" \
  -d '{"model": "ai/llama3.2:1b"}'
```

Then update your YAML to use it:

```bash
cat > dmr_llama_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: dmr/ai/llama3.2:1b
    instruction: You are a helpful assistant
    model_config:
      base_url: http://model-runner:12434/engines/llama.cpp/v1
EOF
```

```bash
cagent run dmr_llama_agent.yaml
```

## Combining DMR with External APIs

You can even create multi-agent systems that combine local and cloud models:

```bash
cat > hybrid_team.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: You are a team lead. Use your local assistant for quick tasks.
    sub_agents: [local_assistant]
  
  local_assistant:
    model: dmr/ai/smollm2
    description: A local assistant for quick tasks that don't need cloud AI
    instruction: You are a helpful local assistant
    model_config:
      base_url: http://model-runner:12434/engines/llama.cpp/v1
EOF
```

## Troubleshooting

If you encounter issues:

1. **Check if Model Runner is running**:
   ```bash
   curl http://model-runner:12434/models
   ```

2. **Check model is downloaded**:
   ```bash
   curl http://model-runner:12434/models | grep smollm2
   ```

3. **Re-pull the model if needed**:
   ```bash
   curl -X POST http://model-runner:12434/models/create \
     -H "Content-Type: application/json" \
     -d '{"model": "ai/smollm2"}'
   ```

## Next Steps

Congratulations! You've now learned how to use local models with Docker Model Runner. This gives you complete flexibility to work offline, keep data private, and experiment without API costs.

In the final step, we'll wrap up what you've learned and explore next steps.
