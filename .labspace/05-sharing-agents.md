## Step 5: Sharing Agents with Docker Registry

By now you should have a couple of agents, and maybe one that already does something useful. Wouldn't it be nice if we could share agents with others?

Sharing agents with cagent is extremely simple - you can push and pull agents to any OCI registry.

### Setting Up Your Docker Hub ID

Enter your Docker Hub username:

::variableDefinition[dockerhubid]{prompt="Enter your Docker Hub username"}

Make sure you're logged in to Docker Hub:
```bash
docker login
```

### Push Your Agent

Push your developer agent to Docker Hub:
```bash
cagent push developer.yaml {{dockerhubid}}/cagent-developer
```

### Pull an Agent

You can pull that agent on any machine:
```bash
cagent pull {{dockerhubid}}/cagent-developer
```

### Run Directly from Registry

Or run it directly without pulling first:
```bash
cagent run {{dockerhubid}}/cagent-developer
```

### Exercise

1. Push your developer agent to your Docker Hub account
2. Ask your neighbor for their Docker Hub ID
3. Pull and run their agent
4. Try some of the pre-built agents from the community:
```bash
   cagent run creek/pirate
```

## Next Steps

In Step 6, we'll explore multi-agent systems and sub-agents.
