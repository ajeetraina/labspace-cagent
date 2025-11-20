# Step 2: Getting Started with cagent

## First, let's set up your API keys

To use cagent, you'll need at least one API key from your preferred AI provider. Please provide the API key(s) you want to use:

::variableDefinition[openaikey]{prompt="Enter your OpenAI API key (or leave blank if not using OpenAI)"}

::variableDefinition[anthropickey]{prompt="Enter your Anthropic API key (or leave blank if not using Anthropic)"}

::variableDefinition[googlekey]{prompt="Enter your Google/Gemini API key (or leave blank if not using Google)"}

## Setting Up Your Environment

Now that you've provided your API key(s), let's set them in your environment. Copy and run the following commands in the terminal based on the provider(s) you're using:

```bash
# For OpenAI models (if you provided an OpenAI key)
export OPENAI_API_KEY=$$openaikey$$

# For Anthropic models (if you provided an Anthropic key)
export ANTHROPIC_API_KEY=$$anthropickey$$

# For Gemini models (if you provided a Google key)
export GOOGLE_API_KEY=$$googlekey$$
```

> **Note:** You only need to set the API keys for the providers you plan to use. At minimum, you need one valid API key to proceed.

## Your First Agent

Let's create the simplest possible agent. Run this command in the terminal to create `basic_hello.yaml`:

```bash
cat > basic_hello.yaml << 'EOF'
agents:
  root:
    model: openai/gpt-4o
    instruction: You talk like a pirate
EOF
```

Now let's run this amazing agent:

```bash
cagent run basic_hello.yaml
```

If everything is setup correctly, you should see the TUI and be able to ask a
question to your agent and it should answer in pirate speak.



To exit the TUI, press `Ctrl+C`.

## Choosing Different Models

If you don't have access to OpenAI, or want to use a different provider, you can easily swap the model in your YAML configuration. Visit [models.dev](https://models.dev) and look for models that exist for your provider.

`cagent` supports these providers:

- **`openai`** - OpenAI models (GPT-4, GPT-4o, etc.)
- **`anthropic`** - Anthropic models (Claude 3.5 Sonnet, etc.)
- **`google`** - Google Gemini models
- **`dmr`** - Use any local [Docker Model Runner](https://docs.docker.com/ai/model-runner/) model that you already have pulled locally

For example, to use Anthropic's Claude instead, create a new file:

```bash
cat > basic_hello_claude.yaml << 'EOF'
agents:
  root:
    model: anthropic/claude-3-5-sonnet-20241022
    instruction: You talk like a pirate
EOF
```

Then run it:

```bash
cagent run basic_hello_claude.yaml
```


## Next Steps

In Step 3, we'll dive deeper into agent configuration and explore the powerful built-in tools that cagent provides for file operations, web browsing, and more.
