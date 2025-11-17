# Step 3: Builtin Tools

An agent on its own can't do much other than just chat with you. To make agents
more useful we need to give the agent tools that it can use to gather
information and execute actions.

`cagent` has builtin tools and also support for MCP (Model Context Protocol).

## Builtin tools

`cagent` has 3 builtin generic agentic tools:

- `think`
- `todo`
- `memory`

### Think

The `think` tool gives the model the ability to stop and think. It's a kind of a
whiteboard that the model can use to jot down its thoughts.

Read more about the thinking tool
[here](https://www.anthropic.com/engineering/claude-think-tool).

To use this tool, add the `type: think` tool to the toolset of your agent. Create the file:

```bash
cat > think_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: You talk like a pirate
    toolsets:
      - type: think
EOF
```

Run the agent:

```bash
cagent run think_agent.yaml
```

You can try by asking your pirate agent: `Think before you answer, where is the treasure map?`

The keen-eyed might have noticed the `version: "2"` on the top of the YAML file
above. While `cagent` is in active development and breaking things, we _try_ not
to break user's existing agents. If there is no version defined in the YAML
file, the default version is `0`, the current latest version is `2`, always use
the latest version.

### Memory

The memory tool gives the agent the ability to remember things about the user.

Create this agent:

```bash
cat > memory_agent.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: You are a personal assistant
    toolsets:
      - type: memory
        path: ./memory.db
EOF
```

Run the agent:

```bash
cagent run memory_agent.yaml
```

Tell it your name and some random fact about you. Something like `I'm John and I'm a software engineer`. You should see it calling the memory tools to remember facts about you.

If you then quit cagent (press `Ctrl+C`) and start a new session with this agent, you can ask it what it knows about you. It should correctly look up its internal memory and tell you what it knows. For example: `Who am I?` or `What do I do for a living?`

### Todo

The `todo` toolset instructs the model to use its todo-tracking tools when it
needs to do a complex task. This tool can help the model keep in line while it's
working on a complex task.

The `todo` toolset can be useful for a developer agent. Breaking down a task
into smaller, more manageable pieces is how most developers work. Let's start
creating our developer agent right now. Create a `developer.yaml` file:

```bash
cat > developer.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: You are an amazing developer
    toolsets:
      - type: todo
EOF
```

Run the agent:

```bash
cagent run developer.yaml
```

_Note:_ Here we changed the model used from `gpt-4o-mini` to `gpt-4o`. Larger
models are usually better at tool calling and following instructions. Feel free
to test out as many models as you wish with different setups.

Try this agent, see how it _magically_ creates todo lists for its tasks and
loops until the todos are done.

### Development Related Builtin Tools

`cagent` agents can be given access to your shell or your filesystem to run
commands or read/write files or directories.

- `shell`
- `filesystem`

Let's enhance our developer agent with these tools. Update the `developer.yaml` file:

```bash
cat > developer.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: You are an amazing developer
    toolsets:
      - type: todo
      - type: shell
      - type: filesystem
EOF
```

This is basically all it takes to have a basic developer agent. Try it out:

```bash
cagent run developer.yaml
```

Ask it to: `write a snake game in an index.html file`

## Extra

This developer agent is a good start, but there's one piece missing that
would make it even better. It doesn't really know anything about the environment
it is working in. It _could_ find it out by running shell scripts, but that's
just wasting tokens. `cagent` can automatically add information about the
environment the agent is working on by adding `add_environment_info: true` to the
agent definition:

```bash
cat > developer.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: You are an amazing developer
    add_environment_info: true
    toolsets:
      - type: todo
      - type: shell
      - type: filesystem
EOF
```

Run this agent again:

```bash
cagent run developer.yaml
```

Ask it what the current directory is or if the current directory is a git repository. The agent now knows this without having to make any tool calls. Neat!

The information that is added to its system prompt with this is:

- the current directory
- the current platform (Windows, Linux, etc.)
- information if the current directory is a git repository or not

If you think that your agent needs to know what the current date is, you can add
this to its system prompt too:

```bash
cat > developer.yaml << 'EOF'
version: "2"

agents:
  root:
    model: openai/gpt-4o
    instruction: You are an amazing developer
    add_environment_info: true
    add_date: true
    toolsets:
      - type: todo
      - type: shell
      - type: filesystem
EOF
```

This can be useful if you are planning on asking your agent questions like `What were the commits made in this repository in the last day?`

## Best Practices

1. **Be Specific in Instructions**: Tell agents how to use their tools
   effectively
2. **Combine Complementary Tools**: Think + filesystem, shell + todo, etc.
3. **Consider Security**: Be careful with shell access and file permissions
4. **Start Simple**: Begin with one tool, then add more as needed

## Next Steps

In the next step we will take a look at ways we can make our developer agent
even more powerful thanks to MCP servers.

> **Note:** Before going to the next step, play around with your developer agent, try and make its system prompt fit _your_ needs.
