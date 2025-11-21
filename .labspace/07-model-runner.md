# Step 7: Using Docker Model Runner (DMR)

Docker Model Runner (DMR) makes it easy to manage, run, and deploy AI models using Docker. Designed for developers, Docker Model Runner streamlines the process of pulling, running, and serving large language models (LLMs) and other AI models directly from Docker Hub or any OCI-compliant registry.

## Model Runner in This Labspace

For this labspace, we've pre-configured a Model Runner container that's accessible at `http://model-runner:8080`.

**Note:** The Model Runner is defined in `.labspace/compose.override.yaml`:
```yaml
model-runner:
  image: docker/model-runner:latest
  container_name: labspace-model-runner
  ports:
    - "12434:8080"
  volumes:
    - model-runner-data:/models
  restart: unless-stopped
```

## Step 1: Verify Model Runner is Running

First, check that the Model Runner is accessible:
```bash
curl http://model-runner:8080/models
```

You should see an empty list of models: `{"data":[]}`

## Step 2: Pull a Model

Pull a small model to test with:
```bash
curl http://model-runner:8080/models/create -X POST -d '{"from": "ai/smollm2"}'
```

This will download the model. It may take a few minutes depending on your connection.

Verify the model is available:
```bash
curl http://model-runner:8080/models
```

## Step 3: Test the Model Directly

Test the model using the OpenAI-compatible API:
```bash
curl http://model-runner:8080/engines/llama.cpp/v1/chat/completions -X POST \
  -H "Content-Type: application/json" \
  -d '{
    "model": "ai/smollm2",
    "messages": [
      {"role": "system", "content": "You are a helpful assistant."},
      {"role": "user", "content": "Hello, how are you?"}
    ]
  }'
```

## Step 4: Create an Agent Using DMR

Now let's create a cagent that uses this local model:
```bash
cat > dmr_pirate_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: ai/smollm2
    instruction: Talk like a pirate
    model_config:
      base_url: http://model-runner:8080/engines/llama.cpp/v1
EOF
```

## Step 5: Run the Agent
```bash
cagent run dmr_pirate_agent.yaml
```

Try asking it something and watch it respond in pirate speak!

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
    model: ai/smollm2
    instruction: You are a developer assistant
    add_environment_info: true
    model_config:
      base_url: http://model-runner:8080/engines/llama.cpp/v1
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
curl http://model-runner:8080/models/create -X POST -d '{"from": "ai/llama3.2:3B"}'

# Update your agent YAML to use the new model
cat > dmr_llama_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: ai/llama3.2:3B
    instruction: You are a helpful assistant
    model_config:
      base_url: http://model-runner:8080/engines/llama.cpp/v1
EOF

# Run it
cagent run dmr_llama_agent.yaml
```

## API Reference

Here are some useful Model Runner API endpoints:

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/models` | GET | List all available models |
| `/models/create` | POST | Pull/create a new model |
| `/models/{namespace}/{name}` | GET | Get info about a specific model |
| `/models/{namespace}/{name}` | DELETE | Delete a model |
| `/engines/llama.cpp/v1/chat/completions` | POST | OpenAI-compatible chat API |
| `/metrics` | GET | Prometheus metrics |

## Next Steps

Congratulations! You've now learned how to use local models with Docker Model Runner. This gives you complete flexibility to work offline, keep data private, and experiment without API costs.

In the final step, we'll wrap up what you've learned and explore next steps.
